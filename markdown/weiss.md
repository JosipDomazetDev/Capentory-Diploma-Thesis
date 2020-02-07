


Technische Umsetzung: Infrastruktur
=============================
Um allen Kunden einen problemlosen Produktivbetrieb zu gewährleisten muss ein physischer Server aufgesetzt werden. Auf diesem können dann alle Komponenten unseres Git-Repositories geklont und betriebsbereit installiert werden. Dafür gab es folgende Punkte zu erfüllen:

* das Organisieren eines Servers
* das Aufsetzen eines Betriebssystemes
* die Konfiguration der notwendigen Applikationen
* die Konfiguration der Netzwerkschnittstellen
* das Testen der Konnektivität im Netzwerk
* die Einrichtung des Produktivbetriebes der Applikation
* das Verfassen einer Serverdokumentation
* die Absicherung der Maschine
* die Überwachung des Netzwerks

In den folgenden Kapitel wird deutlich gemacht, wie die oben angeführten Punkte, im Rahmen der Diplomarbeit "Capentory" abgearbeitet wurden.

## Anschaffung des Servers
Den 5. Klassen wird, dank gesponserter Infrastruktur, im Rahmen ihrer Diplomarbeit ein Diplomarbeitscluster zur Verfügung gestellt. Damit können sich alle Diplomarbeitsteams problemlos Zugang zu ihrer eigenen virtualisierten Maschine verschaffen. 
Die Virtualisierung dieses großen Servercluster funktioniert mittels einer ProxMox-Umgebung.

### ProxMox

Proxmox Virtual Environment (kurz PVE) ist eine auf Debian und KVM basierende Virtualisierungs-Plattform zum Betrieb von Gast-Betriebssystemen.
Vorteile:

* läuft auf fast jeder x86-Hardware
* frei verfügbar
* ab 3 Servern Hochverfügbarkeit

Jedoch liegen alle Maschinen der Diplomarbeitsteams in einem eigens gebaut und gesicherten Virtual Private Network (VPN), sodass nur mittels eines eingerichteten Tools auf den virtualisierten Server zugegriffen werden kann.

### FortiClient

FortiClient ermöglicht es, eines VPN-Konnektivität anhand von IPsec oder SSL zu erstellen. Die Datenübertragung wird verschlüsselt und damit der enstandene Datenstrom vollständig gesichert über einen sogenannten "Tunnel" übertragen.

Da die Diplomarbeit "Capentory" jedoch Erreichbarkeit im Schulnetz verlangt, muss die Maschine in einem Ausmaß abgesichert werden, damit sie ohne Bedenken in das Schulnetz gehängt werden kann. Dafür müssen folgende Punkte gewährleistet sein:

* Konfiguration beider Firewalls (siehe Punkt **Absicherung der virtuellen Maschine**)
* Wohlüberlegte Passwörter und Zugriffsrechte

## Wahl des Betriebssystems

Neben den physischen Hardwarekomponenten wird für einen funktionierenden und leicht bedienbaren Server logischerweise auch ein Betriebsystem benötigt. Die erste Entscheidung, welche Art von Betriebssystem für die Diplomarbeit "Capentory" in Frage kam wurde rasch beantwortet: Linux. Weltweit basieren die meisten Server und andere Geräte auf Linux. Jedoch gibt es selbst innerhalb des OpenSource-Hersteller zwei gängige Distributionen, die das Diplomarbeitsteam während deren Schulzeit an der Htl Rennweg kennenlernen und Übungen darauf durchführen durfte:

 * Linux CentOS
 * Linux Ubuntu

### Linux CentOS

CentOS ist eine frei verfügbare Linux Distribution, die auf [Red Hat Enterprise Linux](https://de.wikipedia.org/wiki/Red_Hat_Enterprise_Linux) aufbaut. Hinter Ubuntu und Debian ist CentOS die am dritthäufigsten verwendete Software und wird von einer offenen Gruppe von freiwilligen Entwicklern betreut, gepflegt und weiterentwickelt.

### Linux Ubuntu

Ubuntu ist die am meist verwendete Linux-Betriebssystemsoftware für Webserver. Auf Debian basierend ist das Ziel der Entwickler, ein ein einfach zu installierendes und leicht zu bedienendes Betriebssystem mit aufeinander abgestimmter Software zu schaffen. Hauptsponsor des Ubuntu-Projektes ist der Software-Hersteller [Canonical](https://de.wikipedia.org/wiki/Canonical), der vom südafrikanischen Unternehmer [Mark Shuttlerworth](https://de.wikipedia.org/wiki/Mark_Shuttleworth) gegründet wurde.

### Vergleich und Wahl

**CentOS**

 * Kompliziertere Bedienung
 * Keine regelmäßigen Softwareupdates
 * Weniger Dokumentation vorhanden

**Ubuntu**

 - Leichte Bedienung
 - Wird ständig weiterentwickelt und aktualisiert
 - Zahlreich brauchbare Dokumentation im Internet vorhanden
 - Wird speziell von Ralph empfohlen

Aus den angeführten Punkten entschied sich "Capentory" klarerweise das Betriebsystem Ubuntu zu verwenden, vorallem auch weil der Hersteller deren Serversoftware die Verwendung von diesem Betriebsystem empfiehlt. Anschließend wird die Installation des Betriebsystemes genauer erläutert und erklärt.

## Installation des Betriebsystemes

Wie bereits unter Punkt """Anschaffung des Servers""" erwähnt, wird uns von der Schule ein eigener Servercluster mit virtuellen Maschinen zur Verfügung gestellt. Durch die ProxMox-Umgebung und diversen Tools, ging die Installation der Ubuntu-Distribution rasch von der Hand. In der Virtualisierungsumgebung der Schule musste nur ein vorhandenes Linux-Ubuntu 18.04 ISO-File gemountet und anschließend eine gewöhnliche Betriebsysteminstallation für Ubuntu durchgeführt werden. 
Jedoch kam es beim ersten Versuch zu Problemen mit der Konfiguration der Netzwerkschnittstellen, die im nächsten Punkt genauer erläutert werden.

## Konfiguration der Netzwerkschnittstellen

### Problematische Ereignise bei der Konfiguration der Schnittstellen

Um eine funktiorierende Internetverbindung zu erstellen, durfte die Konfiguration der Schnittstellen nicht erst "später durchgeführt" werden, da mit dem Aufschub der Schnittstellen-Konfiguration das Paket "NetworkManager" nicht installiert wurde. Der "NetworkManager" ist verantwortlich für den Zugang zum Internet und der Netzwerksteuerung auf dem Linuxsystem. Und da im Nachhinein dieses Paket nicht installiert war (und aufgrund fehlender Internetverbindung nicht installiert werden konnte), half auch die fehlerfreie Interface-Konfiguration nicht, um eine Konnektivität herzustellen. Dadurch musste ein zweiter Installationsdurchgang durchgeführt werden, worauf dann alles fehlerfrei und problemlos lief.

### Konfiguration
Im Rahmen des Laborunterrichts an der Htl Rennweg, bekamen die Schüler für diverse Unklarheiten ein sogenanntes [Cheat-Sheet](https://netzwerktechnik.htl.rennweg.at/~zai/Fachschule/NWT_Stamm_Foerderkurs/Survival_Guide_Linux_Network_Configuration.pdf) für Linux-Befehle zur Verfügung gestellt. In diesem Cheat-Sheet finden sich unteranderem Anleitungen für die Konfiguration einer Netzwerkschnittstelle auf einer CentOS/RedHat sowie Ubuntu/Debian-Distribution.
Den Schülern der fünften Netzwerktechnikklasse sollte diese Kurzkonfiguration jedoch schon leicht von der Hand gehen, da sie diese Woche für Woche benötigen.

Eine Netzwerkkonfiguration mit statischen IPv4-Adressen für eine Ubuntu-Distribution könnte wie folgt aussehen:

Eine Netzwerkkonfiguration mit Verwendung eines IPv4-DHCP-Servers für eine Ubuntu-Distribution könnte wie folgt aussehen:

### Topologie des Netzwerkes

Unter Abbildung 5.1 wird der Netzwerkplan veranschaulicht. Auf der linken Seite wird der Servercluster der Diplomarbeitsteams dargestellt, worauf die virtuelle Maschine von "Capentory" gehostet wird. In der Mitte ist die FortiGate-Firewall zu sehen, die nicht nur als äußerster Schutz vor Angriffen dient, sondern auch die konfigurierte VPN-Verbindung beinhaltet und nur berechtigten Teammitgliedern den Zugriff gewährleistet. Desweiteren ist die moderne Firewall auch für die Konnektivität der virtuellen Maschine im Schulnetz zuständig, aber dazu später (unter Punkt "Absicherung der virtuellen Maschine") mehr.
	![Netzwerkplan](topo1.png)
	
### Testen der Konnektivität im Netzwerk

 
## Installation der notwendigen Applikationen
Damit die Ubuntu-Maschine für den Produktivbetrieb startbereit ist, müssen im Vorhinein noch einige wichtige Konfigurationen durchgeführt werden. Die wichtigste (Netzwerkkonfiguration) wurde soeben ausführlich erläutert doch ohne der Installation von diversen Applikationen, wäre das System nicht brauchbar.

### Advanced Packaging Tool

Jeder Ubuntu Benutzer kennt es. Mit diesem Tool werden auf dem System die notwendigen Applikationen heruntergeladen, extrahiert und anschließend installiert. Insgesamt stehen einem 18 apt-get commands zur Verfügung. Genauere Erklärungen sowie die Syntax zu den wichtigsten commands folgen.

#### apt-get update

Update liest alle in `/etc/apt/sources.list` sowie in `/etc/apt/sources.list.d/` eingetragenen Paketquellen neu ein. Dieser Schritt wird vor allem vor einem upgrade-command oder nach dem Hinzufügen einer neuen Quelle empfohlen, um sich die neusten Informationen für Pakete ansehen zu können.

Syntax: `[sudo] apt-get [Option(en)] update`

#### apt-get upgrade

Upgrade bringt alle bereits installierten Pakete auf den neuesten Stand.

Syntax: `[sudo] apt-get [Option(en)] upgrade`

#### apt-get install

Install lädt das Paket bzw. die Pakete inklusive der noch nicht installierten Abhängigkeiten (und eventuell der vorgeschlagenen weiteren Pakete) herunter und installiert diese. Außerdem besteht die Möglichkeit, beliebig viele Pakete auf einmal anzugeben, indem sie mittels eines Leerzeichens getrennt werden.

Syntax: `[sudo] apt-get [Option(en)] install PAKET1 [PAKET2]`

Falls eine bestimmte Version installiert werden soll:

`[sudo] apt-get [Option(en)] install PAKET1=VERSION [PAKET2=VERSION]`

#### apt-get remove

Wie bereits erkannt, gibt es die Möglichkeit, sich ein beliebiges Paket zu installieren. Doch was soll geschehen falls dieses Paket nicht mehr benötigt wird? Daher gibt es prakitscherweise den remove-command, der, wie schon im Namen deutlich wird, ein oder mehrere Paket/e vollständig vom System entfernt.

Syntax: `sudo apt-get [Option(en)] remove PAKET1 [PAKET2]`

### Installierte Pakete mittels apt-get

#### NGINX

NGINX ist der am Häufigsten verwendete OpenSource-Webserver unter Linux für diverse Webanwendungen. Große Unternehmen wie Cisco, Microsoft, Facebook oder auch IBM schwören auf die Verwendung dieses genialen Paketes. Unteranderem wird NGINX auch als Reverse-Proxy, HTTP-Cache und Load-Balancer verwendet. Wie genau NGINX für den Produktivbetrieb funktioniert wird im Laufe des Punktes "Produktivbetrieb der Applikation" erläutert.

Installation: `[sudo] apt-get install nginx`

#### Docker

Die OpenSource-Software Docker ist eine Containervirtualisierungstechnologie, die die Erstellung und den Betrieb von Linux Containern ermöglicht. Wie genau dies funktioniert, wird später unter Punkt "Produktivbetrieb der Applikation" genauer beschrieben und erklärt.

Installation: `[sudo] apt-get install docker`

#### docker-compose

Jeder Linux-Benutzer hat mindestens einmal in seinem Leben etwas über das `docker-compose.yml` File gehört. Doch was ist docker-compose eigentlich? Nun, die Verwaltung und Verlinkung von mehreren Containern kann auf Dauer sehr nervenaufreibend sein. Die Lösung dieses Problems nennt sich docker-compose. Wie docker-compose jedoch genau funktioniert, wird ebenfalls wie das Grundkonzept von Docker unter Punkt "Produktivbetrieb der Applikation" präziser erläutert.

Installation: `[sudo] apt-get install docker-compose`

#### MySQL

MySQL ist ein OpenSource-Datenverwaltungssystem und die Grundlage für die meisten dynamischen Websiten. Darauf werden die Inventurdatensätze der Htl Rennweg gespeichert. Nähere Informationen finden sich Punkt "Produktivbetrieb der Applikation" wieder.

Installation: `[sudo] apt-get install mysql`

#### Redis

Redis ist eine In-Memory-Datenbank mit einer Schlüssel-Wert-Datenstruktur (Key Value Store). Wie auch MySQL handelt es sich um eine OpenSource-Datenbank. Warum Redis ebenfalls benötigt wurde, wird ebenfalls später im Punkt "Produktivbetrieb der Applikation" erklärt.

Installation: `[sudo] apt-get install redis`

#### virtualenv

Bei virtualenv handelt es sich um ein Tool, mit dem eine isolierte Python-Umgebung erstellt werden kann. Eine solch isolierte Umgebung besitzt eine eigene Installation von diversen Services und teilt ihre libraries nicht mit anderen virtuellen Umgebungen (im optionalen Fall greifen sie auch nicht auf die global installierten libraries zu). Dies bringt vor allem den großen Vorteil, dass im Testfall virtuelle Umgebungen aufgesetzt werden können, um nicht die globalen Konfigurationen zu gefährden.

Installation: `[sudo] apt-get install virtualenv`

#### Python
Python ist einer der Hauptbestandteile auf dem Serversystem der Diplomarbeit "Capentory". Das Backend (=Serveranwendung) basiert wie bereits erwähnt auf dem Python-Framework "Django". Um dieses Framework auf dem System installieren zu können wird jedoch noch ein weiteres "Packaging-Tool", speziell für Python-Module, benötigt.

Installation: `[sudo] apt-get install python3.x`

### pip

Und dieses Tool nennt sich Pip. Pip ist ein rekursives Akronym für **P**ip **I**nstalls **P**ython und ist, wie bereits erwähnt, das Standardverwaltungswerkzeug für Python-Module. Die Funktion sowie Syntax kann relativ gut mit der von apt verglichen werden.

Installation von pip3 (vorrausgesetzt python3.x ist auf dem System bereits installiert):

 `[sudo] apt-get install python3-pip`

### Installierte Pakete mittels pip3
 
#### uWSGI

Das eigentliche Paket, mit dem der Produktivbetrieb schlussendlich gewährleistet wurde, nennt sich uWSGI. Speziell wurde es für die Produktivbereitstellung von Serveranwendungen (wie eben der Django-Server von Team "Capentory") entwickelt und harmoniert eindrucksvoll mit der Webserver-Software NGINX. Die grundlegende Funktionsweise von uWSGI, sowie eine Erklärung, warum schlussendlich dieses Paket und nicht Docker verwendet wurde, wird unter Punkt "Produktivbetrieb der Applikation" veranschaulicht.

Installation: `[sudo] pip3 install uwsgi`

#### Django

Django ist ein in Python geschriebenes Webframework, auf dem unsere Serveranwendung basiert. Genauere Informationen wurden jedoch schon unter Punkt "Django und Ralph" übermittelt.

Installation: `[sudo] pip3 install Django`

## Produktivbetrieb der Applikation

## Absicherung der virtuellen Maschine

## Überwachung des Netzwerks 

## Verfassen einer Serverdokumentation