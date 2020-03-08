


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

* Konfiguration beider Firewalls (siehe Punkt \ref{absicherung-der-virtuellen-maschine})
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

Wie bereits unter Punkt Anschaffung des Servers (siehe \ref{anschaffung-des-servers}) erwähnt, wird uns von der Schule ein eigener Servercluster mit virtuellen Maschinen zur Verfügung gestellt. Durch die ProxMox-Umgebung und diversen Tools, ging die Installation der Ubuntu-Distribution rasch von der Hand. In der Virtualisierungsumgebung der Schule musste nur ein vorhandenes Linux-Ubuntu 18.04 ISO-File gemountet und anschließend eine gewöhnliche Betriebsysteminstallation für Ubuntu durchgeführt werden. 
Jedoch kam es beim ersten Versuch zu Problemen mit der Konfiguration der Netzwerkschnittstellen, die im nächsten Punkt genauer erläutert werden.

## Konfiguration der Netzwerkschnittstellen

### Problematische Ereignise bei der Konfiguration der Schnittstellen

Um eine funktiorierende Internetverbindung zu erstellen, durfte die Konfiguration der Schnittstellen nicht erst "später durchgeführt" werden, da mit dem Aufschub der Schnittstellen-Konfiguration das Paket "NetworkManager" nicht installiert wurde. Der "NetworkManager" ist verantwortlich für den Zugang zum Internet und der Netzwerksteuerung auf dem Linuxsystem. Und da im Nachhinein dieses Paket nicht installiert war (und aufgrund fehlender Internetverbindung nicht installiert werden konnte), half auch die fehlerfreie Interface-Konfiguration nicht, um eine Konnektivität herzustellen. Dadurch musste ein zweiter Installationsdurchgang durchgeführt werden, worauf dann alles fehlerfrei und problemlos lief.

### Konfiguration
Im Rahmen des Laborunterrichts an der Htl Rennweg, bekamen die Schüler für diverse Unklarheiten ein sogenanntes [Cheat-Sheet](https://netzwerktechnik.htl.rennweg.at/~zai/Fachschule/NWT_Stamm_Foerderkurs/Survival_Guide_Linux_Network_Configuration.pdf) für Linux-Befehle zur Verfügung gestellt. In diesem Cheat-Sheet finden sich unteranderem Anleitungen für die Konfiguration einer Netzwerkschnittstelle auf einer CentOS/RedHat sowie Ubuntu/Debian-Distribution.
Den Schülern der fünften Netzwerktechnikklasse sollte diese Kurzkonfiguration jedoch schon leicht von der Hand gehen, da sie diese Woche für Woche benötigen.

Eine Netzwerkkonfiguration mit statischen IPv4-Adressen für eine Ubuntu-Distribution könnte wie folgt aussehen:

In `/etc/network/interfaces`:

    auto ens32
    iface ens32 inet static
    address 192.168.0.1
    netmask 255.255.255.0
    

Eine Netzwerkkonfiguration mit Verwendung eines IPv4-DHCP-Servers für eine Ubuntu-Distribution könnte wie folgt aussehen:

In `/etc/network/interfaces`:

    auto ens32
    iface ens32 inet dhcp

### Topologie des Netzwerkes

Unter Abbildung 5.1 wird der Netzwerkplan veranschaulicht. Auf der linken Seite wird der Servercluster der Diplomarbeitsteams dargestellt, worauf die virtuelle Maschine von "Capentory" gehostet wird. In der Mitte ist die FortiGate-Firewall zu sehen, die nicht nur als äußerster Schutz vor Angriffen dient, sondern auch die konfigurierte VPN-Verbindung beinhaltet und nur berechtigten Teammitgliedern den Zugriff gewährleistet. Desweiteren ist die moderne Firewall auch für die Konnektivität der virtuellen Maschine im Schulnetz zuständig, aber dazu später (unter Punkt "Absicherung der virtuellen Maschine") mehr.
\begin{figure}[ht]
\centering
\includegraphics{topo1.png}
\caption{Netzwerkplan}
\end{figure}
	
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

NGINX ist der am Häufigsten verwendete OpenSource-Webserver unter Linux für diverse Webanwendungen. Große Unternehmen wie Cisco, Microsoft, Facebook oder auch IBM schwören auf die Verwendung dieses genialen Paketes. Unteranderem wird NGINX auch als Reverse-Proxy, HTTP-Cache und Load-Balancer verwendet. Wie genau NGINX für den Produktivbetrieb funktioniert wird im Laufe des Punktes \ref{funktionsweise-der-produktivumgebung} erläutert.

Installation: `[sudo] apt-get install nginx`

#### Docker

Die OpenSource-Software Docker ist eine Containervirtualisierungstechnologie, die die Erstellung und den Betrieb von Linux Containern ermöglicht. Wie genau dies funktioniert, wird später unter Punkt \ref{funktionsweise-der-produktivumgebung} genauer beschrieben und erklärt.

Installation: `[sudo] apt-get install docker`

#### docker-compose

Jeder Linux-Benutzer hat mindestens einmal in seinem Leben etwas über das `docker-compose.yml` File gehört. Doch was ist docker-compose eigentlich? Nun, die Verwaltung und Verlinkung von mehreren Containern kann auf Dauer sehr nervenaufreibend sein. Die Lösung dieses Problems nennt sich docker-compose. Wie docker-compose jedoch genau funktioniert, wird ebenfalls wie das Grundkonzept von Docker unter Punkt \ref{funktionsweise-der-produktivumgebung} präziser erläutert.

Installation: `[sudo] apt-get install docker-compose`

#### MySQL

MySQL ist ein OpenSource-Datenverwaltungssystem und die Grundlage für die meisten dynamischen Websiten. Darauf werden die Inventurdatensätze der Htl Rennweg gespeichert. Nähere Informationen finden sich Punkt \ref{funktionsweise-der-produktivumgebung} wieder.

Installation: `[sudo] apt-get install mysql`

#### Redis

Redis ist eine In-Memory-Datenbank mit einer Schlüssel-Wert-Datenstruktur (Key Value Store). Wie auch MySQL handelt es sich um eine OpenSource-Datenbank.

Installation: `[sudo] apt-get install redis`

#### Nagios

Nagios ist ein Monitoring-System, mit dem sich verschiedene Geräte und auf solche laufende Dienste (oder auch Eigenschaften) überwachen lassen. Ziel ist es dabei schnell Ausfälle festzustellen und diese dem zuständigen Administrator mit zu teilen, so dass dieser dann schnell darauf reagieren kann.

Installation: `[sudo] apt-get install nagios`

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

Das eigentliche Paket, mit dem der Produktivbetrieb schlussendlich gewährleistet wurde, nennt sich uWSGI. Speziell wurde es für die Produktivbereitstellung von Serveranwendungen (wie eben der Django-Server von Team "Capentory") entwickelt und harmoniert eindrucksvoll mit der Webserver-Software NGINX. Die grundlegende Funktionsweise von uWSGI, sowie eine Erklärung, warum schlussendlich dieses Paket und nicht Docker verwendet wurde, wird unter Punkt \ref{produktivbetrieb-der-applikation} veranschaulicht.

Installation: `[sudo] pip3 install uwsgi`

#### Django

Django ist ein in Python geschriebenes Webframework, auf dem unsere Serveranwendung basiert. Genauere Informationen wurden jedoch schon unter Punkt \ref{django-und-ralph} übermittelt.

Installation: `[sudo] pip3 install Django`

#### runsslserver

Runsslserver ist ein Python-Paket mit dem eine Entwicklungsumgebung über https erreichbar gemacht werden kann.

Installation: `[sudo] pip3 install runsslserver`

## Produktivbetrieb der Applikation

Das Aufsetzen beziehungsweise die Installation der Produktivumgebung ist der wichtigste, aber auch aufwendigste, Bestandteil jeder Serverinfrastruktur. Eine Produktivumgebung soll von einer Testumgebung möglichst weit getrennt sein, damit die zu testende Software keinen Schaden für den produktiven Betrieb anrichten kann. Weiters soll durch den Produktivbetrieb der Server um einiges perfomanter sein, da dieser, im Falle der Diplomarbeit "Capentory", einen optimierten Webserver (NGINX) verwendet. 

### Entwicklungsumgebung

Die Entwickler des Grundservers "Ralph" haben eine eigene Entwicklungsumgebung für den Django-Server erstellt. Normalerweise findet sich in einem klassischen Python-Projekt oder einem Projekt, dass auf einem Python-Framework (wie zum Beispiel Django) basiert, ein sogenanntes "`manage.py`"-File. Hierbei handelt es sich um ein Script, dass das Management eines beliebigen Python-Projektes unterstützt. Mit dem Script ist man unter anderem in der Lage, den Webserver auf einem unspezifischen Rechner zu starten, ohne etwas Weiteres installieren zu müssen.

Jedoch wurde diese Datei im Projekt von "Ralph" und daher auch von "Capentory" in eine eigene spezifische Entwicklungsumgebung umimplementiert. Der Server startet sich nach der eigenen Installation (welche ab Seite 4 im Dokument "Serverdokumentation Schritt für Schritt erklärt wird) nicht mehr mittels:

    python manage.py runserver
 
sondern mit:

    dev_ralph runserver 0.0.0.0:8000

Obwohl die Befehle dieses Serverstarts nicht wirklich ident aussehen, führen sie im Hintergrund aber die gleichen Unterbefehle aus, um eine sichere Verwendung der Entwicklungsumgebung zu gewährleisten.

Da das Diplomarbeitsteam "Capentory" diesen Entwicklungsumgebung vorerst auf dem Produktivserver für Testzwecke verwendete, wurde ein eher unbekanntes Paket namens "runsslserver" für die Entwicklungsumgebung installiert und eingebaut. Zuerst musste, üblich um eine https-Verbindung einzurichten, beispielsweise mit dem Befehl

    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 
    -keyout /ssl/nginx.key -out /ssl/cert.crt

ein Zertifikat mit dem zugehörigen Schlüssel generiert werden.

Kurzerklärung der Befehlsoptionen

 - `-x509` Gibt an, statt eines CSR gleich ein selbstsigniertes Zertifikat auszustellen.
 - `-nodes` Zertifikat wird nicht über ein Kennwort geschützt. Damit kann der Server ohne weitere Aktion (Eingabe des Kennworts) gestartet werden.
 - `-days` Beschreibt die Gültigkeit des Zertifikates für 365 Tage.
 - `-newkey rsa:2048` Generiert das Zertifikat und einen 2048-bit langen RSA Schlüssel.
 - `-keyout` Gibt die Ausgabepfad und -datei für den Schlüssel an.
 - `-out` Gibt die Ausgabepfad und -datei des Zertifikates an.

Der Befehl "runsslserver" muss jetzt nur noch in den "Installed-Apps" des Projektes (im Falle von "Ralph" und "Capentory" befinden sich diese in der Datei "`base.py`") hinzugefügt werden.

Dann kann der Server problemlos mit 

    dev_ralph runsslserver 0.0.0.0:443

gestartet werden.

### Probleme der Produktivumgebung

Der Webserver selbst kann für unsere Zwecke nicht mit Docker für eine Produktivumgebung bereitgestellt werden, da die bereits vorhandenen Dockerfiles von Ralph nur für deren Lösung entwickelt wurden (d.h. Ralph installiert deren Webserver sehr komplex wie in etwa mit einem „apt-get install ralph“ Befehl in diversen Skripten). Jedoch gibt es eine eigene Docker-Variante für die Entwicklungsumgebung die rasch mit uWSGI in eine Produktivumgebung umgewandelt werden kann.

### Funktionsweise der Produktivumgebung

####  Funktion von uWSGI

Es wurde bereits desöfteren erklärt warum die vorhandene "Ralph-Dockerlösung" für die Zwecke von "Capentory" nicht brauchbar sind (siehe \ref{probleme-der-produktivumgebung}). Jedenfalls wurde nun die Einrichtung der Produktivumgebung mittels uWSGI erfolgreich durchgeführt. In der folgenden Grafik wird die Funktion von uWSGI genauer dargestellt.

\begin{figure}[ht]
\centering
\includegraphics{uwsgi.png}
\caption{Funktionsweise von uWSGI}
\end{figure}

Da die meisten Serverbenutzer alleine mit der Grafik nicht wirklich viel anfangen können, folgt nun eine Erklärung dieser:

 1. Ein beliebiger Benutzer eines Webbrowsers (zum Beispiel Google Chrome oder Mozilla Firefox) sendet einen sogenannten "Webrequest" auf den https-Port "443" (Abschnitt \ref{probleme-der-produktivumgebung}).
 2. Ein beliebig gewählter, optimierter Webserver (im Fall von "Capentory" NGINX) stellt Dateien wie Javascript, CSS oder auch Bilder bereit und macht diese für den Benutzer abrufbar.
 3. Hier wird die Kommunikation zwischen dem hochperfomanten Webserver NGINX und dem Webinterface uWSGI mit Verwendung eines klassischen Websockets veranschaulicht. (Anm.: Bei einem Websocket handelt es sich um ein auf TCP basierendes Protokoll, dass eine bidirektionale Verbindung zwischen einer Webanwendung und einem Webserver herstellt).
 4. Das eigentliche Interface von uWSGi ist hier zu sehen. Diese Schnittstelle sorgt für die Kommunikation des oben genannten Websockets mit dem verwendeten Python-Framework.
 5. Django reagiert nun auf die Anfrage des Benutzers und lässt diesem (falls dessen Zugriffsrechte darauf es erlauben) die gewünschten Daten.
 6. Die vom Benutzer gewünschten Daten werden (bei "Capentory") in einer MySQL-Datenbank gespeichert. 
 
 Grundsätzlich kann die Grafik auch mittels

    the web client <-> the web server <-> the socket <-> uwsgi <-> Django
 
 als normale ASCII-Zeichenkette dargestellt werden.

#### Funktion von Docker

Docker wird im Projekt "Capentory" ausschließlich für die Bereitstellung der Datenbank verwendet. Der Befehl 

`docker-compose -f docker/docker-compose-dev.yml up -d`

 im Projektstammverzeichnis erstellt und startet die in der folgenenden Grafik zu sehenden Container, die das Datenbanksystem darstellen:

\begin{figure}[ht]
\centering
\includegraphics{docker.png}
\caption{Datenbanksystem mit Docker}
\end{figure}

### Erreichbarkeit mittels HTTPS

Wie es sich für eine Netzwerktechnikklasse gehört, wurde auch an die Erreichbarkeit über HTTPS gedacht. NGINX wurde mit einem sogenannten Self Signed Certificate ausgestattet, um eine sichere Verbindung des Benutzers mit dem Webserver zu gewährleisten. Die Konfiguration von NGINX für den Gebrauch von uWSGI mit HTTPS ist auf Seite 9 der Serverdokumentation zu finden. Im Webbrowser "Microsoft Edge" würde der Aufruf des Servers nun wie folgt aussehen:

\begin{figure}[ht]
\centering
\includegraphics{https.png}
\caption{Aufruf des Servers über HTTPS}
\end{figure}

Der Zertifikatsfehler, der am Bild deutlich zu sehen ist, bedeudet jedoch nichts anderes als dass das Zertifakt des Produktivservers von "Capentory" nicht von einer offiziellen Zertifizierungsstelle signiert wurde. Da der Server aber sowieso nur im Schulnetz der HTL 3 Rennweg beziehungsweise über einen VPN-Tunnel erreichbar ist, war es nicht nötig sich an solch eine Zertifizierungsstelle zu wenden.

### Verwendung einer .ini-Datei

Bei einem File mit einer .ini-Endung handelt es sich um eine Initialisierungsdatei. Diese beinhaltet beispielsweise Konfigurationsmöglichen die zum Start eines bestimmten Dienstes benötigt werden. Glücklicherweise funktioniert so ein File auch wunderbar mit uWSGI. Durch die Implementierung einer ralph.ini-Datei wurden die Befehlsoptionen des `uwsgi`-Befehls ausgelagert und dieser somit für den Serverstart verkürzt.

Ein kleiner Vergleich:

    uwsgi --socket mysite.sock --module mysite.wsgi --chmod-socket=664 --proceses 10
    
    uwsgi --ini ralph.ini

Beide Befehle liefern schlussendlich das gleiche Ergebnis, jedoch ist der unter Befehl (Verwendung einer .ini-Datei) um einiges kürzer und erspart somit nervernaufreibende Schreibarbeit.

### Neustartverhalten mittels Service

Für den Gebrauch von uWSGi gibt es einige Lösungen, um den automatischen Neustart, beispielsweise bei Eintritt eines Stromausfalls, zu gewährleisten. "Capentory" hat sich, mit der Implementierung eines eigenen "Ralph-Service", für den klassischen Linuxweg entschieden.

Nach dem korrekten Erstellen der `ralph.service`-Datei kann dieser als ganz normaler Linux-Service behandelt werden.
Mit 

    systemctl enable ralph
    systemctl start ralph
 
 wird der frisch angelegte Service nun immer wenn die virtuelle Maschine gestartet wurde gestartet.

## Absicherung der virtuellen Maschine

Ein weiterer wichtiger und sensibler Teil der Serverinfrastruktur ist die Absicherung der virtuellen Maschine gegen Angriffe. Da es sich im Rahmen der Diplomarbeit "Capentory" um geheime Daten der Schulinventur handelt, war es ein Anliegen der Verantwortlichen, dass mit diesen Daten verantwortungsvoll und vorsichtig umgegangen wird.

### Firewall

Unter Punkt \ref{topologie-des-netzwerkes} wird der Plan des Netzwerkes veranschaulicht. Die in der Mitte liegende FortiGate-Firewall stellt, wie bereits in oben genannten Punkt erwähnt, die Verfügbarkeit des Servers im Schulnetz bereit. Dadurch dass der Server nur über eine VPN-Verbindung konfiguriert werden kann, denkt man sich bestimmt, dass die virtuelle Maschine schon genug gesichert sei. Jedoch ist es sinnvoll den Server doppelt abzusichern und somit wurden auf der Linux-Maschine ebenfalls noch Firewallregeln konfiguriert.

#### Erlauben einer SSH-Verbindung

Die virtuelle Maschine wurde aufgrund der nicht-vorteilshaften Konsole von ProxMox immer über SSH mit einer beliebigen externen Konsole (beispielsweise Putty) konfiguriert. Dies ist wegen der FortiGate-Konfiguration nur mit VPN-Verbindung möglich. Direkt auf dem Server wurde daher eine Firewallregel für die Erlaubnis von SSH implementiert.

Regel: `sudo ufw allow ssh`

oder:  `sudo ufw allow 22`

#### Erlauben des Datenaustausches mittels HTTP

Obwohl der Webserver den Datenaustausch mit HTTPS bevorzugt, wurde auf der zweiten Ebene auch eine Regel für die HTTP-Verbindung konfiguriert.

Regel: `sudo ufw allow http`

oder:  `sudo ufw allow 80`

#### Erlauben des Datenaustausches mittels HTTPS

Für den Datenaustausch über HTTPS musste ebenso eine Regel aktiviert werden.

Regel: `sudo ufw allow https`

oder:  `sudo ufw allow 443`

#### Verbieten aller restlichen Verbindungen

Zuguterletzt müssen alle restlichen (die nicht von Administratoren gebrauchten) Verbindungen deaktiviert werden um Angriffslücken zu schließen.

Regel: `ufw default deny`

### Mögliche Angriffsszenarien

Da der Server im Schulnetz erreichbar ist, gelten unteranderem auch die Schüler der HTL 3 Rennweg als potenzielle Angreifer. 

#### Malware

Bei Malware handelt es sich um Schadsoftware zudem unteranderem **Viren**, **Würmer** und **Trojaner** zählen.

#### Angriffe auf Passwörter

Neben dem Raten und Ausspionieren von Passwörtern ist die Brute Force Attacke weit verbreitet. Bei dieser Attacke versuchen Hacker mithilfe einer Software, die in einer schnellen Abfolge verschiedene Zeichenkombinationen ausprobiert, das Passwort zu knacken. Je einfacher das Passwort gewählt ist, umso schneller kann dieses geknackt werden.

Vorbeugung von Team "Capentory": Die festgelegten Passwörter sind komplex aufgebaut und sicher in den Köpfen der Mitarbeiter gespeichert.

#### Man-in-the-middle Attacken

Bei der „Man in the Middle“-Attacke nistet sich ein Angreifer  zwischen den miteinander kommunizierenden Rechnern. Diese Position ermöglicht ihm, den ausgetauschten Datenverkehr zu  kontrollieren und zu manipulieren. Er kann z.B. die ausgetauschten Informationen abfangen, lesen, die Weiterleitung kappen usw. Von all dem erfährt der Empfänger aber nichts.

Vorbeugung von Team "Capentory": Der Datenaustausch verläuft über HTTPS und somit verschlüsselt.

#### Sniffing

Unter Sniffing (Schnüffeln) wird das unberechtigte Abhören des Datenverkehrs verstanden. Dabei werden oft Passwörter, die nicht oder nur sehr schwach verschlüsselt sind, abgefangen. Andere Angreife bedienen sich dieser Methode um rausfinden zu können, welche Teilnehmer über welche Protokolle miteinander kommunizieren. Mit den so erlangten Informationen können die Angreifer dann den eigentlichen Angriff starten.

Vorbeugung von Team "Capentory": Der Datenaustausch verläuft über HTTPS und somit verschlüsselt.

## Überwachung des Netzwerks

Sollte der Fall eintreten, dass der Server nicht mehr erreichbar ist, wurde ein eigener Monitoring-Server im Netzwerk eingehängt und installiert. Dadurch wird der Produktivserver rund um die Uhr überwacht. Weiters wird das Diplomarbeitsteam "Capentory" bei etwaigen Komplikationen per E-Mail benachrigtigt, damit das aufgetretene Problem beziehungsweise die aufgetretenen Probleme möglichst schnell behoben werden können.

### Topologieänderung

Im Netzwerk wurde ein zweiter Server installiert, der mit dem OpenSource-Programm "Nagios" als Monitoring-Maschine für den Produktivserver dient.

\begin{figure}[ht]
\centering
\includegraphics{neutopo.png}
\caption{Ergänzter Netzwerkplan}
\end{figure}

### Nagios

Worum es sich bei diesem tollen Tool handelt wurde bereits unter Punkt \ref{nagios} erklärt. Nun folgt ein vertiefender Einblick in diese Monitoring-Software.

#### Hosts

Hosts sind bei Nagios definierte virtuelle Maschinen, die überwacht werden sollen. "Capentory" überwacht nicht nur den Produktivserver, sondern auch den aufgesetzten Nagios-Server selbst. Falls dieser Probleme aufweist, wird das Team ebenfalls per E-Mail benachrichtigt aber dazu unter Punkt \ref{notifications} mehr.

Im Webbrowser sehen die zu überwachenden Hosts wie folgt aus:

\begin{figure}[ht]
\centering
\includegraphics{hosts.png}
\caption{Die definierten Hosts der Diplomarbeit}
\end{figure}

**Capentory** ist der Name des Hosts des Produktivservers.

**Localhost** heißt der Host des Nagios-Servers.

Momentan scheint alles ohne Probleme zu funktionieren. Jedoch kann sich soetwas schlagartig ändern:

\begin{figure}[ht]
\centering
\includegraphics{dedprod.png}
\caption{Der heruntergefahrene Produktivserver}
\end{figure}

Hier wird deutlich gemacht, dass der Produktivserver abgestürzt ist.

#### Services

Nagios-Services sind, wie der Name schon verrät, die einzelnen zu überwachenden Services eines Hosts. Zurzeit wird am Nagios-Server aus Testzwecken eine Menge an Services überwacht. Auf dem Produktivserver hingegen werden nur Probleme, die mit der Verbindung über Port 22 (SSH) oder Port 443 (HTTPS) zu tun haben, erfasst.

So sehen die überwachten Services am Admin-Dashboard des Nagios-Servers aus:

\begin{figure}[ht]
\centering
\includegraphics{services.png}
\caption{Die überwachten Services}
\end{figure}

Im Moment weist kein Service der beiden Hosts ein kritisches Problem auf. Auf dem Nagios-Server ist der HTTP-Service zwar gelb markiert, hierbei handelt es sich jedoch nur um eine harmlose Warnung.

Wenn auf dem Produktivserver hingegen der Webserver NGINX ausfällt sieht das Dashboard jedoch wiederum anders aus:

\begin{figure}[ht]
\centering
\includegraphics{dednginx.png}
\caption{Die überwachten Services}
\end{figure}

#### Notifications

Zuguterletzt gibt es noch das Feature der Notifications. Falls ein Host oder ein Service der Hosts ausfällt, benachrichtigt der Nagios-Server das Diplomarbeitsteam per E-Mail über die aufgetretenen Probleme.

## Verfassen einer Serverdokumentation

Im Laufe dieses Kapitels wurde der Inhalt von den für die Produktivumgebung zu erstellenden Dateien aufgelistet und Zeile für Zeile erklärt. Da nur mit diesen Dateien alleine jedoch kein Server funktionieren kann war ein weiteres großes Ziel die Erstellung einer Guideline für interessierte Benutzer, damit diese ebenfalls in der Lage sind, den Server der Diplomarbeit "Capentory" für eine Entwicklungsumgebung sowie eine Produktumgebung aufzusetzen. 