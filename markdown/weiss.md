\renewcommand{\kapitelautor}{Autor: Mathias Möller}




Technische Umsetzung: Infrastruktur
=============================
Um allen Kunden einen problemlosen Produktivbetrieb zu gewährleisten, muss ein physischer Server aufgesetzt werden. Auf diesem können dann alle Komponenten unseres Git-Repositories geklont und betriebsbereit installiert werden. Dafür gab es folgende Punkte zu erfüllen:

* das Organisieren eines Servers
* das Aufsetzen eines Betriebssystemes
* die Konfiguration der notwendigen Applikationen
* die Konfiguration der Netzwerkschnittstellen
* das Testen der Konnektivität im Netzwerk
* die Einrichtung des Produktivbetriebes der Applikation
* das Verfassen einer Serverdokumentation
* die Absicherung der Maschine
* die Überwachung des Netzwerk

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

* Konfiguration beider Firewalls (\siehe{absicherung-der-virtuellen-maschine})
* Wohlüberlegte Passwörter und Zugriffsrechte

## Wahl des Betriebssystems

Neben den physischen Hardwarekomponenten wird für einen funktionierenden und leicht bedienbaren Server logischerweise auch ein Betriebsystem benötigt. Die erste Entscheidung, welche Art von Betriebssystem für die Diplomarbeit "Capentory" in Frage kam wurde rasch beantwortet: Linux. Weltweit basieren die meisten Server und andere Geräte auf Linux. Jedoch gibt es selbst innerhalb des OpenSource-Hersteller zwei gängige Distributionen, die das Diplomarbeitsteam während deren Schulzeit an der HTL Rennweg kennenlernen und Übungen darauf durchführen durfte:

 * Linux CentOS
 * Linux Ubuntu

### Linux CentOS

CentOS ist eine frei verfügbare Linux Distribution, die auf Red-Hat \cite{redhat} aufbaut. Hinter Ubuntu und Debian ist CentOS die am dritthäufigsten verwendete Software und wird von einer offenen Gruppe von freiwilligen Entwicklern betreut, gepflegt und weiterentwickelt.

### Linux Ubuntu

Ubuntu ist die am meist verwendete Linux-Betriebssystemsoftware für Webserver. Auf Debian basierend ist das Ziel der Entwickler, ein einfach zu installierendes und leicht zu bedienendes Betriebssystem mit aufeinander abgestimmter Software zu schaffen. Hauptsponsor des Ubuntu-Projektes ist der Software-Hersteller Canonical \cite{canonical}, der vom südafrikanischen Unternehmer Mark Shuttleworth \cite{mark} gegründet wurde.

### Vergleich und Wahl \cite{wahl}

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

Wie bereits unter Punkt Anschaffung des Servers (\siehe{anschaffung-des-servers}) erwähnt, wird uns von der Schule ein eigener Servercluster mit virtuellen Maschinen zur Verfügung gestellt. Durch die ProxMox-Umgebung und diversen Tools, ging die Installation der Ubuntu-Distribution rasch von der Hand. In der Virtualisierungsumgebung der Schule musste nur ein vorhandenes Linux-Ubuntu 18.04 ISO-File gemountet und anschließend eine gewöhnliche Betriebsysteminstallation für Ubuntu durchgeführt werden. 
Jedoch kam es beim ersten Versuch zu Problemen mit der Konfiguration der Netzwerkschnittstellen, die im nächsten Punkt genauer erläutert werden.

## Internetkonnektivität der Maschine

### Konfiguration
Im Rahmen des Laborunterrichts an der HTL Rennweg, bekamen die Schüler für diverse Unklarheiten ein sogenanntes Cheat-Sheet \cite{cheat} für Linux-Befehle zur Verfügung gestellt. In diesem Cheat-Sheet finden sich unteranderem Anleitungen für die Konfiguration einer Netzwerkschnittstelle auf einer CentOS/RedHat sowie Ubuntu/Debian-Distribution.
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
	

## Installation der notwendigen Applikationen
Damit die Ubuntu-Maschine für den Produktivbetrieb startbereit ist, müssen im Vorhinein noch einige wichtige Konfigurationen durchgeführt werden. Die wichtigste (Netzwerkkonfiguration) wurde soeben ausführlich erläutert doch ohne der Installation von diversen Applikationen, wäre das System nicht brauchbar.

### Advanced Packaging Tool

Jeder Ubuntu Benutzer kennt es. Mit diesem Tool werden auf dem System die notwendigen Applikationen heruntergeladen, extrahiert und anschließend installiert. Insgesamt stehen einem 18 apt-get commands zur Verfügung. Genauere Erklärungen sowie die Syntax zu den wichtigsten commands folgen.

#### apt-get update

Update liest alle in `/etc/apt/sources.list` sowie in `/etc/apt/sources.list.d/` eingetragenen Paketquellen neu ein. Dieser Schritt wird vor allem vor einem upgrade-command oder nach dem Hinzufügen einer neuen Quelle empfohlen, um sich die neusten Informationen für Pakete ansehen zu können.

#### apt-get upgrade

Upgrade bringt alle bereits installierten Pakete auf den neuesten Stand.

#### apt-get install

Install lädt das Paket bzw. die Pakete inklusive der noch nicht installierten Abhängigkeiten (und eventuell der vorgeschlagenen weiteren Pakete) herunter und installiert diese. Außerdem besteht die Möglichkeit, beliebig viele Pakete auf einmal anzugeben, indem sie mittels eines Leerzeichens getrennt werden.

#### apt-get remove

Mit apt-get remove werden nicht mehr benötigte Pakete von einem Ubuntu-System vollständig entfernt.

### Installierte Pakete

#### NGINX

NGINX ist einer der am Häufigsten verwendeten OpenSource-Webserver unter Linux für diverse Webanwendungen. Große Unternehmen wie Cisco, Microsoft, Facebook oder auch IBM verwenden diesen Webserver für deren Zwecke. Unteranderem wird NGINX auch als Reverse-Proxy, HTTP-Cache und Load-Balancer verwendet. Wie genau NGINX für den Produktivbetrieb funktioniert wird in einem eigenen Punkt (\siehe{funktionsweise-der-produktivumgebung}) erläutert.

#### Docker

Docker ist eine frei verwendbare Software, die die Erstellung und den Betrieb von Linux Containern ermöglicht. Wie genau dies funktioniert, wird später etwas später (\siehe{funktionsweise-der-produktivumgebung}) genauer beschrieben und erklärt.

#### docker-compose

Die Verwaltung und Verlinkung von mehreren Containern kann auf Dauer sehr nervenaufreibend sein. Die Lösung dieses Problems nennt sich docker-compose. Wie docker-compose jedoch genau funktioniert, wird ebenfalls wie das Grundkonzept von Docker (\siehe{funktionsweise-der-produktivumgebung}) präziser erläutert.

#### MySQL

MySQL ist ein OpenSource-Datenverwaltungssystem und die Grundlage für die meisten dynamischen Websiten. Darauf werden die Inventurdatensätze der HTL Rennweg gespeichert. Nähere Informationen finden sich ebenfalls während der Erklärung der Funktionsweise des Produktivbetriebes (\siehe{funktionsweise-der-produktivumgebung}) wieder.

#### Redis

Redis ist eine In-Memory-Datenbank mit einer Schlüssel-Wert-Datenstruktur (Key Value Store). Wie auch MySQL handelt es sich um eine OpenSource-Datenbank.

#### Nagios

Nagios ist ein Monitoring-System, mit dem sich verschiedene Geräte und auf solche laufende Dienste (oder auch Eigenschaften) überwachen lassen. Ziel ist es dabei schnell Ausfälle festzustellen und diese dem zuständigen Administrator mit zu teilen, so dass dieser dann schnell darauf reagieren kann.

#### virtualenv

Bei virtualenv handelt es sich um ein Tool, mit dem eine isolierte Python-Umgebung erstellt werden kann. Eine solch isolierte Umgebung besitzt eine eigene Installation von diversen Services und teilt ihre libraries nicht mit anderen virtuellen Umgebungen (im optionalen Fall greifen sie auch nicht auf die global installierten libraries zu). Dies bringt vor allem den großen Vorteil, dass im Testfall virtuelle Umgebungen aufgesetzt werden können, um nicht die globalen Konfigurationen zu gefährden.

#### Python
Python ist einer der Hauptbestandteile auf dem Serversystem der Diplomarbeit "Capentory". Das Backend (=Serveranwendung) basiert wie bereits erwähnt auf dem Python-Framework "Django". Um dieses Framework auf dem System installieren zu können wird jedoch noch ein weiteres "Packaging-Tool", speziell für Python-Module, benötigt.

#### pip

Und dieses Tool nennt sich Pip. Pip ist ein rekursives Akronym für **P**ip **I**nstalls **P**ython und ist, wie bereits erwähnt, das Standardverwaltungswerkzeug für Python-Module. Die Funktion sowie Syntax kann relativ gut mit der von apt verglichen werden.
 
#### uWSGI

Das eigentliche Paket, mit dem der Produktivbetrieb schlussendlich gewährleistet wurde, nennt sich uWSGI. Speziell wurde es für die Produktivbereitstellung von Serveranwendungen (wie eben der Django-Server von Team "Capentory") entwickelt und harmoniert eindrucksvoll mit der Webserver-Software NGINX. Die grundlegende Funktionsweise von uWSGI, sowie eine Erklärung, warum schlussendlich diese Software und nicht Docker verwendet wurde, wird in einem eigenen Teil (\siehe{probleme-der-produktivumgebung}) veranschaulicht.

#### Django

Django ist ein in Python geschriebenes Webframework, auf dem unsere Serveranwendung basiert. Genauere Informationen wurden jedoch schon übermittelt. (\siehe{kurzfassung-der-funktionsweise-von-django-und-ralph})

#### runsslserver

Runsslserver ist ein Python-Paket mit dem eine Entwicklungsumgebung über https erreichbar gemacht werden kann.

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

Zu Beginn dachte Capentory, dass die Umsetzung des Django-Servers in eine Produktivumgebung nicht allzu kompliziert wäre, da von Ralph bereits Dockerfiles für den Produktivbetrieb vorlagen. Dadurch wurde zu Beginn der Arbeit versucht dieses bereits existierendes Dockersystem zu starten, jedoch war im Browser dann nicht der Capentory-Server, sondern die Basislösung von Ralph zu sehen. Der Infrastrukturverantwortliche analysierte anschließend die Docker-Dateien und musste feststellen, dass sie für die Zwecke von Capentory so tatsächlich nicht verwendet werden können. Ralph hat in Verbindung mit dem implementierten Dockersystem einige komplexe, aufeinander zugreifende Skripts verwendet. Vorweg muss gesagt werden, dass Ralph deren Serverlösung als Service anbietet und in den vorliegenden Skripts installieren sie den Ralphserver ohne den Änderungen von Capentory. Daraufhin wird aus dem Webservice ein Docker-Container erstellt und anschließend mit den anderen Docker-Komponenten hochgefahren. Dadurch war im Webbrowser beim Serverabruf, statt der gewollten Serverlösung, die falsche zu sehen. Der Fortschritt des Arbeitspaketes der Produktivumgebung war somit wieder um einiges geschrumpft und es musste schnellstmöglich eine Alternative gefunden werden, da das Umschreiben der Dockerfiles, beziehungsweise der Skripts, ein riesiger Aufwand wäre.

Nach kurzer Recherche stieß man schließlich auf zwei verwendbare Alternativen, nachfolgend genannt und verglichen werden.

### Alternativen

Es  wurde bereits erläutert, warum "Capentory" die Dockerlösung von Ralph nicht verwendet (\siehe{probleme-der-produktivumgebung}). Allerdings musste rasch eine Alternative für die Umsetzung des Produktivbetriebs gesucht werden, um möglichst wenig Zeit zu verlieren.
Zwei Alternativen, die für den Produktivbetrieb in Frage kamen, wurden genauer analysiert: uWSGI sowie Gunicorn.

### uWSGI vs. Gunicorn

Bei beiden Alternativen handelt es sich um Schnittstellen-Spezifikationen, unteranderem für die Programmiersprache Python, die eine Schnittstelle zwischen Webservern und Webframeworks bzw. Web Application Servern festlegen, um die Portabilität von Webanwendungen auf unterschiedlichen Webservern zu fördern.

Gunicorn ist ein Pre-Fork-Worker-Modell\cite{prefork}, das aus Rubys Unicorn-Projekt portiert wurde. Der Gunicorn-Server ist weitgehend kompatibel mit verschiedenen Web-Frameworks, einfach implementiert, spart Serverressourcen und ist recht schnell.

Das uWSGI-Projekt zielt darauf ab, einen vollständigen Stack für den Aufbau von Hosting-Diensten zu entwickeln.

### Wahl

Da beide Alternativen sehr ähnlich sind, lag es an Capentory welche für die Installation der Produktivumgebung ausgewählt wird. Zuerst wurde versucht, alles mithilfe von Gunicorn aufzusetzen. Da dies jedoch mehrmalig Probleme verursachte, entschied man sich für die Verwendung von uWSGI. Nach kurzer Recherche stieß der Infrastrukturverantwortliche auf ein vielversprechendes Tutorial im Internet\cite{tutorialuwsgi}, mit dem die Installation reibungslos verlief. Die genaue Installationsanleitung wurde bereits in der Serverdokumentation niedergeschrieben. Die Funktionsweise, also wie uWSGI genau arbeitet, wirdnachfolgend (\siehe{funktion-von-uwsgi}) anhand einer Grafik erklärt.

### Funktionsweise der Produktivumgebung

####  Funktion von uWSGI

Es wurde bereits desöfteren erklärt warum die vorhandene "Ralph-Dockerlösung" von "Capentory" nicht verwendet wird. (\siehe{probleme-der-produktivumgebung}) Jedenfalls wurde nun die Einrichtung der Produktivumgebung mittels uWSGI erfolgreich durchgeführt. In der folgenden Grafik wird die Funktion von uWSGI genauer dargestellt.

\begin{figure}[ht]
\centering
\includegraphics{uwsgi.png}
\caption{Funktionsweise von uWSGI}
\cite{uwsgi}
\end{figure}

 1. Ein beliebiger Benutzer eines Webbrowsers (zum Beispiel Google Chrome oder Mozilla Firefox) sendet einen sogenannten "Webrequest" auf den https-Port "443" (\siehe{probleme-der-produktivumgebung}).
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

Die erstellten Container enthalten die Datenbankanwendungen, sowie deren Ressourcen und speichern, beziehungsweise stellen eine dauerhafte Verfügbarkeit der erfassten Inventurdaten bereit. 

### Erreichbarkeit mittels HTTPS

Wie es sich für eine Netzwerktechnikklasse gehört, wurde auch an die Erreichbarkeit über HTTPS gedacht. NGINX wurde mit einem sogenannten Self Signed Certificate ausgestattet, um eine sichere Verbindung des Benutzers mit dem Webserver zu gewährleisten. Die Konfiguration von NGINX für den Gebrauch von uWSGI mit HTTPS ist auf Seite 9 der Serverdokumentation zu finden. Im Webbrowser "Microsoft Edge" würde der Aufruf des Servers nun wie folgt aussehen:

\begin{figure}[ht]
\centering
\includegraphics{https.png}
\caption{Aufruf des Servers über HTTPS}
\end{figure}

Der Zertifikatsfehler, der am Bild deutlich zu sehen ist, bedeudet jedoch nichts anderes als dass das Zertifakt des Produktivservers von "Capentory" nicht von einer offiziellen Zertifizierungsstelle signiert wurde. Da der Server aber sowieso nur im Schulnetz der HTL 3 Rennweg beziehungsweise über einen VPN-Tunnel erreichbar ist, war es nicht nötig, sich an solch eine Zertifizierungsstelle zu wenden.

### Verwendung einer .ini-Datei

Bei einem File mit einer .ini-Endung handelt es sich um eine Initialisierungsdatei. Diese beinhaltet beispielsweise Konfigurationsmöglichen die zum Start eines bestimmten Dienstes benötigt werden. Glücklicherweise funktioniert so ein File auch wunderbar mit uWSGI. Durch die Implementierung einer ralph.ini-Datei wurden die Befehlsoptionen des `uwsgi`-Befehls ausgelagert und dieser somit für den Serverstart verkürzt.

Ein kleiner Vergleich:

    uwsgi --socket mysite.sock --module mysite.wsgi --chmod-socket=664 --proceses 10
    
    uwsgi --ini ralph.ini

Beide Befehle liefern schlussendlich das gleiche Ergebnis, jedoch ist der unter Befehl (Verwendung einer .ini-Datei) um einiges kürzer und erspart somit nervernaufreibende Schreibarbeit.

### Neustartverhalten mittels Service

Für den Gebrauch von uWSGi gibt es einige Lösungen, um den automatischen Neustart, beispielsweise bei Eintritt eines Stromausfalls, zu gewährleisten. "Capentory" hat sich, mit der Implementierung eines eigenen "Ralph-Service", für den klassischen Linuxweg entschieden.

#### Systemd

System gibt es seit 2010 und wurde ursprünglich von Lennart Poettering (Red Hat Inc.) entwickelt. Es ist heutzutage der de-facto Standard, daher in nahezu allen Distros standardmäßig inkludiert und hat damit System-V-Init abgelöst (ist aber noch zu 99% mit System-V-Init Skripten abwärtskompatibel). Die ursprüngliche Überlegung: ein schnellerer Systemstart durch Parallelisierung und Verwenden von kompiliertem C-Code statt Shell-Boot-Skripten. Mittlerweile ist Systemd nicht NUR Service-, sondern ganzer System-Manager und daher viel mächtiger als Init-V. Systemd wird als erster Prozess vom Linux-Kernel gestartet und hat daher die PID 1.

#### .service-Datei

    [Unit]
    Description=ralphserver
    After=network.target
    
    [Service]
    ExecStart=/usr/bin/uwsgi /home/capentory/capentory-prod/capentory-ralph/ralph/ralph.ini
    
    Restart=on-failure
    RestartSec=60s
    
    [Install]
    WantedBy=multi-user.target
   
 - **Description**:  Kurzbeschreibung des Dienstes
 - **After=network.target**: Der Dienst wird gestartet, sobald eine Netzwerkverbindung besteht
 - **ExecStart**: Den auszuführenden Shell-Befehl (in diesem Fall wird der uWSGI-Server gestartet)
 - **Restart und RestartSec**: Hier wird der Dienst beispielsweise bei einem unsauberen Beenden des Prozesses, einem Timeout o.ä. neugestartet. Es ist auch möglich, einen Dienst neu zu starten, wenn der Watchdog einen Fehler meldet.
 - **WantedBy=multi-user.target**: Normalbetrieb

#### Start des Service

Nach dem korrekten Erstellen der `ralph.service`-Datei kann dieser als ganz normaler Linux-Service behandelt werden.
Mit 

    systemctl enable ralph
    systemctl start ralph
 
 wird der angelegte Service nun immer, wenn die virtuelle Maschine gestartet wurde, ausgeführt.

## Absicherung der virtuellen Maschine

Ein weiterer wichtiger und sensibler Teil der Serverinfrastruktur ist die Absicherung der virtuellen Maschine gegen Angriffe. Da es sich im Rahmen der Diplomarbeit "Capentory" um geheime Daten der Schulinventur handelt, war es ein Anliegen der Verantwortlichen, dass mit diesen Daten verantwortungsvoll und vorsichtig umgegangen wird.

### Firewall

Weiter oben (\siehe{topologie-des-netzwerkes}) wird der Plan des Netzwerkes veranschaulicht. Die in der Mitte liegende FortiGate-Firewall stellt, wie bereits in oben genannten Punkt erwähnt, die Verfügbarkeit des Servers im Schulnetz bereit. Dadurch dass der Server nur über eine VPN-Verbindung konfiguriert werden kann, denkt man sich bestimmt, dass die virtuelle Maschine schon genug gesichert sei. Jedoch ist es sinnvoll den Server doppelt abzusichern und somit wurden auf der Linux-Maschine ebenfalls noch Firewallregeln konfiguriert.

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

Bei der „Man in the Middle“-Attacke nistet sich ein Angreifer  zwischen den miteinander kommunizierenden Rechnern. Diese Position ermöglicht ihm, den ausgetauschten Datenverkehr zu  kontrollieren und zu manipulieren. Er kann \zB die ausgetauschten Informationen abfangen, lesen, die Weiterleitung kappen usw. Von all dem erfährt der Empfänger aber nichts.

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

Worum es sich bei diesem tollen Tool handelt wurde bereits bei den installierten Paketen (\siehe{nagios}) erklärt. Nun folgt ein vertiefender Einblick in diese Monitoring-Software.

#### Hosts

Hosts sind bei Nagios definierte virtuelle Maschinen, die überwacht werden sollen. "Capentory" überwacht nicht nur den Produktivserver, sondern auch den aufgesetzten Nagios-Server selbst. Falls dieser Probleme aufweist, wird das Team ebenfalls per E-Mail benachrichtigt aber dies wird in einem eigenen Punkt (\siehe{notifications}) näher erläutert.

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

Im Laufe dieses Kapitels wurde der Inhalt von den für die Produktivumgebung zu erstellenden Dateien aufgelistet und Zeile für Zeile erklärt. Da nur mit diesen Dateien alleine jedoch kein Server funktionieren kann, war ein weiteres großes Ziel die Erstellung einer Guideline für Benutzer, damit diese ebenfalls in der Lage sind, den Server der Diplomarbeit "Capentory" für eine Entwicklungsumgebung sowie eine Produktumgebung aufzusetzen. 