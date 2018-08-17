+ [1. Principis sobre el disseny d’aplicacions] (# principis-aplicacions)
    - 1.1 Segregació de funcions/responsabilitats
    - 1.2 Arquitectura desacoblada
    - 1.3 Orientació a serveis
    - 1.4 Maximitzar el rendiment
    - 1.5 Compatibilitat de versions
    - 1.6 Rendiment de les aplicacions
    - 1.7 Facilitat d’utilització
    - 1.8 Autentificació
    - 1.9 Model de qualitat
	- 1.10 Integració continua i custodia de codi
	- 1.11 Framework Canigó.

+ [2. Principis sobre la Tecnologia] (# principis-tecnologia)
    - 2.1 Continuïtat tecnològica
    - 2.2 Estabilitat del sistema
    - 2.3 Control de la diversitat tècnica
    - 2.4 Interoperabilitat
    - 2.5 Portabilitat tecnològica
    - 2.6 Reutilització tecnològica
    - 2.7 Radar Tecnològic
    - 2.8 Ubicació dels certificats
    - 2.9 Nomenclatura de dominis
    - 2.10 Nomenclatura de les infraestructures
    - 2.11 Servidors SMTP Transversals
    - 2.12 Accés a Internet des de xCAT
    - 2.13 Us de Cloud Públic
    - 2.14 Connectivitat de tercers.
    - 2.15 Comunicacions per sFTP
    - 2.16 Us d'Https
	- 2.17 Principis sobre la seguretat

+ [3. Principis sobre el cost i manteniment de les solucions] (# principis-cost)
    - 3.1 Prioritat dels principis
    - 3.2 Optimització de costos
    - 3.3 Benefici màxim al menor cost i risc possible
    - 3.4 Impacte d’actualització
    - 3.5 Protecció de la propietat intel·lectual
    - 3.6 Compliment de la Llei

+ [Principis de Dades] (# principis-de-dades) 
    - Les dades són un actiu
    - Accés a les dades
    - Responsable de dades
    - Seguretat de les dades

	
	# 1. Principis sobre el disseny d’aplicacions

<p>1.1 Segregació de funcions/responsabilitats, les aplicacions han d’estar estructuralment dividides en blocs independents per funcionalitats, processos de negoci o serveis, per tal d’evitar els monòlits.</p><p>Aquest principi és d’aplicació a totes les capes. Una aplicació tipus pot dividir-se fàcilment, per exemple, en els següents mòduls:</p>

<ul>
    <li>Públic general (internet)</li>
    <li>Col·laboradors externs (extranet)</li>
    <li>BackOffice (intranet)</li>
    <li>Processos batch</li>
    <li>Extraccions (ETL)</li>
</ul>
<span style="color:blue">
<p>1.2 Arquitectura desacoblada , permeten als components mantenir-se completament autònoms e independents:</p>

<ul>
    <li>1.2.1 Components autònoms (separació de la frontend/presentació i el backend/negoci), es desenvolupen i es despleguen independentment.</li>
    <li>1.2.2 Components independents, poden ser reemplaçats o actualitzats sense afectar a la resta de components.</li>
	<li>1.2.3 Desacoblament entre aplicacions: Evitar les relacions entre aplicacions que impedeixin el seu desacoblament.(Per exemple, relacions a nivell de base de dades, us de llibreries compartides, fitxers de configuració compartits). Totes les relacions es tenen que porta a terme mitjançant serveis Web.</li>
</ul>
</span>
<p>1.3 Arquitectura Orientada a Serveis, cada cop més, les aplicacions poden ser consumides externament o bé han d’integrar-se amb 3rs. Els backend han d’exposar la seva funcionalitat de negoci via serveis per facilitar-ho. 

<p>1.4 Maximitzar el rendiment, emmagatzemar a la memòria cau tot allò que sigui possible. Per fer-ho, utilitzar la tecnologia que millor s’adapti tant a client (html5 cache, localstorage , etc.) com a servidor (Redis, Varnish, Memcache, caché personalitzada, etc.).</p>

<p>1.5 Compatibilitat de versions, tenir present sempre la compatibilitat cap a enrere dels components, com per exemple: </p>

<ul>
    <li> Si s’exposa una API REST i s’actualitza un servei, que sigui compatible amb versions anteriors per a evitar actualitzacions innecessàries als consumidors i d’aquesta manera poder evolucionar el servei lliurement (sense dependre de calendaris o recursos de tercers).</li>
</ul>

<span style="color:blue">
<p>1.6 Rendiment de les aplicacions, el comportament de les  aplicacions han de satisfer les necessitats del negoci:</p>

<ul>
    <li>1.6.1 Concurrència, els sistemes permetran que els processos s’executin simultàniament i que puguin interactuar entre ells.</li>
    <li>1.6.2 Disponibilitat, el sistema tindrà la capacitat de desenvolupar les funcions per les quals va estar dissenyat en les condicions d’ús determinades.</li>
    <li>1.6.3 Elasticitat, els disseny de les aplicacions permetrà ampliar  o reduir la infraestructura per poder donar el servei desitjat sense posar en perill els requeriments d'estabilitat, rendiment, seguretat, governabilitat o de compliment normatiu i legal.</li>
    <li>1.6.4 Zero DownTime, el serveis no poden ser interromputs, s’han d’utilitzar els mecanismes necessaris per evitar situacions de parada de negoci.</li>
</ul>
</span>
<p>1.7 Facilitat d’utilització, les aplicacions seran fàcils d'usar. La tecnologia subjacent ha de ser transparent per als usuaris.</p>
<span style="color:blue">
<p>1.8 Autentificació, les aplicacions han d’autentificar els usuaris tenint en compte els següents models: </p>

<ul>
    <li>1.8.1 Col·lectiu Gencat: autentificació mitjançant “GICAR”.</li>
    <li>1.8.2 Col·lectiu Híbrid (Gencat/Empreses): autentificació mitjançant “GICAR”.</li>
    <li>1.8.3 Ciutadans: autentificació mitjançant “VÀlid” de AOC.</li>
</ul>
</span>
<p>1.9 Model de qualitat, a l’hora de dissenyar un sistema cal incorporar aspectes qualitatius al cicle de vida:</p>
<ul>
    <li>1.9.1 Proves per a verificar la qualitat o requisits no funcionals del sistema.</li>
    <li>1.9.2 Documentació detallada del projecte (descripció d’arquitectura, document funcional, manual de desplegament, manual d’explotació, …).</li>
    <li>1.9.3 Control de versions. El sistema, en el seu conjunt, ha de ser tractat com un producte amb les seves versions majors, menors, etc.</li>
    <li>1.9.4 Desplegament automatitzat, execució de proves automàtiques que verifiquin la instal·lació i integració contínua.</li>
</ul>
<span style="color:blue">
<p> 1.10 Integració continua i custodia de codi:
<ul>
	<li> Totes les aplicacions tenen que tenir custodiat el codi font a algun dels repositoris oficials de la Generalitat.</li>
	<li> Totes les noves aplicacions que es donin d'alta tenen que esta preparades per ser desplegades de forma automàtica utilitzant les eines proporcionades per el <a href="https://canigo.ctti.gencat.cat/sic-documentacio/">SIC 2.0.</a></li>
	
</ul>	

<p> 1.11 Es recomana l'ús del <a href="https://canigo.ctti.gencat.cat/canigo/framework/">Framework Canigó.</a></p>

</span>	


# 2. Principis sobre la Tecnologia

<span style="color:blue">


<p> 2.1 Continuïtat tecnològica, d’acord a les necessitats i amb l’objectiu de millorar el manteniment i evolució de les aplicacions es promou:

<ul>
	<li> 2.1.1 La creació d’aplicacions orientades a serveis; els serveis (backend) exposaran el seu negoci mitjançant REST i en format JSON (REST permet ser consumit per qualsevol tecnologia que interpreti Http).</li>
	<li> 2.1.2 En el cas d’aplicacions web, la presentació estarà construïda amb tecnologies estàtiques  (html5/javascript/css) i consumirà els serveis que li proporcioni el backend. </li>
</ul>


<p>2.2 Estabilitat del sistema, les versions de les diferents peces (productes, llibreries...) que componen un sistema han de ser el més estable possible, d’aquí que es recomani l’ús de versions LTS (Long-Term Support) o, si de cas hi manca, GA (General Availability) o la nomenclatura que hagi donat el fabricant. Versions productives d’un sistema mai haurien d’incorporar versions no consolidades (snapshot, alpha, beta, release candidate, milestone...) dels components que en formen part.</p>

<p>2.3 Diversitat tècnica, davant solucions estàndards s’utilitzaran preferentment els components llestos per adoptar que es troben al <a href="https://qualitat.solucions.gencat.cat/estandards/estandard-full-ruta-programari.">Full de Ruta</a> com a suportats CTTI. Aquest fet no exclou que per a noves solucions es puguin proposar altres tecnologies, que eventualment passaran a formar-ne part. Cal utilitzar les tecnologies que millor encaixin al cas d’ús, ja que en un mateix sistema en poden conviure diverses per cobrir diferents necessitats.</p>

<p>2.4 Interoperabilitat, el programari i el maquinari han d’ajustar-se a estàndards definits que promouen la interoperabilitat de dades, aplicacions i tecnologia.</p>

<p>2.5 Portabilitat tecnològica, les aplicacions són independents de la tecnologia escollida i per això poden operar sobre una gran varietat de plataformes tecnològiques, s’ha d’assegurar que les aplicacions no són dependents d’un hardware o software específic. Un exemple podria ser la facilitat i transparència a l'hora de moure una aplicació d’un Cloud privat a un Cloud públic:</p>

<ul>
    <li>2.5.1 Desenvolupament multi plataforma, les aplicacions  s'han de construir tenint en compte que s'han de poder executar correctament en diferents plataformes de hardware i software</li>
    <li>2.5.2 Disseny responsive; les aplicacions s'han de poder veure correctament en pc's, portàtils, tabletes i smartphones.</li>
    <li>2.5.2 Manifest d'Heroku, el desenvolupament d'aplicacions web, o software-as-a-service ha d'estar alineat amb els 12 factors d'Heroku.</li>
</ul>

<p>2.6 Reutilització tecnològica, es prioritzarà la utilització de solucions transversals en comptes de fer-ne solucions a mida. La reutilització d'infraestructura ja existent no eximeix del requeriment d'actualitzar el programari en cas que aquest ja no estigui suportat pel fabricant.</p>
<p>2.7  Des de les unitats d'Arquitectura es fa un seguiment de noves tecnologies per a avaluar el seu encaix i tenir altres opcions davant noves necessitats dels projectes (per exemple, nous llenguatges i frameworks o provisió d'infraestructura). L'eina que s'utilitza per a aquesta tasca és el <a href="https://canigo.ctti.gencat.cat/drafts/radar/">Radar Tecnològic. </a>

<p> 2.8 Ubicació dels certificats, els certificats de les URLs tenen que ser pujats als Balancejadors de NUS.
<p> 2.9 Nomenclatura de dominis, es tenen que complir les nomenclatures de noms de dominis del document <a href="https://qualitat.solucions.gencat.cat/estandards/estandard-dominis-dns/">Estandards Dominis DNS.</a> Els Dominis "aplicacio".gencat.cat tenen que ser validats per la DGAC.
<p> 2.10 Nomenclatura de les infraestructures, es te que complir l'estàndard en quant al nom de les infraestructures detallat al document <a href="https://qualitat.solucions.gencat.cat/estandards/estandard-nomenclatura-infraestructures/">Estandard Nomenclatura Infraestructures</a>
<p> 2.11 Servidors SMTP Transversals, es te que fer us dels servidors Smtp transversals (IronPort) com servidor SMTP per enviar correus des de les aplicacions, es necessari donar d'alta els servidors d'aplicacions per pogué reenviar correus des de la plataforma.
			<a href="https://portic.ctti.gencat.cat/solucions/soltecnologiques/_layouts/15/WopiFrame.aspx?sourcedoc=%2Fsolucions%2Fsoltecnologiques%2FDocuments%2FLloc%20de%20Treball%2F10%2D02%2FCTTI%5F9%2E61%5FIntegraci%C3%B3%5FSMTP%5FIronPort%2Epdf&action=view">Manual per a la Integració SMTP</a>
<p> 2.12 Accés a internet des de xCAT, l'accés a recursos internet des de servidors ubicats a la xarxa XCAT, es necessari utilitzar el ProxyPass, mai accedir directament a Internet.
<p> 2.13 Us de Cloud Públic, valorar l'ús d'entorns Cloud públics, recomanat per aplicacions de les següents característiques:
<ul>
	<li> D'us des de Internet. </li>
	<li> Aplicació sense requeriments de seguretat alts. </li>
	<li> Sense integració amb altres serveis de Gencat. </li>
</ul>
<p> 2.14 Connectivitat de tercers, tenir en compte l'estàndard per la  <a href="https://qualitat.solucions.gencat.cat/estandards/estandard-connexio-equips-tercers/">connexió d'equips de tercers.</a>
<p> 2.15 Comunicacions per sFTP, no permeses les connexions no segures com el FTP.
<p> 2.16 Us d'Https, es necessari l’ús d'Https per les urls de les aplicacions.
<p> 2.17 Principis sobre la seguretat, es tenen que tenir en compte els principis de seguretat publicats per CESICAT, per mes informació visitar el <a href="https://portal.cesicat.cat/index.php">Portal de CESICAT. </a>

</span>	

# 3. Principis sobre el cost i manteniment de les solucions

<p>3.1 Prevalença dels principis, tots els principis definits en aquest document i els principis que estableix la Guia Web Gencat apliquen a tota l’organització, incloent els seus col·laboradors externs.

<p>3.2 Optimització de costos, pensar en els costos i en la seva optimització:</p>
<ul>
    <li>3.2.1 Monitoritzar els serveis per a identificar necessitats d’ampliació o reducció de recursos i poder ajustar els costos en conseqüència.</li>
    <li>3.3.2 Arquitectura/dissenyar les càrregues de treball amb els costos en ment.</li>
    <li>3.2.3 Arquitectura Mínima, tenir en compte l’escalabilitat i fer una previsió (mínim 1 any) amb l’objectiu d’aconseguir una arquitectura sostenible en el temps.</li>
</ul>

<p>3.3 Benefici màxim al menor cost i risc possible, cal tenir presents els costos d’infraestructura i el model de llicenciament requerits per a posar en marxa una solució ja que representen un cost recurrent. Alhora de concebre una solució s’ha d’identificar quin tipus de llicenciament serà el millor per la solució desitjada. Quan s’escull un producte (opensource o comercial) o es tria fer un desenvolupament a mida, cal fer una avaluació del cost vs. benefici de l’opció triada respecte a les altres: </p>
<ul>
    <li>3.3.1 Per a problemes comuns, utilitza “Opensource”. </li>
    <li>3.3.2 Per a problemes poc comuns, compra. </li>
    <li>3.3.3 Per a problemes únics, construeix.</li>
</ul>

<p>3.4 Impacte d’actualització, pensar en l’impacte d’actualització que pugui tenir un canvi de sistema operatiu, middleware o producte allà on corre l’aplicació: com menys acoblament amb el sistema de base i més utilització d’estàndards existeix, més senzilla serà l’actualització o l’ampliació de funcionalitats de l’aplicació.</p>

<p>3.5 Protecció de la propietat intel·lectual, La propietat intel·lectual de l'empresa (IP) ha d'estar protegida.</p>

<p>3.6 Compliment de la Llei, les aplicacions desenvolupades compleixen amb totes les lleis, polítiques i regulacions pertinents.</p>

# Principis de Dades

<p>Les dades són un actiu , les dades són un recurs corporatiu valuós i per tant s’han de protegir; tenen  un valor real i mesurable. </p>

<p>Les dades s’han de gestionar amb  cura, són el fonament de la nostra presa de decisions, per la qual cosa també les hem de gestionar acuradament per assegurar que sabem on estan i obtenir-les quan les necessitem.</p>

<p>1.1 Accés a les dades,  les dades són accessibles per permetre als usuaris realitzar les seves funcions.</p>

<p>1.2 Responsable de dades, cada element de dades té un reposable que s’encarrega d’assegurar la qualitat de les mateixes.</p>

<p>1.3 Seguretat de les dades, les dades estan protegides contra l'ús i divulgació no autoritzades.</p>
