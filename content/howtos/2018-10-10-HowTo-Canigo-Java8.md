+++
date        = "2018-10-10"
title       = "Migració Canigó 3 de Java 7 a Java 8"
description = "En aquest HowTo es donen unes pautes per migrar aplicacions Canigó 3 de Java 7 a Java 8"
sections    = ["howtos"]
categories  = ["canigo"]
key         = "NOVEMBRE2018"
+++

## Audiència

Aquest how-to va dirigit principalment al personal tècnic (desenvolupadors i arquitectes) que vulguin actualitzar la versió de Java 7 a 8 en una aplicació Canigó 3.

## Context

Els canvis "grans" de versions de Java acostumen a implicar canvis en les aplicacions. L'objectiu d'aquesta guia és facilitar la migració a Java 8 de les aplicacions Canigó 3 que facin servir encara Java 7.

## Canigó 3

La versió de Canigó 3.1.0 publicada a finals de 2014 va introduir la versió 4 de Spring, la qual suporta _out-of-the-box_ Java 8. Per aquest motiu, les aplicacions que funcionin amb una versió de Canigó igual o posterior a la 3.1.0 no patiran problemes amb Java 8.

Per aplicacions Canigó 3.0.x, es recomana actualitzar a Canigó 3.1 seguint aquesta [guia](https://canigo.ctti.gencat.cat/noticies/2016-05-28-Canigo-Proces-actualitzacio-a-Canigo-3_1/).

A continuació s'especifica el canvi a realitzar al codi de l'aplicació Canigó 3.1 / 3.2 per forçar que les classes generades siguin Java 8 compatibles, impedint el desplegament en servidors d'aplicacions que corrin en versions anteriors de Java. 

### pom.xml

Per compilar el projecte amb Java 8, només cal modificar el fitxer `pom.xml` i fer el següent canvi:

                        <plugin>
                                <groupId>org.apache.maven.plugins</groupId>
                                <artifactId>maven-compiler-plugin</artifactId>
                                <configuration>
                                        <source>1.7</source>
                                        <target>1.7</target>
                                </configuration>
                        </plugin>

per

                        <plugin>
                                <groupId>org.apache.maven.plugins</groupId>
                                <artifactId>maven-compiler-plugin</artifactId>
                                <configuration>
                                        <source>1.8</source>
                                        <target>1.8</target>
                                </configuration>
                        </plugin>


## Canvis a Java 8

A continuació es presenta un llistat de canvis introduïts a la versió 8 de Java que poden afectar a les aplicacions que funcionaven amb la versió 7, i en funció de les quals s'hauria de revisar el codi de l'aplicació.

### DateFormat i SimpleDateFormat

En el cas que es faci ús del formatat de dates amb noms de mesos (p.e. gener, juny, etc.) poden patir problemes amb el canvi de versió de Java.

### java.net 

* La nova versió suposa canvis en el formatat de capçaleres `WWW-Authenticate` gestionades per la classe `HttpURLConnection`.
* Pot produïr-se _Security Exceptions_ quan s'obrin sockets de "servidor" amb el canvi de versió.

### LocaleServiceProvider

La selecció de `LocaleServiceProvider` ha canviat, i pot donar problemes en la gestió dels idiomes.

### Thread.stop(Throwable)

Aquest mètode llençarà sempre un `UnsupportedOperationException`.

### java.security

Les claus RSA de menys de 1024 bits deixaran de funcionar per defecte.

### java.lang

La selecció en sistemes Windows del directori HOME de l'usuari ha canviat, i pot presentar problemes amb aquesta nova versió.

### java.util.Collection

Els mètodes `removeAll()` i `retainAll()` llençaran un `NullPointerException` quan es passi NULLs.

### XML/JAXP

La selecció del proveïdor de XML pot donar problemes, seleccionant per defecte el que incorpora Java 8, i provocant inconsistències en el tractament de fitxers XML.

### Altres canvis

* _Warnings_ produïts en la compilació amb Java 7 pot generar _Errors_ amb la nova versió de Java.
* El tractament de genèrics s'ha fet més restrictiu i pot implicar problemes de compilació
* java.awt : La gestió de tecles pot donar problemes.
* Nashorn javascript engine: El motor de Javascript "Rhino" s'ha canviat pel nou motor "Nashorn", i en el cas que l'aplicació faci ús del motor "Rhino" s'haurà d'accedir a l'enllaç [2] de l'apartat _Informació addicional_ per corregir els canvis indicats.
* java.net.DatagramPacket ha canviat la signatura d'un constructor, i pot implicar que el codi no compili.

## Informació addicional

1. https://www.oracle.com/technetwork/java/javase/8-compatibility-guide-2156366.html
2. https://wiki.openjdk.java.net/display/Nashorn/Rhino+Migration+Guide
3. https://allegro.tech/2014/12/How-to-migrate-to-Java-8.html

