+++
date        = "2019-09-18"
title       = "Actualització archetype 1.6.4 i plugin Eclipse 1.7.6"
description = "S'ha publicat una nova versió del archetype i del plugin del eclipse de Canigó per generar projectes amb Canigó 3.4.1"
#sections    = ["Notícies", "home"]
#categories  = ["canigo"]
key         = "NOVEMBRE2019"
+++

Seguint l'objectiu de CS Canigó de proporcionar als desenvolupadors d'aplicacions les màximes eines útils per a la creació de projectes amb Canigó, s'ha actualitzat el _archetype_ i del _plugin_ de l'Eclipse de Canigó per generar projectes amb Canigó 3.4.1.

Aquesta actualització s'emmarca dins de l'abast de la **versió 3.4.1 del framework Canigó**.

Podeu consultar l'abast complet de la versió 3.4.1 a: [Abast Canigó 3.4.1](https://cstd.ctti.gencat.cat/jiracstd/issues/?jql=project%20%3D%20CAN%20AND%20fixVersion%20%3D%203.4.1)

## Introducció

Dins dels lliurables de Canigó hi consten el archetype i el plugin de l'Eclipse.

El archetype és una eina maven per a la creació de projectes seguint una plantilla. Així executant un _goal_ de maven obtenim un projecte Canigó amb una base preconfigurada preparada per a incorporar-li noves funcionalitats.

El plugin de l'Eclipse és una plugin desenvolupat pel IDE eclipse per afegir menús contextuals per a la creació de projectes Canigó utilitzant el archetype i per afegir mòduls, a un projecte creat, amb una preconfiguració ja donada. Així utilitzant l'Eclipse podem crear un projecte i afegir-l'hi els mòduls necessaris, obtenint un projecte preconfigurat, estalviant temps als desenvolupadors.

## Novetats archetype

S'ha publicat la versió 1.6.4 del archetype del Canigó per a generar projectes amb Canigó 3.4.1.  

La principal novetat és la utilització de fitxers _yml_ per a la configuració de propietats de l'aplicació i la incorporació dels canvis necessaris per Spring 5 i Spring Boot 2 publicats a Canigó 3.4.0 documentats a: [Canigó. Publicació versió 3.4.0](/noticies/2019-03-29-actualitzacio-canigo-3_4_0/).

## Novetats plugin Eclipse

La nova versió 1.7.5 del plugin utilitza la versió 1.6.4 del archetype de Canigó per generar projectes amb Canigó 3.4.1.

Per instal·lar o actualitzar la versió del plugin és necessari seguir els passos descrits a la secció "Instal·lació" del [Plugin Canigó per a Eclipse # Instal·lació](/canigo-download-related/plugin-canigo/#instal-lació)

## Documentació plugin eclipse

Està disponible la documentació dels lliurables de Canigó 3.4 a:

[Binaris de Canigó](/canigo/download/)

La documentació del plugin de l'Eclipse està disponible:

[Plugin Canigó per a Eclipse](/canigo-download-related/plugin-canigo/)

Detall dels canvis de Canigó 3.4.0:

[Canigó. Publicació versió 3.4.0](/noticies/2019-03-29-actualitzacio-canigo-3_4_0/)

La guia per a passar manualment projectes de Canigó 3.2 a Canigó 3.4 està a:

[Actualitzar aplicació de Canigó 3.2 a Canigó 3.4](/howtos/2019-03-Howto-Actualitzacio_Canigo3_2_Canigo3_4/)
