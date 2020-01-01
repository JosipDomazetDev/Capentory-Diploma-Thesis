
Technische Umsetzung: Server
=============================
Die Inventur- sowie Gegenstandsdaten der HTL Rennweg sollen an einem zentralen Ort verwaltet und geführt werden. Für den dafür eingesetzten Server ergeben sich also bestimmte Anforderungen:

* eine einfache Datenbankverwaltung und -verbindung
* das Führen einer Historie aller Zustände der  Inventar-Gegenständen
* eine Grundlage für eine Web-Administrationsoberfläche
* die Möglichkeit für Daten Import- und Export, etwa als \emph{.xlsx}\index{.xlsx: Format einer Excel Datei} Datei
* eine Grundlage für die Kommunikation mit der Client-Applikation
* hohe Stabilität und Verfügbarkeit

Angesichts der Programmiersprachen, in die das Projektteam spezialisiert ist, stehen als  Backend-Lösung 4 \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt.} zur öffentlichen Verfügung, zwischen denen entschieden wurde:

* Django \cite{django}
* Pyramid \cite{pyramid}
* Web2Py \cite{web2py}
* Flask \cite{flask}

Alle genannten Alternativen sind \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt.} der Programmiersprache Python. Gewählt wurde die Alternative "Django" aufgrund einer bestehenden und frei verfügbaren Inventarverwaltungsplattform "Ralph" \cite{ralph}, welche auf Django aufbaut und durch die vorliegende Diplomarbeit hinreichend erweitert wird. 


## Django und Ralph

Django ist ein in der Programmiersprache Python geschriebenes Webserver-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt.}. Ralph ist eine auf dem Django-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt.} basierende \emph{DCIM}\index{DCIM: Data Center Infrastructure Management} und \emph{CMDB}\index{CMDB: Configuration Management Database} Softwarelösung. Haupteinsatzgebiet der Software sind vor allem Rechenzentren mit hoher Komplexität, die externe Verwaltungsplattformen benötigen. Zusätzlich können aber auch herkömmliche Inventardaten von IT-spezifischen Gegenständen in die Ralph-Plattform aufgenommen werden. 

Ralph wurde von der polnischen Softwarefirma "Allegro" entwickelt und ist unter der Apache 2.0 Lizenz öffentlich verfügbar. Dies ermöglicht auch Veränderungen und Erweiterungen.

Die vorliegende Diplomarbeit bietet eine Erweiterung des Ralph-Systems.

### Begründung der Wahl von Django und Ralph
Django bietet eine weit verbreitete Open-Source Lösung für die Entwicklung von Web-Diensten. Bekannte Webseiten, die auf Django basieren sind u.a. Instagram, Mozilla, Pinterest und Open Stack. \cite{django-overview}
Django zeichnet sich besonders durch die sog. \emph{"Batteries included"}\index{Batteries included: Das standardmäßige Vorhandensein von erwünschten bzw. gängigen Features, zu Deutsch: "Batterien einbezogen"} Mentalität aus. Das heißt, dass Django bereits die gängigsten Features eines Webserver-Backends standardmäßig innehat. Diese sind (im Vergleich zu beispielsweise der Alternative "Flask") u.a.

* Authentifikation und Autorisierung, sowie eine damit verbundene Benutzerverwaltung
* Schutz vor gängigen Attacken (wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF}\index{CSRF: Cross-Site-Request-Forgery - eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken.\cite{csrf}}\cite{csrf} , siehe \todo{[Verweis auf Kapitel Django-Forms und Templates]})

Zusätzlich bietet Ralph bereits einige Features, die die grundgegende Führung und Verwaltung eines herkömmlichen Inventars unterstützen (beispielsweise eine Suchfunktion mit automatischer Textvervollständigung). 

## Kurzfassung der Funktionsweise von Django und Rlaph
Im folgenden Kapitel wird die Funktionsweise des Django-\emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt.}, sowie Ralph beschrieben. Ziel dieses Kapitels ist es, eine Wissensbasis für die darauffolgenden Kapitel zu schaffen.

### Datenbank-Verbindung, Pakete und Tabellen-Definition
Folgende Datenbank-Arten werden von Django unterstützt:

* PostgreSQL
* MariaDB
* MySQL
* Oracle
* SQLite

Die Konfiguration der Datenbank-Verbindung geschieht unter Standard-Django in der Datei `settings.py`
, unter Ralph in der jeweiligen Datei im Verzeichnis `settings`
Eine detaillierte Anleitung zur Verbindung mit einer Datenbank ist in der offiziellen Django-Dokumentation\cite{django-doku-db} zu finden.


Die verschiedenen Funktionsbereiche des Servers sind in Paketen bzw. Module gegliedert. Jedes Paket ist ein Ordner, der verschiedene Dateien und Unterordner beinhalten kann. Die Dateinamen-Nomenklatur eines Packets ist normalisiert\cite{django-file-nomenklatur}. Der Name eines Pakets wird fortläufig App-Label genannt. Standardmäßig ist dieser Name erster Bestandteil einer URL zu einer beliebigen grafischen Administrationsoberfläche des Pakets.
Pakete werden durch einen Eintrag in die Variable `INSTALLED_APPS`
innerhalb der o.a. Einstellungsdatei registriert. Beispiele sind die beiden durch die vorliegende Diplomarbeit registrierten Pakete `"ralph.capentory"` und  `"ralph.stocktaking"`


Ist ein Python-Paket erfolgreich registriert, konnen in der Datei `models.py`
Datenbank-Tabellen als python Klassen (abgeleitet von der Superklasse `Model`\cite{django-doku-models} definiert werden. Diese Klassen werden daher fortlaufend als "Modell" bezeichnet. 
Tabellenattribute werden als Attribute dieser Klassen definiert und sind jeweils Instanzen der Superklasse `Field`\cite{django-doku-models}. Datenbankeinträge können demnach als python Objekte betrachtet und behandelt werden. 

Jedes Modell benötigt eine innere Klasse `Meta`. Sie beschreibt  die \emph{Metadaten}\index{Metadaten: Daten, die einen gegebenen Datensatz beschreiben, beispielsweise der Autor eines Buches}. Dazu gehört vor allem der von Benuzern lesbare Name des Modells `verbose_name`. \cite{django-doku-models-options}

### API und DRF

Um Daten außerhalb der grafischen Administrationsoberfläche zu bearbeiten, wird eine \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} benötigt. Eine besondere und weit verbreitete Form einer API ist eine \emph{REST-API} \index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} - eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} \cite{rest-api}, die unter Django durch das integrierte \emph{DRF} \index{DRF: Django REST Framework - Implementierung einer \emph{REST-API}\index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} unter Django} implementiert wird.\cite{django-rest-framework}
API Definitionen werden unter Django in einem Paket in der Datei `api.py` getätigt. 

\todo{Ignore below content for now}
... irgendwo im Intro:
bezüglich folgender 2 Aspekte:
* Eine Inventurfunktion für die Validierung von Gegenständen und die Verwaltung von Änderungen die durch eine Inventur entstehen, sowie die benötigte Kommunikation mit der Client-Applikation, die für die Durchführung von Inventuren benutzt wird.
* Eine zentralisierte Verwaltungsplatform der in der HTL Rennweg geführten Inventarlisten, besonders der SAP-Datenbank.