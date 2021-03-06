---
layout: guide
title: GPC
creator: cryptncyrix
author: 
	- name: _cyrix_
	  profile: 7661

	- name: nikosch
	  profile: 2314

	- name: manko10
	  profile: 1139

inhalt:
	- name: "Superglobale Parametersumme"
	  anchor: superglobal
	  simple: "allgemeines Arrayrequest"

	- name: "Initialisierungsreihenfolge"
	  anchor: initialisierungsreihenfolge
	  simple: "korrekte Reihenfolge"
	  
---

Bei **GPC** handelt es sich um ein Kürzel, welches für **G**ET, **P**OST, **C**OOKIE steht. Gemeint sind Parameterwerte, die dem Script je nach Art des Aufrufs im [HTTP-Request](http://www.php.de/wiki-php/index.php/Request)  übergeben werden können.

Im Variablenraum von PHP manifestieren sich diese Parametermengen jeweils als gleichnamiges, superglobales Array: Anfallende Daten werden als $_GET, $_POST bzw. $_COOKIE global verfügbar. Da die Arrays getrennte Wertebereiche pro Parameterart benutzen, repräsentieren sie stets alle bei einem Scriptaufruf anfallenden Daten und unterliegen nicht der Konkurrenz gleichnamiger Schlüsselwerte. Ist die Herkunft der Angabe dagegen nicht relevant, kann alternativ eine andere Superglobale verwendet werden: $_REQUEST.

### 1. <a id="superglobal"></a>Superglobale Parametersumme
---
Das Array $_REQUEST vereint die Werte dieser drei Arrays in sich, wobei die Behandlung doppelter Werte abhängig von ini-Einstellungen variieren kann (siehe nachfolgend).

### 2. <a id="initialisierungsreihenfolge"></a>Initialisierungsreihenfolge
---
Sowohl für Arrays, die verschiedene Wertmengen gemeinsam verwalten, als auch das Verfahren [register_globals](http://www.php.de/wiki-php/index.php/Register_globals_(php.ini-Direktive\)), das Daten lokal unter ihrem Parameternamen als Variable verfügbar macht, ergibt sich die Problematik gleichnamiger Schlüssel: In einem Namensraum wie auch einer Array kann stets nur ein Wert einem Bezeichner zugeordnet sein. Wird aber bspw. in einem Aufruf gleichzeitig per POST und per COOKIE ein Parameter namens wert übergeben, treten die beiden Angaben in eine Konkurrenzsituation. 

Maßgeblich für die Initialisierung von Parameterdaten sind die [php.ini](http://www.php.de/wiki-php/index.php/Php.ini)-Direktiven gpc_order bzw. variables_order, die die Reihenfolge angeben, in denen gleichnamige Werte in $_REQUEST überschrieben werden. Die Standardreihenfolge ist gpc bzw. egpcs. Das heißt, GET wird von POST und POST von COOKIE überschrieben.
