+++
date = "2019-01-02"
title = "Binaris"
description = "Per la distribució d'artefactes a CPD"
sections = "SIC"
aliases = [
  "/noticies/2017-07-05-SIC-Gestio-binaris/"
]
taxonomies = []
weight = 3
+++

## Introducció

El sistema de gestió de binaris del SIC s'encarrega de:

* Emmagatzemar els binaris que carreguen els Release Managers (o carregats pel sistema d'integració continua per a entorns amb desplegament no automatitzat) i deixar-los a disposició del CPD encarregat de desplegar-los.
* Emmagatzemar binaris i arxius pesats que no són permesos dins de GIT i que, per algun motiu, no es poden emmagatzemar al Nexus (material multimèdia pesat, binaris que no són dependències, etc.) per a aplicacions que repositen codi font.

## Operativa principal

### Accés al servei

Podrà accedir mitjançant el següent enllaç: https://bin.sic.intranet.gencat.cat

<br/>
<CENTER>![Binaris](/images/news/SIC-GestioBinarisPortal.PNG)</center>
<br/>

### Pujada d'artefactes

En accedir al servei es mostra una pàgina de benvinguda amb l'acció **Dipositar artefactes al SIC**. <br/>
Aquesta acció és només accessible per als lots d'aplicacions i redirigieix a l'usuari al job de Jenkins de pujada d'artefactes al SIC. Es tracta d'un mateix job per a tots els Release Managers de tots els lots. No registra traces amb informació sensible i transmet els links amb les ubicacions dels manuals i artefactes per correu electrònic a l'usuari que ha iniciat l'execució.

Aquest job sol·licita la següent informació:

* Codi d'aplicació: obligatori (nombre de 4 xifres que es correspon amb el codi de diàleg)
* Projecte: obligatori
* Versió: obligatori
* Arxiu de binaris: obligatori (arxiu de binaris que desitja dipositar)
* Descomprimir ZIP: indica si l'arxiu de binaris caldrà descomprimir-lo un cop pujat
* Arxiu de documentació: opcional (arxiu de documentació que desitja associar)

El job validarà que el codi d'aplicació existeixi i que l'usuari disposi dels corresponents permissos. Si s'especifica un codi d'aplicació - projecte - versió ja pujada anteriorment, el sistema sobreescriurà el seu contingut.

<br/>
**Avís**: A partir del dia 24/01/2019 s'activarà el mode restrictiu en la validació que la pujada d'un nou binari vingui acompanyada de l'actualització de la versió del codi font del projecte. Només estaran exemptes les aplicacions que disposin d'una excepció aprovada en la custodia de codi. Fins aleshores, el control es realitza en mode informatiu permetent continuar.

### Recuperació d'artefactes

En accedir al servei es mostra una pàgina de benvinguda amb l'acció **Recuperar artefactes del SIC**. <br/>
Aquesta acció és accessible tant pels Release Managers de tots els lots com per a tots els administradors de tots els CPDS/LdT. Els accessos són securitzats (requereixen autenticació amb credencials GICAR i cada codi d'aplicació requereix autorització per Lot/CPD/LdT). <br/>
Aquesta opció és la que utilitzarà CPD/LdT per al desplegament de les aplicacions. Aquests accediran al servei en mode lectura a través del frontal web.

<br/>
Per a més detalls, teniu tota la informació disponible al [Manual d'Usuari](http://canigo.ctti.gencat.cat/related/sic/2.0/manual-usuari.pdf).
