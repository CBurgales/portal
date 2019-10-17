+++
date        = "2017-09-23"
title       = "Detall Dades de Referència"
description = "Arquitectura de Dades de CTTI"
sections    = ["Data Architecture"]
categories  = ["Data Architecture"]
weight= 5
+++

  <div>
    <h2 id="entitatTitol"></h2>
    <p id="myDefinition"></p>
    <table id="myTable"></table>
  </div>
  


<script type="text/javascript">

    var dades = JSON.parse(localStorage.getItem('data'));


    var body = document.getElementsByTagName("body")[0];
    
    var entitatTitol = document.getElementById("entitatTitol");
    entitatTitol.style.width = "70%";
    entitatTitol.style.marginRight = "15%";
    entitatTitol.style.marginLeft = "15%";
    entitatTitol.style.marginTop = "0.5em";
    entitatTitol.style.marginBottom = "0.5em";

    var textEntitatTitol = document.createTextNode(dades[1]);
    entitatTitol.appendChild(textEntitatTitol);

    body.appendChild(entitatTitol);

    var definicio = document.getElementById("myDefinition");
    definicio.style.width = "70%";
    definicio.style.marginRight = "15%";
    definicio.style.marginLeft = "15%";
    definicio.style.marginTop = "0.5em";
    definicio.style.marginBottom = "1.5em";

    var textDefinicio = document.createTextNode(dades[2]);
    definicio.appendChild(textDefinicio);

    body.appendChild(definicio);

    var metadatos = ["Identificador","Origen","Classificació funcional","Agrupació temàtica","Òrgan responsable","Òrgan propietari","Període actualització","Tipus","Visibilitat","Descàrrega"];


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
    var textoCelda2 =  document.createTextNode("Definició");
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
	
//	var nomFitAtr="Atr_" + dades[13] + ".docx";
    -- Els atributs venen per un doc amb el mateix nom que l'excel canviant .xlsx per _ATR.doc--
	-- Pendent de confirmar el canvi i veure si es més factible passar el nom del doc a parametre
  	var nomFitAtr_aux=dades[13];
    var nomFitAtr=nomFitAtr_aux.replace(".xlsx","_ATR.docx";
	
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
    textoCelda =  document.createTextNode("Descàrrega");
    celda.appendChild(textoCelda);
    hilera.appendChild(celda);

    celda = document.createElement("td");
    var link = document.createElement('a');
    var image = document.createElement('img');

    link.setAttribute('href', '../../entitats/' + dades[13]);
    link.innerHTML = "Descarregar Excel";

    image.setAttribute("src","https://img.icons8.com/color/50/000000/ms-excel.png");

    //link.innerHTML = dades[15];
    //link.appendChild(image);
    celda.appendChild(link);
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
    textoCelda =  document.createTextNode("Preview");
    celda.appendChild(textoCelda);
    hilera.appendChild(celda);

    celda = document.createElement("td");
    var frame = document.createElement('iframe');
    frame.setAttribute("src","https://view.officeapps.live.com/op/embed.aspx?src=https://canigo.ctti.gencat.cat/drafts/entitats/"+ dades[13]);
    frame.style.width= "100%"
    frame.style.height= "400px";
    celda.appendChild(frame);
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
  
  
