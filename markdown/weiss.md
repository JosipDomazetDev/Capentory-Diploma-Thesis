

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
Jedoch kam es beim ersten Versuch zu Problemen mit der """Konfiguration der Netzwerkschnittstellen""", die im nächsten Punkt genauer erläutert werden.

## Konfiguration der Netzwerkschnittstellen

Um eine funktiorierende Internetverbindung zu erstellen, durfte die Konfiguration der Schnittstellen nicht erst "später durchgeführt" werden, da mit dem Aufschub der Schnittstellen-Konfiguration das Paket "NetworkManager" nicht installiert wurde. Der "NetworkManager" ist verantwortlich für den Zugang zum Internet und der Netzwerksteuerung auf dem Linuxsystem. Und da im Nachhinein dieses Paket nicht installiert war (und aufgrund fehlender Internetverbindung nicht installiert werden konnte), half auch die fehlerfreie Interface-Konfiguration nicht, um eine Konnektivität herzustellen. Dadurch musste ein zweiter Installationsdurchgang durchgeführt werden, worauf dann alles fehlerfrei und problemlos lief.

### Topologie des Netzwerkes

Unter Abbildung 5.1 wird der Netzwerkplan veranschaulicht.
	![Netzwerkplan](topo1.png)

## Konfiguration der notwendigen Applikationen

## Testen der Konnektivität im Netzwerk

## Produktivbetrieb der Applikation

## Verfassen einer Serverdokumentation

## Absicherung der virtuellen Maschine

## Überwachung des Netzwerks 