
Technische Umsetzung: Server
=============================
Die Inventur- sowie Gegenstandsdaten der HTL Rennweg sollen an einem zentralen Ort verwaltet und geführt werden. Der für diesen Zweck entwickelte Server muss also folgende Anforderungen erfüllen:

* eine einfache Datenbankverwaltung und -verbindung
* das Führen einer Historie aller Zustände der Inventar-Gegenstände
* eine Grundlage für eine Web-Administrationsoberfläche
* die Möglichkeit für Datenimport und -export, etwa als \emph{.xlsx}\index{.xlsx: Format einer Excel Datei} Datei
* eine Grundlage für die Kommunikation mit der Client-Applikation
* hohe Stabilität und Verfügbarkeit

Angesichts der Programmiersprachen, in die das Projektteam spezialisiert ist, stehen als  Backend-Lösung vier \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} zur öffentlichen Verfügung, zwischen denen gewählt wurde:

* Django \cite{django}
* Pyramid \cite{pyramid}
* Web2Py \cite{web2py}
* Flask \cite{flask}

Alle genannten Alternativen sind \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} der Programmiersprache Python. Gewählt wurde "Django" aufgrund einer bestehenden und frei verfügbaren Inventarverwaltungsplattform "Ralph" \cite{ralph}, welche auf Django aufbaut und durch die vorliegende Diplomarbeit hinreichend erweitert wird. 


## Django und Ralph

Django ist ein in der Programmiersprache Python geschriebenes Webserver-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}. Ralph ist eine auf dem Django-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} basierende \emph{DCIM}\index{DCIM: Data Center Infrastructure Management - Software, die zur Verwaltung von Rechenzentren entwickelt wird } und \emph{CMDB}\index{CMDB: Configuration Management Database - Eine Datenbank, die für die Konfiguration von IT-Geräten entwickelt ist \cite{cmdb}} Softwarelösung. Haupteinsatzgebiet dieser Software sind vor allem Rechenzentren mit hoher Komplexität, die externe Verwaltungsplattformen benötigen. Zusätzlich können aber auch herkömmliche Inventardaten von IT-spezifischen Gegenständen in die Ralph-Plattform aufgenommen werden. 

Ralph wurde von der polnischen Softwarefirma "Allegro" entwickelt und ist unter der Apache 2.0 Lizenz öffentlich verfügbar. Dies ermöglicht auch Veränderungen und Erweiterungen. Zu Demonstrationszwecken bietet Allegro eine öffentlich nutzbare Demo-Version \cite{ralph-demo} von Ralph an.

Die vorliegende Diplomarbeit bietet eine Erweiterung des Ralph-Systems.

### Begründung der Wahl von Django und Ralph
Django bietet eine weit verbreitete Open-Source Lösung für die Entwicklung von Web-Diensten. Bekannte Webseiten, die auf Django basieren sind u.a. Instagram, Mozilla, Pinterest und Open Stack. \cite{django-overview}
Django zeichnet sich besonders durch die sog. \emph{"Batteries included"}\index{Batteries included: Das standardmäßige Vorhandensein von erwünschten bzw. gängigen \emph{Features}\index{Feature: Eigenschaft bzw. Funktion eines Systems}, zu Deutsch: Batterien einbezogen} Mentalität aus. Das heißt, dass Django bereits die gängigsten \emph{Features}\index{Feature: Eigenschaft bzw. Funktion eines Systems} eines Webserver-Backends standardmäßig innehat. Diese sind (im Vergleich zu Alternativen wie etwa "Flask") u.a.:

* Authentifikation und Autorisierung, sowie eine damit verbundene Benutzerverwaltung
* Schutz vor gängigen Attacken (wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF}\index{CSRF: Cross-Site-Request-Forgery - eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken \cite{csrf}}\cite{csrf}), siehe [Views][views]

Zusätzlich bietet Ralph bereits einige \emph{Features}\index{Feature: Eigenschaft bzw. Funktion eines Systems}, die die grundgegende Führung und Verwaltung eines herkömmlichen Inventars unterstützen (beispielsweise eine Suchfunktion mit automatischer Textvervollständigung). 

## Kurzfassung der Funktionsweise von Django und Rlaph
Im folgenden Kapitel wird die Funktionsweise des Django-\emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}, sowie Ralph beschrieben. Ziel dieses Kapitels ist es, eine Wissensbasis für die darauffolgenden Kapitel zu schaffen.

### Datenbank-Verbindung, Pakete und Tabellen-Definition
Folgende Datenbank-Typen werden von Django unterstützt:

* PostgreSQL
* MariaDB
* MySQL
* Oracle
* SQLite

Die Konfiguration der Datenbank-Verbindung geschieht unter Standard-Django in der Datei `settings.py`
, unter Ralph in der jeweiligen Datei im Verzeichnis `settings`.
Eine detaillierte Anleitung zur Verbindung mit einer Datenbank ist in der offiziellen Django-Dokumentation \cite{django-doku-db} zu finden.


Die verschiedenen Funktionsbereiche des Servers sind in Pakete bzw. Module gegliedert. Jedes Paket ist ein Ordner, der verschiedene Dateien und Unterordner beinhalten kann. Die Dateinamen-Nomenklatur eines Packets ist normiert.\cite{django-file-nomenklatur} Der Name eines Pakets wird fortlaufend "App-Label" genannt. Standardmäßig ist dieser Name erster Bestandteil einer URL zu einer beliebigen grafischen Administrationsoberfläche des Pakets.
Pakete werden durch einen Eintrag in die Variable `INSTALLED_APPS`
innerhalb der o.a. Einstellungsdatei registriert. Beispiele sind die beiden durch die vorliegende Diplomarbeit registrierten Pakete `"ralph.capentory"` und  `"ralph.stocktaking"`


Ist ein Python-Paket erfolgreich registriert, konnen in der Datei `models.py`
Datenbank-Tabellen als python Klassen[^model-class-inheritance] definiert werden. Diese Klassen werden fortlaufend als "Modell" bezeichnet. 
Tabellenattribute werden als Attribute dieser Klassen definiert und sind jeweils Instanzen der Klasse `Field`\cite{django-doku-models}[^field-class-inheritance]. Datenbankeinträge können demnach als Instanzen der Modellklassen betrachtet und behandelt werden. Standardmäßig besitzt jedes Modell ein Attribut `id`, welches als \emph{primärer Schlüssel}\index{primärer Schlüssel: engl. "primary key", abgek. "pk" - ein Attribut, das einen Datensatz eindeutig identifiziert} dient. Der Wert des `id` Attributs ist unter allen Instanzen eines Modells einzigartig. Die Anpassung dieses Attributs wird in der offiziellen Django-Dokumentation genauer behandelt. \cite{django-doku-models}

Jedes Modell benötigt eine innere Klasse `Meta`. Sie beschreibt  die \emph{Metadaten}\index{Metadaten: Daten, die einen gegebenen Datensatz beschreiben, beispielsweise der Autor eines Buches} der Modellklasse. Dazu gehört vor allem der von Benuzern lesbare Name des Modells `verbose_name`. \cite{django-doku-models-options}

[^model-class-inheritance]: erbend von der Superklasse `Model`\cite{django-doku-models}
[^field-class-inheritance]: oder davon erbende Klassen

### Administration über das Webinterface

Um die Administration von Modelldaten über das Webinterface des Servers zu ermöglichen, werden grundsätzlich zwei Ansichten der Daten benötigt: Eine Listenansicht aller Datensätze und eine Detailansicht einzelner Datensätze. 

Die Listenansicht aller Datensätze eines Modells wird in der Datei `admin.py` als \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ModelAdmin`[^ralph-admin] definiert. Attribute dieser Klasse beeinflussen das Aussehen und die Funktionsweise der Weboberfläche. Durch  das Setzen von `list_display` werden beispielsweise die in der Liste anzuzeigenden Attribute definiert. 

Die Detailansicht einzelner Datensätze wird grundsätzlich durch die `ModelAdmin` Klasse automatisch generiert, kann aber durch Setzen dessen `form` Attributs auf eine eigens definierte \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ModelForm`[^ralph-adminform]  angepasst werden. Diese Klassen werden in der Datei `forms.py` definiert und besitzen, ähnlich der `Model` Klasse, auch eine innere Klasse  `Meta`.

Um die `ModelAdmin`  \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} über eine URL erreichbar zu machen, müssen diese registriert werden. Dies geschiet durch den `register` \emph{Dekorator}\index{Dekorator: Fügt unter Python einer Klasse oder Methode eine bestimmte Funktionsweise hinzu \cite{python-decorators}}. Dieser Dekorator akzeptiert die zu registrierende Modellklasse, die zu dem `ModelAdmin` gehört, als Parameter.
Die Listenansicht einer registrierten `ModelAdmin` Subklasse ist standardmäßig unter der URL 
```
/<App-Label>/<Modell-Name>/
```
erreichbar, die Detailansicht einer Modellinstanz unter der URL 
```
/<App-Label>/<Modell-Name>/<Modellinstanz-ID>/
```
. Somit repräsentiert die URL der Listenansicht gleichzeitig den Pfad, unter der sie definiert wurde. 

Die Dokumentation der Administrationsfeatures von Django ist auf der offizielen Dokumentationswebseite von Django  \cite{django-doku-admin} zu finden.

[^ralph-admin]: Unter Ralph steht hierfür die Klasse `RalphAdmin` zur Verfügung.\cite{ralph-admin-doku}
[^ralph-adminform]: Unter Ralph steht hierfür die Klasse `RalphAdminForm` zur Verfügung.

### API und DRF

Um Daten außerhalb der grafischen Administrationsoberfläche zu bearbeiten, wird eine \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} benötigt. Eine besondere und weit verbreitete Form einer API ist eine \emph{REST-API} \index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} - eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} \cite{rest-api}, die unter Django durch das integrierte \emph{DRF}\index{DRF: Django REST Framework - Implementierung einer \emph{REST-API}\index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} - eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} unter Django \cite{django-rest-framework}} implementiert wird.\cite{django-rest-framework}
API Definitionen werden unter Django in einem Paket in der Datei `api.py` getätigt. 

Um den API-Zugriff auf ein Modell zu ermöglichen werden üblicherweise eine `APIView`[^ralph-viewset] \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} und eine `Serializer`[^ralph-serializer] \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} definiert. `APIView` Klassen sind zuständig für das Abarbeiten von Anfragen mithilfe einer `Serializer` Klasse, die die Daten aus der Datenbank repräsentiert und in das gewünschte Format konvertiert. Durch `APIView` Klassen werden Berechtigungen und sonstige Attribute definiert, die sich auf das wahrgenommene Erscheinungsbild des Servers auf einen Client auswirken. Beispiel dafür ist die Art der \emph{Paginierung}\cite{django-rest-framework}\index{Paginierung: engl. pagination - Die Aufteilung von Datensätzen in diskrete Seiten \cite{django-rest-framework-pagination}}.  Die erstellten `APIView` Klassen können dann mithilfe einer `Router`[^ralph-router] Instanz registriert werden. Anleitungen zur Erstellung dieser API-Klassen sind auf der offizielen Webseite des DRF \cite{django-rest-framework} und der offiziellen Ralph-Dokumentationsseite \cite{ralph-api-doku} zu finden.

[^ralph-serializer]: Unter Ralph steht hierfür die Klasse `RalphAPIViewSet` zur Verfügung. \cite{ralph-api-doku}
[^ralph-viewset]: Unter Ralph steht hierfür die Klasse `RalphAPISerializer` zur Verfügung. \cite{ralph-api-doku}
[^ralph-router]: Unter Ralph steht hierfür die globale `RalphRouter` Instanz `router`zur Verfügung. \cite{ralph-api-doku}

### Views

Schnittstellen, die keiner der beiden o.a. Kategorien zugeordnet werden können, werden in der Datei `views.py` definiert. Bei diesen \emph{generischen}\index{generisch: in einem allgemeingültigen Sinn} Schnittstellen handelt es sich entweder um \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} der Klasse `View`[^view-apiview-inheritance] \cite{django-doku-class-based-views} oder vereinzelte Methoden mit einem `request`[^request-german] Parameter. \cite{django-doku-views}
Diese Schnittstellen werden fortlaufend Views genannt.

Soll ein View als Antwort auf eine Anfrage HTML-Daten liefern, so sollte dazu ein \emph{Template}\index{Template: zu Deutch: Vorlage, Schablone} verwendet werden. Mithilfe von Templates können Daten, die etwa durch Datenbankabfrage entstehen, zu einer HTML Antwort aufbereitet werden. Besonders ist hierbei die zusätzlich zu HTML verfügbare Django-Template-\emph{Syntax}\index{Syntax: Regelwerk, sprachliche Einheiten miteinander zu verknüpfen \cite{syntax}} \cite{django-doku-template}. Damit können HTML Elemente auf den Input-Daten basierend dynamisch generiert werden. So stehen beispielsweise `if` Statements direkt in der Definition des Templates zur Verfügung.
Die Benutzung von Templates schützt standardmäßig gegen gängige Attacken, wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF}\index{CSRF: Cross-Site-Request-Forgery - eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken \cite{csrf}}\cite{csrf} und gilt daher als besonders sicher.

Da reguläre Views nicht automatisch registriert werden, müssen sie manuell bekanntegegeben werden. Dies geschieht durch einen Eintrag in die Variable `urlpatterns` in der Datei `urls.py`.  \cite{django-doku-urls}

[^view-apiview-inheritance]: die ebenfalls Superklasse der Klasse `ApiView` ist
[^request-german]: zu Deutch: Anfrage; entspricht dem empfangenen Packet, meist als HTML. 


### Designgrundlagen

Designgrundlagen für Django-Entwickler sind auf der offiziellen Dokumentationsseite von Django abrufbar. \cite{django-doku-coding-style}
Die Erweiterung von Django durch die vorliegende Diplomarbeit wurde anhand dieser Grundlagen entwickelt. 

Das Konzept des `Mixin`s wird von der Ralph-Plattform besonders häufig genutzt. `Mixin`s sind Klassen, die anderen von ihnen erbende Klassen, bestimmte Attribute und Methoden hinzufügen. Manche `Mixin`s setzen implizit voraus, dass die davon erbenden Klassen ebenfalls von bestimmten anderen Klassen erben. Beispiel ist die Klasse `AdminAbsoluteUrlMixin`, welche eine Methode `get_absolute_url` zur Verfügung stellt. Diese Methode liefert die URL, die zu der Detailansicht der Modellinstanz führt, die die Methode aufruft. Voraussetzung für das Erben einer Klasse von `AdminAbsoluteUrlMixin` ist daher, dass sie ebenfalls von der Klasse `Model` erbt.


\todo{Ignore below content for now}
... irgendwo im Intro:
bezüglich folgender zwei Aspekte:
* Eine Inventurfunktion für die Validierung von Gegenständen und die Verwaltung von Änderungen die durch eine Inventur entstehen, sowie die benötigte Kommunikation mit der Client-Applikation, die für die Durchführung von Inventuren benutzt wird.
* Eine zentralisierte Verwaltungsplatform der in der HTL Rennweg geführten Inventarlisten, besonders der SAP-Datenbank.