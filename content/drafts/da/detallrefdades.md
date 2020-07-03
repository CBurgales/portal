+++
date        = "2020-04-01"
title       = "Detall Dades de Referència"
description = "Arquitectura de Dades de CTTI"
sections    = ["Data Architecture"]
categories  = ["Data Architecture"]
weight= 5
+++

<h2 id="entitatTitol"></h2>
<p id="myDefinition"></p>
<table id="myTable"></table>


<script type="text/javascript">

    var dades = JSON.parse(localStorage.getItem('data'));


    var body = document.getElementsByTagName("body")[0];
    
    var entitatTitol = document.getElementById("entitatTitol");
    entitatTitol.style.width = "70%";
    entitatTitol.style.marginRight = "15%";
    entitatTitol.style.marginLeft = "15%";
    entitatTitol.style.marginTop = "0.5em";
    entitatTitol.style.marginBottom = "0.5em";

    var textEntitatTitol = document.createTextNode(dades[2]);
    entitatTitol.appendChild(textEntitatTitol);

    body.appendChild(entitatTitol);

    var definicio = document.getElementById("myDefinition");
    definicio.style.width = "70%";
    definicio.style.marginRight = "15%";
    definicio.style.marginLeft = "15%";
    definicio.style.marginTop = "0.5em";
    definicio.style.marginBottom = "1.5em";

    var textDefinicio = document.createTextNode(dades[3]);
    definicio.appendChild(textDefinicio);

    body.appendChild(definicio);

    var metadatos = ["Identificador","Agrupació temàtica","Classificació funcional","Òrgan propietari","Període actualització","Tipus","Visibilitat","Data publicació","Data darrera actualització","Descàrrega"];


    var tabla   =  document.getElementById("myTable");
    tabla.style.width = "70%";
    tabla.style.marginRight = "15%";
    tabla.style.marginLeft = "15%";

    var tblBody = document.createElement("tbody");
    var tblThead = document.createElement("thead");


    var hilera = document.createElement("tr");
   
    var celda = document.createElement("th");
    var textoCelda =  document.createTextNode("Metadades");
    celda.appendChild(textoCelda);
    hilera.appendChild(celda);

    var celda2 = document.createElement("th");
    var textoCelda2 =  document.createTextNode("Valor");
    celda2.appendChild(textoCelda2);
    hilera.appendChild(celda2);

    tblThead.appendChild(hilera);


     // Crea las celdas
     for (var i = 0; i < 9; i++) {
      // Crea las hileras de la tabla
      hilera = document.createElement("tr");

      for (var j = 0; j < 2; j++) {
        // Crea un elemento <td> y un nodo de texto, haz que el nodo de
        // texto sea el contenido de <td>, ubica el elemento <td> al final
        // de la hilera de la tabla
        if(j==0){
          celda = document.createElement("td");
          textoCelda = document.createTextNode(metadatos[i]);
          celda.style.fontWeight = "bold";
          celda.appendChild(textoCelda);
          hilera.appendChild(celda);
        }
        else{
          celda = document.createElement("td");
          textoCelda = document.createTextNode(dades[i+4]);
          celda.appendChild(textoCelda);
          hilera.appendChild(celda);
        }
      }

      // agrega la hilera al final de la tabla (al final del elemento tblbody)
      tblBody.appendChild(hilera);
    }
	

	// -----------------------------------------------------
    //  v02.CTD-16/10/2019 (inici) Crear nova fila de dades per mostrar informació sobre els atributs

	hilera = document.createElement("tr");

    celda = document.createElement("td");
    celda.style.fontWeight = "bold";
    textoCelda =  document.createTextNode("Atributs");
    celda.appendChild(textoCelda);
    hilera.appendChild(celda);
	
    celda = document.createElement("td");
	
    var frame = document.createElement('iframe');
	
  	var nomFitAtr=dades[14];
	
    frame.setAttribute("src","https://view.officeapps.live.com/op/embed.aspx?src=https://canigo.ctti.gencat.cat/drafts/entitats/"+ nomFitAtr);
    frame.style.width= "100%"
    frame.style.height= "300px";
    celda.appendChild(frame);
    hilera.appendChild(celda);

    tblBody.appendChild(hilera);

    //  v02.CTD-16/10/2019 (fi)
	// -----------------------------------------------------	
	

    hilera = document.createElement("tr");
   
    celda = document.createElement("td");
    celda.style.fontWeight = "bold";
    textoCelda =  document.createTextNode("Descàrregues");
    celda.appendChild(textoCelda);
    hilera.appendChild(celda);

    celda = document.createElement("td");

	textoCelda =  document.createTextNode("           ");
    celda.appendChild(textoCelda);

    var link_valors = document.createElement('a');
    link_valors.setAttribute('href', '../../entitats/' + dades[14]);
    link_valors.innerHTML = "Descarregar definició atributs";
    celda.appendChild(link_valors);

	textoCelda =  document.createTextNode("               ");
    celda.appendChild(textoCelda);
	
    //++1 var link = document.createElement('a');
    //++1 link.setAttribute('href', '../../entitats/' + dades[13]);
    //++1 link.innerHTML = "Descarregar fitxer de dades";
    //++1 celda.appendChild(link);

    // var image = document.createElement('img');
    // image.setAttribute("src","https://img.icons8.com/color/50/000000/ms-excel.png");
    //link.innerHTML = dades[15];
    //link.appendChild(image);

    hilera.appendChild(celda);

    tblBody.appendChild(hilera);


    // posiciona el <tbody> debajo del elemento <table>

    //tabla.appendChild(tblThead);
    //tabla.appendChild(tblBody);
    // appends <table> into <body>
    //body.appendChild(tabla);

    
    hilera = document.createElement("tr");
   
    celda = document.createElement("td");
    celda.style.fontWeight = "bold";
    textoCelda =  document.createTextNode("Dades");
    celda.appendChild(textoCelda);
    hilera.appendChild(celda);

    celda = document.createElement("td");

	//++1 (02/07/2020) Es comenta la visualitzacio dels fitxers excel i a canvi es posa un text
	//++1 indicant que per consultar les dades s'ha de sol.licitar a la bústia de GTD

    //++1 var frame = document.createElement('iframe');
    //++1 frame.setAttribute("src","https://view.officeapps.live.com/op/embed.aspx?src=https://canigo.ctti.gencat.cat/drafts/entitats/"+ dades[13]);
    //++1 frame.style.width= "100%"
    //++1 frame.style.height= "400px";
    //++1 celda.appendChild(frame);
    
	//++1 inici noves instruccions
	celda.innerHTML = "Per consultar el conjunt de valors de l'entitat cal que envieu un correu a l'Oficina de Gestió Tècnica <br/> de Dades CTTI (<a href="mailto:gtd.ctti@gencat.cat">gtd.ctti@gencat.cat</a>) indicant quines són les entitats sobre les quals voleu consultar els valors.";
	//++1 fi noves instruccions

	hilera.appendChild(celda);


    tblBody.appendChild(hilera);


    tabla.appendChild(tblThead);
    tabla.appendChild(tblBody);

    body.appendChild(tabla);
    
    //function happycode(){
     //var dades = JSON.parse(localStorage.getItem('data'));
     //var url = "https://view.officeapps.live.com/op/embed.aspx?src=https://canigo.ctti.gencat.cat/drafts/entitats/"+ dades[13];
     //$('#myframe').attr("src", url);
    //}
	
	
	
	

</script>
