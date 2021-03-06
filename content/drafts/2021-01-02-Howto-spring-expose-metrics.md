+++
date        = "2021-01-02"
title       = "Canigó. Como exponer y capturar métricas de desempeño (Prometheus, Grafana)"
description = "Como exponer y capturar métricas de desempeño generadas a través de Spring y capturadas por servicios como Prometheus y Grafana."
section     = "howtos"
categories  = ["canigo"]
#key        = "GENER2021"
+++


## Introducción

El objetivo de este artículo es mostrar como exponer métricas de desempeño de aplicaciones, dentro de un proyecto generado con el framework Canigó, y capturadas por servicios de monitarización, alerta y visualización OpenSource tales como: [Prometheus](https://github.com/prometheus/prometheus) y [Grafana](https://github.com/grafana/grafana)

---
## Justificación

Uno de los retos de una aplicación es tener visibilidad de lo que ocurre durante su ejecución, sobre todo en escenarios de despliegues sobre contenedores. Los contenedores agregan velocidad y aumentan el rendimiento dentro del proceso de desarrollo, pero aportan complejidad adicional sobre la visibilidad del comportamiento y la gestión de alertas relacionadas. Es en este punto en el que las soluciones de monitoreo como Prometheus y Grafana pueden ayudar.

Cuando se utiliza un proyecto creado con [Canigó plugin](https://canigo.ctti.gencat.cat/canigo/entorn-desenvolupament/) que se basa en Spring, es posible exponer distintas métricas utilizando la librería `spring-boot-starter-actuator`, y es posible exportar las métricas en un formato compatible con Prometheus con esta libreria `micrometer-registry-prometheus`.

---
## Configuración

Para activar las métricas con `actuator`, y exponerla con un exporter de `Prometheus` en un proyecto creado con [Canigó plugin](https://canigo.ctti.gencat.cat/canigo/entorn-desenvolupament/), se requiere agregar algunas dependencias.

### Cambios en `pom.xml`

```xml
  ...

  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
    <exclusions>
      <exclusion>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-logging</artifactId>
      </exclusion>
    </exclusions>
  </dependency>

  <dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
  </dependency>

  ...
```

### Cambios en `application.yml` para configurar los `endpoints` de `actuator`

```yaml
server:
  port: 8099

# Actuator
  management:
    endpoint:
      shutdown:
        enabled: true
      beans:
        cache:
          time-to-live: 10s
      health:
        #### Show all details of health
        show-details: always
    endpoints:
      web:
        exposure:
          #### Activate all web endpoints
          include: "*"
```

Se requiere iniciar un contenedor con `Prometheus`

### Archivo de configuración `prometheus.yml`

```yaml
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: 'canigo_spring_prometheus'
    metrics_path: '/actuator/prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['xxx.xxx.xxx.xxx:8099']  ## xxx.xxx.xxx.xxx -> Servidor de la aplicación
```

```sh
docker run --rm -d -p 9090:9090 -v $PWD/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```

Se requiere iniciar un contenedor con `Grafana`

```sh
docker run --rm -d -p 3000:3000 --net=host grafana/grafana
```

---
## Uso 

### Pruebas 

> Para probar el funcionamiento se inicia la aplicación

```sh
  mvn spring-boot:run 
```

> Se prueban algunos servicios que muestran las métricas disponibles
  * /actuator
  * /actuator/health
  * /actuator/metrics/jvm.memory.used
  * /actuator/prometheus

![Spring Exposes Metrics Ejemplo 1](/images/howtos/2021-01-02_spring_expose_metrics_example1.gif)


> Se consulta en `Prometheus` que esté configurado el job configurado en `Prometheus`

![Spring Exposes Metrics Ejemplo 2](/images/howtos/2021-01-02_spring_expose_metrics_example2.gif)


> Se consulta en `Prometheus` las métricas

![Spring Exposes Metrics Ejemplo 3](/images/howtos/2021-01-02_spring_expose_metrics_example3.gif)


> Se configura `Grafana` y se consultan las métricas

![Spring Exposes Metrics Ejemplo 4](/images/howtos/2021-01-02_spring_expose_metrics_example4.gif)


---
## Conclusión

 * Es posible en base a configuración exponer métricas en proyecto creado con [Canigó plugin](https://canigo.ctti.gencat.cat/canigo/entorn-desenvolupament/).
