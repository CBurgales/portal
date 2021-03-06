+++
date        = "2021-01-02"
title       = "Canigó. Como utilizar Spring Retry para reintentar acciones"
description = "Como utilizar Spring Retry para reintentar acciones que generan una excepción al ejecutarse"
section     = "howtos"
categories  = ["canigo"]
#key         = "GENER2021"
+++


## Introducción

El objetivo de este artículo es mostrar el uso de la libreria [Spring Retry](https://mvnrepository.com/artifact/org.springframework.retry/spring-retry) dentro de un proyecto generado con el framework Canigó

---
## Justificación

Existen escenarios de negocios que requieren reintentar un proceso luego de una falla. Esto es útil por ejemplo, cuando se tratan errores temporales o transitorios como el acceso a un recurso externo.

_Spring Retry_ puede aplicarse de distintas formas: 

 * De forma declarativa: Utilizando solo anotaciones de Spring. Esta vía permite facil y rapidamente aplicar reintentos. 
 * De forma imperativa: Utilizando `RetryTemplate` y las clases del package `org.springframework.retry.backoff` de la librería [Spring Retry](https://mvnrepository.com/artifact/org.springframework.retry/spring-retry).
 * Una mezcla de las anteriores.

---
## Configuración

Para utilizar la opción de reintentos en un proyecto creado con [Canigó plugin](https://canigo.ctti.gencat.cat/canigo/entorn-desenvolupament/), se requiere agregar algunas dependencias de maven.

### Cambios en el archivo `pom.xml`

> Los proyectos generados con [Canigó plugin](https://canigo.ctti.gencat.cat/canigo/entorn-desenvolupament/) ya contienen la dependencia de `AspectJ`, que es requerida para que funcionen los reintentos. Si se quiere excluir esa dependencia para hacer mas eficiente la carga de la aplicación, se requiere agregar la libreria `aspectjweaver` como depdendencia. 

Dependencia requerida existente:
```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
  </dependency>
```

> Se necesitan las librerías [spring-retry](https://mvnrepository.com/artifact/org.springframework.retry/spring-retry) y [aspectjweaver](https://mvnrepository.com/artifact/org.aspectj/aspectjweaver)

```xml
  <dependency>
    <groupId>org.springframework.retry</groupId>
    <artifactId>spring-retry</artifactId>
  </dependency>
  <dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
  </dependency>
```

### Cambios en inicializador del proyecto `Application.java`

> Se necesita la anotación `@EnableRetry`

```java
  @SpringBootApplication
  @EnableRetry
  public class Application extends SpringBootServletInitializer {
    ...
  }
```


---
## Uso 
#### (de forma imperativa)

### Desarrollo 

> Configuración de propiedades: `application.yml`

```yaml
retry:
  maxAttempts: 5
  initialIntervalRetries: 1000
  maxIntervalRetries: 3000
  multiplierRetries: 2.0
```

> Se requiere crear un `bean` con la plantilla que contiene el comportamiento de los reintentos. Existen distintas estrategias de espera (back off) ya implementadas, tales como:

 * ExponentialBackOffPolicy: aumenta el período de espera para cada reintento en un conjunto dado usando la función Math.exp(double).
 * ExponentialRandomBackOffPolicy: elige un múltiplo aleatorio del intervalo de espera para cada reintento.

> Ejemplo de configuración del `bean`

```java
@Configuration
public class RetryConfig {
  @Value("${retry.maxAttempts:3}")
  private int maxAttempts;

  @Value("${retry.initialIntervalRetries:1000}")
  private long initialIntervalRetries;

  @Value("${retry.maxIntervalRetries:3000}")
  private long maxIntervalRetries;

  @Value("${retry.multiplierRetries:2.0}")
  private double multiplierRetries;

  @Bean
  public RetryTemplate retryTemplate() {
    RetryTemplate retryTemplate = new RetryTemplate();

    ExponentialRandomBackOffPolicy backOffPolicy = new ExponentialRandomBackOffPolicy();
    backOffPolicy.setInitialInterval(initialIntervalRetries);
    backOffPolicy.setMultiplier(multiplierRetries);
    backOffPolicy.setMaxInterval(maxIntervalRetries);
    retryTemplate.setBackOffPolicy(backOffPolicy);

    SimpleRetryPolicy retryPolicy = new SimpleRetryPolicy();
    retryPolicy.setMaxAttempts(maxAttempts);
    retryTemplate.setRetryPolicy(retryPolicy);

    return retryTemplate;
  }
}
```

> En la capa de servicio se requiere inyectar la plantilla `retryTemplate` y luego utilizar el método `execute` para envolver el contenido de los métodos que requieren reintentos.

```java
@Service("equipamentService")
public class EquipamentServiceImpl implements EquipamentService {
  @Inject
  private EquipamentRepository repository;

  @Inject
  private RetryTemplate retryTemplate;

  @Override
  public List<Equipament> findAll() {
    return retryTemplate.execute(arg -> repository.findAll());
  }

  ...
}
```

### Pruebas 

> Pruebas unitarias que plantean 2 escenarios: 

* Reintentar 3 veces, las 2 primeras generan una Excepción, y la ultima responde correctamente.
* Reintentar 5 veces, en todas las ejecuciones se genera una Excepción, al alcanzar el máximo de reintentos se muetra la Excepción.

> Configuración: `/test/resources/spring/canigo-core.xml`

```xml
  <context:component-scan base-package="cat.gencat.retrycanigoexample.service, cat.gencat.retrycanigoexample.config.retry" />
```

> Test:

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(locations = { "classpath:spring/canigo-core.xml" })
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class EquipamentRetryTemplateTest {
  @Inject
  private RetryTemplate retryTemplate;

  @Inject
  @Named("equipamentRetryService")
  private EquipamentService equipamentRetryService;

  @Inject
  @Named("equipamentRetryExceptionService")
  private EquipamentService equipamentRetryExceptionService;

  @Before
  public void settingUp() {
    assertNotNull(retryTemplate);
    assertNotNull(equipamentRetryService);
    assertNotNull(equipamentRetryExceptionService);
  }

  @Test
  public void testRetryWithFinalResponse() throws RetryException {
    List<Equipament> equipaments = retryTemplate.execute(arg -> this.equipamentRetryService.findAll());
    verify(equipamentRetryService, times(3)).findAll();
    assertFalse(equipaments.isEmpty());
  }
  
  @Test(expected = RetryException.class)
  public void testRetryWithFinalException() throws RetryException {
    List<Equipament> equipaments = retryTemplate.execute(arg -> this.equipamentRetryExceptionService.findAll());
    verify(equipamentRetryExceptionService, times(5)).findAll();
    assertTrue(equipaments.isEmpty());
  }

  @Configuration
  @EnableAspectJAutoProxy
  public static class SpringConfig {
    @Bean("equipamentRetryService")
    public EquipamentService equipamentRetryService() throws Exception {
      EquipamentService equipamentService = mock(EquipamentService.class);
      Equipament equipament = mock(Equipament.class);

      when(equipamentService.findAll())
        .thenThrow(new RetryException("Remote Exception 1"))
        .thenThrow(new RetryException("Remote Exception 2"))
        .thenReturn(Collections.singletonList(equipament));
      return equipamentService;
    }

    @Bean("equipamentRetryExceptionService")
    public EquipamentService equipamentRetryExceptionService() throws Exception {
      EquipamentService equipamentService = mock(EquipamentService.class);

      when(equipamentService.findAll())
        .thenThrow(new RetryException("Remote Exception 1"))
        .thenThrow(new RetryException("Remote Exception 2"))
        .thenThrow(new RetryException("Remote Exception 3"))
        .thenThrow(new RetryException("Remote Exception 4"))
        .thenThrow(new RetryException("Remote Exception 5"));
      return equipamentService;
    }
  }
}
```

![Spring Retry Ejemplo 1](/images/howtos/2021-01-02_spring_retry_example1.gif)


> Pruebas utilizando Postman

> Se modifica la capa de servicios para crear un método que genera excepciones durante las primeras ejecuciones, y luego consulta correctamente la capa de persistencia.

```java
@Service("equipamentService")
public class EquipamentServiceImpl implements EquipamentService {

  private static final Logger logger = LoggerFactory.getLogger(EquipamentServiceImpl.class);
  private int counter = 0;

  @Inject
  private EquipamentRepository repository;

  @Inject
  private RetryTemplate retryTemplate;

  @Override
  public Page<Equipament> findPaginated(final Integer page, final Integer rpp, final String sort, final String filter) {
    return retryTemplate.execute(arg -> {
      if(++counter != 5) {
        logger.error("findPaginated retry = {}", counter);
        throw new RetryException("findPaginated retry");
      }
      counter = 0;
      return findAllPaginated(page, rpp, sort, filter);
    });
  }

  private Page<Equipament> findAllPaginated(final Integer page, final Integer rpp, final String sort, final String filter) {
    GenericPredicateBuilder<Equipament> builder = new GenericPredicateBuilder<>(Equipament.class, "equipament");
    builder.populateSearchCriteria(filter);
    Pageable pageable = PageRequest.of(page - 1, rpp, getOrdenacio(sort));
    return repository.findAll(builder.build(), pageable);
  }

  ...
}
```

![Spring Retry Ejemplo 2](/images/howtos/2021-01-02_spring_retry_example2.gif)


---
## Uso 
#### (de forma declarativa)

### Desarrollo 

> Agregar un componente que contenga la anotación que indica la aplicación de reintentos. La anotación `@Retryable` se debe agregar a cada método.

```java
@Service
public class RetryService {
  @Inject
  private EquipamentService equipamentService;

  public void setEquipamentService(EquipamentService equipamentService) {
    this.equipamentService = equipamentService;
  }

  @Retryable(value = {RetryException.class}, maxAttempts = 5, backoff = @Backoff(1000))
  public List<Equipament> findAll() {
    return equipamentService.findAll();
  }

  @Retryable(value = {RetryException.class}, maxAttempts = 5, backoff = @Backoff(3000))
  public Page<Equipament> findPaginated(final Integer page, final Integer rpp, final String sort, final String filter) {
    return equipamentService.findPaginated(page, rpp, sort, filter);
  }
}
```

> La capa de servicios rest debe invocar el nuevo componente y no directamente la capa de servicio de negocio. Esto porque el proceso de reintento se maneja con aspectos (AspectJ) y el método que se anota que requiere reintentos tiene que ser distinto al método que genera la excepción que origina el reintento.

```java
@RestController
@RequestMapping("/equipaments")
public class EquipamentServiceController {
  @Inject
  private RetryService retryService;

  @GetMapping(produces = { MediaType.APPLICATION_JSON_VALUE })
  public ResponsePage<Equipament> findPaginated(
    @ApiParam(value = "Nombre de p&agrave;gines") @RequestParam(defaultValue = "1", value = "page", required = false) final Integer page,
    @ApiParam(value = "Valors per p&agrave;gina") @RequestParam(defaultValue = "10", value = "rpp", required = false) final Integer rpp,
    @ApiParam(value = "Camp Ordenaci&oacute; Ex(id,-municipi)") @RequestParam(defaultValue = "id,-municipi", value = "sort", required = false) final String sort,
    @ApiParam(value = "Filtre Ex(Municipi:Cambrils)") @RequestParam(value = "filter", required = false) final String filter) {

    Page<Equipament> equipaments = retryService.findPaginated(page, rpp, sort, filter);
    long offset = Long.valueOf(equipaments.getNumber()) * equipaments.getSize();
    Data<Equipament> data = new Data<>(equipaments.getTotalElements(), equipaments.getSize(),
      equipaments.getNumberOfElements(), offset, equipaments.getContent());

    return new ResponsePage<>(data);
  }
}
```

### Pruebas 

> Pruebas unitarias que plantean 2 escenarios:  

* Reintentar 3 veces, las 2 primeras generan una Excepción, y la ultima responde correctamente.
* Reintentar 5 veces, en todas las ejecuciones se genera una Excepción, al alcanzar el máximo de reintentos se muetra la Excepción.

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(locations = { "classpath:spring/canigo-core.xml" })
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class EquipamentRetryTemplateTest {
  @Inject
  private RetryService retryService;

  @Inject
  @Named("equipamentRetryService")
  private EquipamentService equipamentRetryService;

  @Inject
  @Named("equipamentRetryExceptionService")
  private EquipamentService equipamentRetryExceptionService;

  @Before
  public void settingUp() {
    assertNotNull(retryService);
    assertNotNull(equipamentRetryService);
    assertNotNull(equipamentRetryExceptionService);
  }

  @Test
  public void testRetryWithFinalResponse() throws RetryException {
    retryService.setEquipamentService(equipamentRetryService);
    List<Equipament> equipaments = this.retryService.findAll();
    verify(equipamentRetryService, times(3)).findAll();
    assertFalse(equipaments.isEmpty());
  }

  @Test(expected = RetryException.class)
  public void testRetryWithFinalException() throws RetryException {
    retryService.setEquipamentService(equipamentRetryExceptionService);
    List<Equipament> equipaments = this.retryService.findAll();
    verify(equipamentRetryExceptionService, times(5)).findAll();
    assertTrue(equipaments.isEmpty());
  }

  @Configuration
  @EnableRetry
  public static class SpringConfig {
    @Bean("equipamentRetryService")
    public EquipamentService equipamentRetryService() throws Exception {
      EquipamentService equipamentService = mock(EquipamentService.class);
      Equipament equipament = mock(Equipament.class);
      when(equipamentService.findAll())
        .thenThrow(new RetryException("Remote Exception 1"))
        .thenThrow(new RetryException("Remote Exception 2"))
        .thenReturn(Collections.singletonList(equipament));
      return equipamentService;
    }

    @Bean("equipamentRetryExceptionService")
    public EquipamentService equipamentRetryExceptionService() throws Exception {
      EquipamentService equipamentService = mock(EquipamentService.class);
      when(equipamentService.findAll())
        .thenThrow(new RetryException("Remote Exception 1"))
        .thenThrow(new RetryException("Remote Exception 2"))
        .thenThrow(new RetryException("Remote Exception 3"))
        .thenThrow(new RetryException("Remote Exception 4"))
        .thenThrow(new RetryException("Remote Exception 5"));
      return equipamentService;
    }
  }
}
```

![Spring Retry Ejemplo 3](/images/howtos/2021-01-02_spring_retry_example3.gif)


> Pruebas utilizando Postman

> Se modifica la capa de servicios para crear un método que genera excepciones durante las primeras ejecuciones, y luego consulta correctamente la capa de persistencia.

```java
@Service("equipamentService")
public class EquipamentServiceImpl implements EquipamentService {

  private static final Logger logger = LoggerFactory.getLogger(EquipamentServiceImpl.class);

  private int counter = 0;

  @Inject
  private EquipamentRepository repository;

  ...

  @Override
  public Page<Equipament> findPaginated(final Integer page, final Integer rpp, final String sort, final String filter) {
    if(++counter != 5) {
      logger.error("findPaginated retry = {}", counter);
      throw new RetryException("findPaginated retry");
    }
    counter = 0;
    GenericPredicateBuilder<Equipament> builder = new GenericPredicateBuilder<>(Equipament.class, "equipament");
    builder.populateSearchCriteria(filter);
    Pageable pageable = PageRequest.of(page - 1, rpp, getOrdenacio(sort));
    return repository.findAll(builder.build(), pageable);
  }

  ...
}
```

![Spring Retry Ejemplo 4](/images/howtos/2021-01-02_spring_retry_example4.gif)


---
## Conclusiones

 * Implementar la opción de reintentos a través de Spring permite crear procesos robustos que toleren fallos ocasionales sobre todo asociados al consumo de servicios externos.
 * Utilizar la implementación imperativa permite reutilizar patrones de reintentos sin tener que repetir anotaciones.
 * Utilizar la implementación basada en declaraciones, es mas sencillo de implementar, solo requiere de anotaciones, sin embargo, tiene algunas consideraciones, tales como: 
    - Se debe generar excepciones en el método que se está reintentando. Si se lanza la excepción en el método que contiene la anotación `@Retryable`, Retry no se ejecutará.
    - El método recuperable debe ser público y las llamadas a ese método deben ser de una clase externa para que Retry funcione.
