+++
date = "2019-01-02"
title = "Custodia de codi font"
description = "GitLab és l'eina implantada al SIC per la custodia de codi font"
aliases = [
    "/howtos/2017-07-Howto-Migrar-repositori-SVN-a-repositori-GIT/"
]
sections = "SIC"
taxonomies = []
toc = true
weight = 1
+++

## Introducció

![GitLab](/related/sic/serveis/gitlab-logo.png "GitLab")

**GitLab** és l'eina implantada al SIC per la custodia de codi font. Es tracta d'un servei web de control de versions i desenvolupament de software col·laboratiu basat en Git. A més de gestor de repositoris, el servei ofereix també allotjament de Wikis i un sistema de seguiment d'errors, tot publicat sota una llicència de codi obert. 

Aquest repositori no és un entorn de desenvolupament, per lo que només les persones assignades com a Release Managers seran les encarregades de **consolidar el codi i lliurar-lo**. Aquest codi font ja haurà d’estar validat en entorns de desenvolupament i es lliurarà quan es decideixi distribuir als entorns dels serveis TIC centrals.

## Funcionament

### Accés al servei

Podrà accedir mitjançant el següent enllaç: https://git.intranet.gencat.cat <br/>
Haurà d'autenticar-se amb de les seves credencials d'accés **GICAR**. Els Release Manager, responsables de lot i tècnics de CPD disposaran d'accés al servei. Si no disposa d'accés, haurà de sol·licitar-ho al seu responsable. <br/>

### Creació de comptes d'usuari i permisos d'accés

Per a poder accedir al servei caldrà disposar d'un usuari GICAR operatiu (amb l'adreça de correu associada) i crear el compte corresponent. Per a fer-ho, haurà d'introduir l'identificador d'usuari i contrasenya i, en cas de tractar-se d'un nou compte, el sistema el redirigirà a la plana de perfil per a que pugui dur a terme el procés d'alta. <br/>
Si no disposa d'accés als grups i projectes s'haurà d'adreçar als **Release Managers** del codi de diàleg o al responsable del lot per a que l'incloguin com a membre del projecte o projectes que es considerin. A partir d’aquest moment, ja podrà gestionar el codi font i a l'endemà l'usuari passarà a ser un Release Manager a tots els efectes, disposant d'accés a tots els serveis del SIC per al codi de diàleg corresponent.

Per a més informació: [Autoservei d'usuaris] (/sic-serveis/autoservei-usuaris/)

### Estructura de grups i projectes

Totes les aplicacions que recull l'inventari d'aplicacions disposen automàticament d'un grup al GitLab per al codi de diàleg del CTTI corresponent (per exemple: https://git.intranet.gencat.cat/0192). Dins el grup assignat, es poden crear tants projectes com conjunts de codi susceptibles de ser versionats de forma independent. Pot tractar-se d'una **llibreria, un microservei, un mòdul o un programa sense fragments independents**.
<br/>
Per accedir a la vista de grups i projectes, ho podrà fer des del menú d'opcions generals (botó <img style="display:inline" src="/images/news/icone_menu_gitlab.PNG" alt="icone menu gitlab"/> situat a la part superior esquerra).
<br/>
No es poden incloure binaris de llibreries ni d’altres mòduls ni executables (JAR, WAR, EAR, DLL, EXE…) i la mida màxima dels arxius serà de 25MB. Es podrà fer ús del servei de gestió de binaris.

Per a més informació: [Binaris] (/sic-serveis/binaris/)

### Creació de nous projectes

Per a crear nous projectes caldrà dirigir-se al grup del codi de diàleg i prémer l'acció "New project". Haurà d'indicar el nom del projecte i seleccionar el nivell de visibilitat "Private".

<CENTER>![Nou projecte](/related/sic/new_project.PNG)</center>
<br/>

### Integració contínua

En publicar una nova versió del codi font sobre la branca MASTER, automàticament es dispararà la tasca de construcció i desplegament associada en el sistema d'Integració Contínua (Jenkins). En aquest sentit, cal tenir pressents algunes premisses:

* Els repositoris poden tenir tantes branques com siguin necessàries, però sempre s’haurà d’incloure la branca **MASTER** i el contingut d’aquesta branca serà amb el que treballarà la tasca associada.
* Cal proporcionar processos de construcció d’artefactes **independents de les màquines i plataformes** on s’executen, de forma que siguin aplicables tant en els entorns de desenvolupament com en els entorns del SIC.
Per a més informació: [Integració continua] (/sic-serveis/ci/)

## Migració repositoris SVN a Git

Per aplicacions que encara no hagin migrat el seu codi font cap a l'eina actual de custodia de codi font del SIC (GitLab), podran fer-ho si s'acompleixen els següents requisits:

* Utilitzar un sistema operatiu Linux i tenir instal·lats els paquets git, git-svn i svn.
* El repositori a migrar **no pot contenir binaris** (.jar, .war, .ear, .dll, .exe, carpeta "node_modules") **ni arxius de mida superior a 25 MB**.
Actualment el repositori original a SVN es troba disponible en mode lectura per a poder accedir a l'historial. Per a dur a terme la migració al nou sistema, caldrà seguir les següents passes:

<br/>
#### 1. Eliminació de binaris

El Git del SIC té restriccions alhora de pujar fitxers binaris, fet que pot provocar errors en el procés de migració. Aquests repositoris que tinguin binaris no es podran migrar al Git, romandran en el SVN en mode lectura. Per aquests casos, per començar a treballar amb Git s'haurien de seguir els següents passos:

* Descarregar el darrer tag del SVN
* Eliminar els binaris. Podem tenir dos casos:
	- Binaris prescindibles: es poden generar a partir de codi font de l'aplicació, i per tant, no cal conservar-los
	- Binaris no prescindibles: s'han d'ubicar en el [servei de custodia de binaris](/sic-serveis/binaris/) o al [Nexus](https://hudson.intranet.gencat.cat/nexus/) del SIC.
* Inicialitzar el repositori Git local (git init)
* Fer el commit, push i tag d'aquesta versió inicial al Git
Un cop finalitzat aquest procés al Git es disposarà del tag més recent. L'històric es mantindrà al SVN en mode lectura.

<br/>
#### 2. Obtenir els autors

En primer lloc, s'ha de consultar qui ha modificat els arxius del SVN. Per fer això s'ha preparat un [job de Jenkins] (https://hudson.intranet.gencat.cat/hudson/job/MIGRACIO_GENERAR_AUTORS/).

S'ha d'executar aquest job informant els següents paràmetres:

	DEPARTAMENT -> acrònim del departament (En una ruta com http://svn.intranet.gencat.cat/ctt/0192/) seria ctt )
	CODI_DIALEG -> codi de diàleg (Per exemple 0192)

Una vegada executat el job envia per email el fitxer author.txt a l'usuari que ha executat el job.

S'ha de descarregar i desar el fitxer a la carpeta desitjada, en aquest howto /migració.

A més d'obtenir el fitxer author.txt, s'ha de descarregar el següent [fitxer] (/related/sic/howto/unknown_author.zip) i descomprimir-lo en la mateixa carpeta (/migracio). Aquest zip conté un procés per a evitar errors en el següent pas.

<br/>
#### 3. Obtenir les dades del SVN

Per a obtenir les dades del SVN que s'han de migrar, dintre de la carpeta /migracio (on es troba el fitxer author.txt) executar:

	git svn clone --authors-file=author.txt --authors-prog=unknown_author.sh http://svn.intranet.gencat.cat/$1/$2/$3 --no-metadata -s $4

On s'ha de substituir les variables segons:

	$1 -> acrònim del departament (En una ruta com http://svn.intranet.gencat.cat/ctt/0192/) seria ctt )
	$2 -> codi de diàleg (Per exemple 0192)
	$3 -> path de la carpeta que conté les carpetes tags, trunk i branches. En cas que estiguin a l'arrel no posar res.
	$4 -> nom de la carpeta que es crea al sistema de fitxers locals on es deixa el codi

<br/>
#### 4. Tractament del codi SVN

Accedir a la carpeta que s'ha creat en el punt anterior ($4) i abans de pujar el codi a Git executar les següents comandes:

	cp -Rf .git/refs/remotes/origin/tags/* .git/refs/tags/
	rm -Rf .git/refs/remotes/origin/tags
	cp -Rf .git/refs/remotes/* .git/refs/heads/
	rm -Rf .git/refs/remotes
	for i in $(ls .git/refs/tags/ -1 | grep '@'); do rm ".git/refs/tags/$i"; done
	
<br/>
#### 5. Crear projecte en Git

Per a crear el projecte en Git heu d'accedir a https://git.intranet.gencat.cat anar al grup del vostre codi de diàleg i prémer l'acció "New Project".

<br/>
#### 6. Pujar el codi a Git

A la carpeta on s'ha desat el codi del SVN executar:

	git remote add origin https://$1@git.intranet.gencat.cat/$2/$3.git

On s'ha de substituir les variables segons:

	$1 -> usuari GitLab
	$2 -> codi de diàleg (Per exemple 0192)
	$3 -> Nom del projecte de GitLab

Per finalitzar es puja el codi a Git

	git push origin --all
	git push --tags
	
És molt important analitzar el log que mostra Git per pantalla per assegurar que no hi ha errors i s'han migrat tant el trunk com tots els tags.

<br/><br/><br/>
Si voleu més informació podeu consultar la secció de [Manuals](https://canigo.ctti.gencat.cat/sic/manuals). <br/>
Si teniu qualsevol dubte o problema assegureu-vos de no trobar resposta a les [FAQ] (/sic/faq) i utilitzeu el canal de [Suport] (/sic/suport) o contacteu amb l'Oficina Tècnica Canigó CTTI a través del correu electrònic: **oficina-tecnica.canigo.ctti@gencat.cat**.
