# TESTer

/*
Zuerst werden drei Variablen StaticServer, 
http und file angelegt und initilisiert
 */
var dynamicServer = require('node-hb'); //import des Moduls "node-hb"
var http = require('http'); //import des Moduls "http"
var file = new dynamicServer.Server('./html'); // Anlegen eines "Objektes" vom Typ dynamicServer bzw. node-hb
var url=require('url'); //import des Moduls "url"

'use strict';
/*
hilfreicher Code aus dem Tutorial
*/

var express = require('express'),
    exphbs  = require('../../'); // "express-handlebars"

var server = express();

server.engine('handlebars', exphbs({defaultLayout: 'main'}));
server.set('view engine', 'handlebars');

server.get('/', function (req, res) {
    res.render('home');
});

server.listen(3000, function () {
    console.log('express-handlebars example server listening on: 3000');
});
 

 
/*
Rest ist gleich geblieben
*/

/*
Als nächstes wird unsere Funtkion erstellt,
die die Ausgabe der gewünschten Informationen
in der Konsole vornimmt.
console.log(...) ist dafür die Methode der Konsolenausgabe
*/

function handleRequest (request, response) {
	
	var q = url.parse(request.url,true).query; //Zuweisung des geparsten QueryStrings an die Variable q
	console.log('Request received \n'); //Konsolenausgabe von dem String "Request received"
	if(q.user){ //Nur wenn der Key user vorhanden ist, führt er die folgenden Konsolenausgaben dort aus
	console.log('Anmeldung als:\n'); //Konsolenausgabe
	console.log(q.user+'\n'); //Konsolenausgabe vom Benutzernamen (den Wert des Keys)
	}
	console.log(request.method + '\n'); //Konsolenausgabe von der request Methode (z. B. GET)
	console.log(request.httpVersion + '\n'); //Konsolenausgabe von der http Version, die beim request verwendet wird (z. B. 1.1)
	console.log(request.url + '\n'); //Konsolenausgabe der Adresse, die aufgerufen wird (z. B. / für die Startseite bzw. index)
	console.log(request.headers); //Konsolenausgabe des Headers
	
	request.addListener('end', function () {// fügt ein "Listner" beim Event "end" hinzu -> callback; wird bei "end" getriggert/aufgerufen
				
		
		
		file.serve(request, response);//.serve leitet die angeforderte Seite weiter "response handler"
		console.log('\n');//Eine Leerzeile

		
		
	}).resume();//nötig damit es weiter geht nach dem "response" handler"
}

var server = http.createServer(handleRequest); //Hier wird der "http Server" erstellt und ihm wird die "handleRequest" Funktion übergeben
