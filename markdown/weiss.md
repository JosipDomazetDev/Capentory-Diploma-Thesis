Technische Umsetzung: Infrastruktur
=============================
Um allen Kunden einen problemlosen Produktivbetrieb zu gewährleisten muss ein physischer Server aufgesetzt werden. Auf diesem können dann alle Komponenten unseres Git-Repositories geklont und betriebsbereit installiert werden. Dafür gab es folgende Punkte zu erfüllen:

* das Organisieren eines Servers
* das Aufsetzen eines Betriebssystemes
* die Konfiguration der notwendigen Applikationen
* die Konfiguration der Netzwerkschnittstellen
* das Testen der Konnektivität im Netzwerk
* die Containervirtualisierung der einzelnen Komponenten (z.B. Datenbank, Webserver, ...)
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

Jedoch liegen alle Maschinen der Diplomarbeitsteams in einem eigens gebaut und gesicherten Virtual Private Networks (VPN), sodass nur mittels eines eingerichteten Tools auf den virtualisierten Server zugegriffen werden kann.

### FortiClient
FortiClient ermöglicht es, eines VPN-Konnektivität anhand von IPsec oder SSL zu erstellen. Die Datenübertragung wird verschlüsselt und damit der enstandene Datenstrom vollständig gesichert über einen sogenannten "Tunnel" übertragen.

Da die Diplomarbeit "Capentory" jedoch Erreichbarkeit im Schulnetz verlangt, muss die Maschine in einem Ausmaß abgesichert werden, damit sie ohne Bedenken in das Schulnetz gehängt werden kann. Dafür müssen folgende Punkte gewährleistet sein:
* Konfiguration beider Firewalls (siehe Punkt **Serversicherung**)
*  Wohlüberlegte Passwörter und Zugriffsrechte

## Das Aufsetzen eines Betriebssystemes

## Konfiguration der Netzwerkschnittstellen

## Konfiguration der notwendigen Applikationen

## Testen der Konnektivität im Netzwerk

## Containervirtualisierung der einzelnen Komponenten

## Verfassen einer Serverdokumentation

## Serversicherung

## Überwachung des Netzwerks 