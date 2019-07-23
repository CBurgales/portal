+++
date        = "2019-07-23"
title       = "Resolució problema optimització Spring a Canigó 2"
description = "Com resoldre el problema congeut d'optimització d'obtenció de Beans de Spring a Canigó 2"
section     = "howtos"
categories  = ["canigo"]
key         = "AGOST2019"
+++

### A qui va dirigit

Aquest how-to va dirigit a tots aquells perfils tècnics que utilitzin Canigó 2 i/o Spring anterior a la versió 3.1.2

### Introducció al problema

El Juliol de 2019 es va reportar un problema de rendiment a aplicacions que utilitzaven Canigó 2

El problema s'originava si hi havia una alta utilització del component **net.gencat.ctti.canigo.services.web.taglib.util.TagUtil** de la llibreria **canigo-services-web** de Canigó 2

El problema generava un bloqueig als threads del servidor d'aplicacions i acabava desestabilitzant el sistema

Un exemple de traça del servidor d'aplicacions amb  el thread bloquejat:

```
<Jul 17, 2019 12:35:42 PM CEST> <Error> <WebLogicServer> <BEA-000337> <[STUCK] ExecuteThread: '44' for queue: 'weblogic.kernel.Default (self-tuning)' has been busy for "668" seconds working on the request "Http Request Information: weblogic.servlet.internal.ServletRequestImpl@1fc30e99[GET XXX.jsp]<br>
", which is more than the configured time (StuckThreadMaxTime) of "600" seconds in "server-failure-trigger". Stack trace:<br>
    org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:141)<br>
    org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:246)<br>
    org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:160)<br>
    org.springframework.beans.factory.support.AbstractBeanFactory.getTypeForFactoryBean(AbstractBeanFactory.java:1145)<br>
    org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.getTypeForFactoryBean(AbstractAutowireCapableBeanFactory.java:569)<br>
    org.springframework.beans.factory.support.AbstractBeanFactory.isTypeMatch(AbstractBeanFactory.java:439)<br>
    org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanNamesForType(DefaultListableBeanFactory.java:174)<br>
    org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeansOfType(DefaultListableBeanFactory.java:243)<br>
    org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeansOfType(DefaultListableBeanFactory.java:237)<br>
    org.springframework.context.support.AbstractApplicationContext.getBeansOfType(AbstractApplicationContext.java:814)<br>
    net.gencat.ctti.canigo.services.web.spring.util.WebApplicationContextUtils.getBeanOfType(WebApplicationContextUtils.java:45)<br>
    net.gencat.ctti.canigo.services.web.taglib.util.TagUtil.copyConfiguration(TagUtil.java:163)<br>
    net.gencat.ctti.canigo.services.web.struts.taglib.forms.fields.OptionsFieldTag.doEndTag(OptionsFieldTag.java:85)<br>
    net.gencat.ctti.canigo.services.web.struts.taglib.forms.fields.helpers.SelectFieldTagHelper.generateOptions(SelectFieldTagHelper.java:174)<br>
    net.gencat.ctti.canigo.services.web.struts.taglib.forms.fields.SelectFieldTag.doAfterValue(SelectFieldTag.java:330)<br>
    fr.improve.struts.taglib.layout.field.SelectTag.doEndEditField(SelectTag.java:238)<br>
    fr.improve.struts.taglib.layout.field.SelectTag.doEndEditMode(SelectTag.java:229)<br>
    fr.improve.struts.taglib.layout.field.AbstractModeFieldTag.doEndLayoutTag(AbstractModeFieldTag.java:110)<br>
    net.gencat.ctti.canigo.services.web.struts.taglib.forms.fields.SelectFieldTag.doEndLayoutTag(SelectFieldTag.java:352)<br>
    fr.improve.struts.taglib.layout.LayoutTagSupport.doEndTag(LayoutTagSupport.java:47)<br>
    ```

### Detall del problema

El problema radica en la forma de llistar i retornar un Bean de Spring. A la versió 2.0.5 de Spring que s'utilitza a Canigó 2, té el següent bug registrat:

https://github.com/spring-projects/spring-framework/issues/14083

En el bug s'apuntava que la forma com Spring retornava un Bean no era òptim i era necessari incorporar una caché

El bug va ser resolt per la versió 3.1.2 de Spring afegint una caché

Es pot consultar el detall de la resolució al següent commit de Spring:

https://github.com/spring-projects/spring-framework/commit/4c7a1c0a5403b35dd812dae1f2a753538928bb32

### Solució al problema

Hi ha 3 formes de resoldre el problema: optimitzant l'obtenció dels beans de Spring als components de Canigó (es mitiga el problema però no es soluciona), afegint una caché al mètode centralitzat de Canigó d'obtenció de Beans o afegint una caché a nivell de Spring

Des de CS Canigó recomenaríem la sol·lució d'afegir una caché a nivell de Spring

#### Solució 1: optimitzant l'obtenció dels beans de Spring als components de Canigó

#### Solució 2: afegint una caché al mètode centralitzat de Canigó d'obtenció de Beans

#### Solució 3: afegint una caché a nivell de Spring


### Conclusió

Si s'està utilitzant Spring anterior a la versió 3.1.2 es necessari revisar l'aplicació per si aplica aquest bug i quina sol·lució d'hauria d'aplicar

Des de CS Canigó es recomana la solució d'afegir una caché a nivell de Spring

Si a més informació preferiblement podeu obrir tiquet via [JIRA CSTD](https://cstd.ctti.gencat.cat/jiracstd/projects/CAN) o en cas de no disposar de permisos d’accés, a la bústia del CS Canigó (oficina-tecnica.canigo.ctti@gencat.cat)




