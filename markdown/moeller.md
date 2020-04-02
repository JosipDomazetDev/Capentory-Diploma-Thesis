\chapter{Einführung in die Server-Architektur}
\label{intro_server}
\renewcommand{\kapitelautor}{Autor: Mathias Möller}

Die Inventur- sowie Gegenstandsdaten der HTL Rennweg sollen an einem zentralen Ort verwaltet und geführt werden. Der für diesen Zweck entwickelte Server muss also folgende Anforderungen erfüllen:

* eine einfache Datenbankverwaltung und -verbindung
* das Führen einer Historie aller Zustände der Inventar-Gegenstände
* eine Grundlage für eine Web-Administrationsoberfläche
* die Möglichkeit für Datenimport und -export, etwa als \emph{.xlsx}\index{.xlsx: Format einer Excel Datei} Datei
* eine Grundlage für die Kommunikation mit der Client-Applikation
* hohe Stabilität und Verfügbarkeit

Angesichts der Programmiersprachen, auf die das Projektteam spezialisiert ist, stehen als  Backend-Lösung vier \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} zur öffentlichen Verfügung, zwischen denen gewählt wurde:

* Django \cite{django}
* Pyramid \cite{pyramid}
* Web2Py \cite{web2py}
* Flask \cite{flask}

Alle genannten Alternativen sind \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} in der Programmiersprache Python. Sie wurden anhand der Faktoren Funktionsumfang, verfügbarer Dokumentation und Anzahl aktiver EntwicklerInnen verglichen. Django übertrifft alle anderen Alternativen in jedem dieser Punkte und gilt daher als der Standard eines solchen \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}. Die zu Django ähnlichste Alternative bezüglich Funktionsumfang ist Pyramid. Die komplexere Integration von kundenspezifischen Funktionen, der geringere Umfang öffentlich verfügbarer Dokumentation und die wesentlich geringere Anzahl aktiver Entwickler sind klare Nachteile gegenüber Django. Web2Py kann mittlerweile als veraltet eingestuft werden. Die Dokumentation entspricht nicht dem aktuellen Stand der Technik und die Entwicklerbasis ist größtenteils auf andere \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} umgestiegen. Flask fällt unter die Kategorie der Microframeworks und besitzt daher nur einen sehr geringen Funktionsumfang, der entweder durch Einsatz von Fremdsoftwarepaketen oder eigene Implementierung auf das benötigte Niveau gehoben werden muss. Dadurch wird eine hohe Anpassbarkeit und hohe Performanz  versprochen, jedoch ist das Endresultat meist in Django effizienter implementierbar. 

Aufgrund des vorliegenden Vergleichs und einer bestehenden und frei verfügbaren Inventarverwaltungsplattform "Ralph" \cite{ralph}, die auf Django aufbaut, wurde die Alternative Django gewählt. 

# Begründung der Wahl von Django und Ralph

Django bietet eine weit verbreitete Open-Source Lösung für die Entwicklung von Web-Diensten. Bekannte Webseiten, die auf Django basieren sind \ua{} Instagram, Mozilla, Pinterest und Open Stack \cite{django-overview}.
Django zeichnet sich besonders durch die sog. \emph{"Batteries included"}\index{Batteries included: Das standardmäßige Vorhandensein von erwünschten \bzw{} gängigen \emph{Features}\index{Feature: Eigenschaft \bzw{} Funktion eines Systems}, zu Deutsch: Batterien einbezogen} Mentalität aus. Das heißt, dass Django bereits die gängigsten \emph{Features}\index{Feature: Eigenschaft \bzw{} Funktion eines Systems} eines Webserver-Backends standardmäßig innehat. Diese sind (im Vergleich zu Alternativen wie etwa Flask) \ua{}:

* Authentifikation und Autorisierung, sowie eine damit verbundene Benutzerverwaltung
* Schutz vor gängigen Attacken (wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF}\index{CSRF: Cross-Site-Request-Forgery -- eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken \cite{csrf}} \cite{csrf}), \siehe{views}.

Zusätzlich bietet Ralph bereits einige \emph{Features}\index{Feature: Eigenschaft \bzw{} Funktion eines Systems}, die die grundlegende Führung und Verwaltung eines herkömmlichen Inventars unterstützen (beispielsweise eine Suchfunktion mit automatischer Textvervollständigung). Ralph ist daher eine angemessene Basis für die vorliegende Diplomarbeit. 

# Kurzfassung der Funktionsweise von Django und Ralph

## Einleitung

Django ist ein in der Programmiersprache Python geschriebenes Webserver-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}. Ralph ist eine auf dem Django-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} basierende \emph{DCIM}\index{DCIM: Data Center Infrastructure Management -- Software, die zur Verwaltung von Rechenzentren entwickelt wird } und \emph{CMDB}\index{CMDB: Configuration Management Database -- Eine Datenbank, die für die Konfiguration von IT-Geräten entwickelt ist \cite{cmdb}} Softwarelösung. Haupteinsatzgebiet dieser Software sind vor allem Rechenzentren mit hoher Komplexität, die externe Verwaltungsplattformen benötigen. Zusätzlich können aber auch herkömmliche Inventardaten von IT-spezifischen Gegenständen in die Ralph-Plattform aufgenommen werden. 

Ralph wurde von der polnischen Softwarefirma "Allegro" entwickelt und ist unter der Apache 2.0 Lizenz öffentlich verfügbar. Dies ermöglicht auch Veränderungen und Erweiterungen. Zu Demonstrationszwecken bietet Allegro eine öffentlich nutzbare Demo-Version \cite{ralph-demo} von Ralph an.

Die vorliegende Diplomarbeit bietet eine Erweiterung des Ralph-Systems.

In diesem Kapitel wird die Funktionsweise des Django-\emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}, sowie Ralph beschrieben. Ziel dieses Kapitels ist es, eine Wissensbasis für die darauffolgenden Kapitel zu schaffen.

## Datenbank-Verbindung, Pakete und Tabellen-Definition
Folgende Datenbank-Typen werden von Django unterstützt:

* PostgreSQL
* MariaDB
* MySQL
* Oracle
* SQLite

Die Konfiguration der Datenbank-Verbindung geschieht unter Standard-Django in der Datei `settings.py` und unter Ralph in der jeweiligen Datei im Verzeichnis `settings`.
Eine detaillierte Anleitung zur Verbindung mit einer Datenbank ist in der offiziellen Django-Dokumentation \cite{django-doku-db} zu finden.

Die verschiedenen Funktionsbereiche des Servers sind in Pakete \bzw{} Module gegliedert. Jedes Paket ist ein Ordner, der verschiedene Dateien und Unterordner beinhalten kann. Die Dateinamen-Nomenklatur eines Pakets ist normiert \cite{django-file-nomenklatur}. Der Name eines Pakets wird fortan "App-Label" genannt. Standardmäßig ist dieser Name erster Bestandteil einer \emph{URL} \index{URL: Addressierungsstandard im Internet} zu einer beliebigen graphischen Administrationsoberfläche des Pakets.
Pakete werden durch einen Eintrag in die Variable `INSTALLED_APPS`
innerhalb der \oa{} Einstellungsdatei registriert. Beispiele sind die beiden durch die vorliegende Diplomarbeit registrierten Pakete `ralph.capentory` und  `ralph.stocktaking`

Ist ein Python-Paket erfolgreich registriert, können in der Datei `models.py`
Datenbank-Tabellen als Python Klassen[^model-class-inheritance] definiert werden. Diese Klassen werden fortan als "Modell" bezeichnet. 
Tabellenattribute werden als Attribute dieser Klassen definiert und sind jeweils Instanzen der Klasse `Field`\cite{django-doku-models}[^field-class-inheritance]. Datenbankeinträge können demnach als Instanzen der Modellklassen betrachtet und behandelt werden. Standardmäßig besitzt jedes Modell ein Attribut `id`, welches als \emph{primärer Schlüssel}\index{primärer Schlüssel: engl. "primary key", abgekürzt "pk" -- ein Attribut, das einen Datensatz eindeutig identifiziert} dient. Der Wert des `id` Attributs ist unter allen Instanzen eines Modells einzigartig. Die Anpassung dieses Attributs wird in der offiziellen Django-Dokumentation genauer behandelt \cite{django-doku-models}.

Jedes Modell benötigt eine innere Klasse `Meta`. Sie beschreibt  die \emph{Metadaten}\index{Metadaten: Daten, die einen gegebenen Datensatz beschreiben, beispielsweise der Autor eines Buches} der Modellklasse. Dazu gehört vor allem der von Benutzern lesbare Name des Modells `verbose_name` \cite{django-doku-models-options}.

[^model-class-inheritance]: erbend von der Superklasse `Model`\cite{django-doku-models}
[^field-class-inheritance]: oder davon erbende Klassen

## Administration über das Webinterface

Um die Administration von Modelldaten über das Webinterface des Servers zu ermöglichen, werden grundsätzlich zwei Ansichten der Daten benötigt: Eine Listenansicht aller Datensätze und eine Detailansicht einzelner Datensätze. 

Die Listenansicht aller Datensätze eines Modells wird in der Datei `admin.py` als \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ModelAdmin`[^ralph-admin] definiert. Attribute dieser Klasse beeinflussen das Aussehen und die Funktionsweise der Weboberfläche. Durch  das Setzen von `list_display` werden beispielsweise die in der Liste anzuzeigenden Attribute definiert. 

Die Detailansicht einzelner Datensätze wird grundsätzlich durch die `ModelAdmin` Klasse automatisch generiert, kann aber durch Setzen dessen `form` Attributs auf eine eigens definierte \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ModelForm`[^ralph-adminform]  angepasst werden. Diese Klassen werden in der Datei `forms.py` definiert und besitzen, ähnlich der `Model` Klasse, auch eine innere Klasse  `Meta`.

Um die `ModelAdmin`  \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} über eine \emph{URL} \index{URL: Addressierungsstandard im Internet} erreichbar zu machen, müssen diese registriert werden. Dies geschieht durch den `register` \emph{Dekorator}\index{Dekorator: Fügt unter Python einer Klasse oder Methode eine bestimmte Funktionsweise hinzu \cite{python-decorators}}. Dieser Dekorator akzeptiert die zu registrierende Modellklasse, die zu dem `ModelAdmin` gehört, als Parameter.
Die Listenansicht einer registrierten `ModelAdmin` Subklasse ist standardmäßig unter der \emph{URL} \index{URL: Addressierungsstandard im Internet} 

`/<App-Label>/<Modell-Name>/`

erreichbar und die Detailansicht einer Modellinstanz unter der \emph{URL} \index{URL: Addressierungsstandard im Internet} 

`/<App-Label>/<Modell-Name>/<Modellinstanz-ID>/` .

Die Dokumentation der Administrationsfeatures von Django ist auf der offiziellen Dokumentationswebseite von Django  \cite{django-doku-admin} zu finden.

[^ralph-admin]: Unter Ralph steht hierfür die Klasse `RalphAdmin` zur Verfügung \cite{ralph-admin-doku}.
[^ralph-adminform]: Unter Ralph steht hierfür die Klasse `RalphAdminForm` zur Verfügung.

## API und DRF

Um Daten außerhalb der graphischen Administrationsoberfläche zu bearbeiten, wird eine \emph{API}\index{API: Application-Programming-Interface -- Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} benötigt. Eine besondere und weit verbreitete Form einer API ist eine \emph{REST-API} \index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface -- Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} -- eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} \cite{rest-api}, die unter Django durch das integrierte \emph{DRF}\index{DRF: Django REST Framework -- Implementierung einer \emph{REST-API}\index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface -- Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} -- eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} unter Django \cite{django-rest-framework}} implementiert wird \cite{django-rest-framework}.
API Definitionen werden unter Django in einem Paket in der Datei `api.py` getätigt. 

Um den API-Zugriff auf ein Modell zu ermöglichen werden üblicherweise eine `APIView`[^ralph-viewset] \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} und eine `Serializer`[^ralph-serializer] \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} definiert. `APIView` Klassen sind zuständig für das Abarbeiten von Anfragen mithilfe einer `Serializer` Klasse, die die Daten aus der Datenbank repräsentiert und in das gewünschte Format konvertiert. Durch `APIView` Klassen werden Berechtigungen und sonstige Attribute definiert, die sich auf das wahrgenommene Erscheinungsbild des Servers auf einen Client auswirken. Beispiel dafür ist die Art der \emph{Paginierung} \cite{django-rest-framework}\index{Paginierung: engl. pagination -- Die Aufteilung von Datensätzen in diskrete Seiten \cite{django-rest-framework-pagination}}.  Die erstellten `APIView` Klassen können dann mithilfe einer `Router`[^ralph-router] Instanz registriert werden. Anleitungen zur Erstellung dieser API-Klassen sind auf der offiziellen Webseite des DRF \cite{django-rest-framework} und der offiziellen Ralph-Dokumentationsseite \cite{ralph-api-doku} zu finden.

[^ralph-serializer]: Unter Ralph steht hierfür die Klasse `RalphAPIViewSet` zur Verfügung  \cite{ralph-api-doku}.
[^ralph-viewset]: Unter Ralph steht hierfür die Klasse `RalphAPISerializer` zur Verfügung \cite{ralph-api-doku}.
[^ralph-router]: Unter Ralph steht hierfür die globale `RalphRouter` Instanz `router` zur Verfügung \cite{ralph-api-doku}.

## Views

Schnittstellen, die keiner der beiden \oa{} Kategorien zugeordnet werden können, werden in der Datei `views.py` definiert. Bei diesen \emph{generischen}\index{generisch: in einem allgemeingültigen Sinn} Schnittstellen handelt es sich entweder um \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} der Klasse `View`[^view-apiview-inheritance] \cite{django-doku-class-based-views} oder vereinzelte Methoden mit einem `request`[^request-german] Parameter \cite{django-doku-views}.
Diese Schnittstellen werden fortan Views genannt.

Soll ein View als Antwort auf eine Anfrage HTML-Daten liefern, so sollte dazu ein \emph{Template}\index{Template: zu Deutch: Vorlage, Schablone} verwendet werden. Mithilfe von Templates können Daten, die etwa durch Datenbankabfrage entstehen, zu einer HTML Antwort aufbereitet werden. Besonders ist hierbei die zusätzlich zu HTML verfügbare Django-Template-\emph{Syntax}\index{Syntax: Regelwerk, sprachliche Einheiten miteinander zu verknüpfen \cite{syntax}} \cite{django-doku-template}. Damit können HTML Elemente auf den Input-Daten basierend dynamisch generiert werden. So stehen beispielsweise `if` Statements direkt in der Definition des Templates zur Verfügung. Die Benutzung von Templates schützt standardmäßig gegen Attacken, wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF} \index{CSRF: Cross-Site-Request-Forgery -- eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken \cite{csrf}}\cite{csrf} und gilt daher als besonders sicher. Durch das Diplomarbeitsteam wurden weitere Möglichkeiten zur Sicherung des Serversystems \cite{django-doku-security} implementiert und alle Sicherheitsempfehlungen der Entwickler von Django \cite{django-doku-security} eingehalten.

Da reguläre Views nicht automatisch registriert werden, müssen sie manuell bekanntgegeben werden. Dies geschieht durch einen Eintrag in die Variable `urlpatterns` in der Datei `urls.py` \cite{django-doku-urls}.

[^view-apiview-inheritance]: die ebenfalls Superklasse der Klasse `ApiView` ist
[^request-german]: zu Deutch: Anfrage; entspricht den empfangenen Daten

## Datenbankabfragen

Datenbankabfragen werden in Django durch `Queryset`-Objekte getätigt. Das Definieren eines `Queryset`-Objekts löst nicht sofort eine Datenbankabfrage aus. Erst, wenn Werte aus einem `Queryset`-Objekt gelesen werden, wird eine Datenbankabfrage ausgelöst. So kann ein `Queryset`-Objekt beliebig oft verändert werden, bevor davon ausgelesen wird. Ein Beispiel hierfür ist das Anwenden der `filter()` Methode.

In dem folgenden Code-Auszug[^django-query-example] werden aus dem Modell `Entry` bestimmte Einträge gefiltert: 
```python
# Erstelle ein Queryset aller Entry-Objekte, 
# dessen Attribut "headline" mit "What" beginnt.
q = Entry.objects.filter(headline__startswith="What")

# Filtere aus dem erstellten Queryset alle Entry-Objekte, 
# dessen Attribut "pub_date" kleiner oder gleich 
# dem aktuellen Datum ist.
q = q.filter(pub_date__lte=datetime.date.today())

# Schließe aus dem erstellten Queryset alle Entry-Objekte, 
# dessen Attribut "body_text" den Text "food" beinhaltet, aus.
q = q.exclude(body_text__icontains="food")

# Ausgabe des erstellten Querysets.
# Erst hier kommt es zu der ersten Datenbankabfrage!
print(q)
```
Weitere Beispiele und Methoden sind der offiziellen Django-Dokumentation zu entnehmen \cite{django-doku-queries}.

[^django-query-example]: entnommen aus der offiziellen Django Dokumentation \cite{django-doku-queries}

# Designgrundlagen

Designgrundlagen für Django-Entwickler sind auf der offiziellen Dokumentationsseite von Django \cite{django-doku-coding-style} abrufbar.
Die Erweiterung von Django durch die vorliegende Diplomarbeit wurde anhand dieser Grundlagen entwickelt. 

Das Konzept des `Mixin`s wird von der Ralph-Plattform besonders häufig genutzt. `Mixin`s sind Klassen, die anderen von ihnen erbenden Klassen bestimmte Attribute und Methoden hinzufügen. Manche `Mixin`s setzen implizit voraus, dass die davon erbenden Klassen ebenfalls von bestimmten anderen Klassen erben. Beispiel ist die Klasse `AdminAbsoluteUrlMixin`, die eine Methode `get_absolute_url` zur Verfügung stellt. Diese Methode liefert die \emph{URL}\index{URL: Addressierungsstandard im Internet}, die zu der Detailansicht der Modellinstanz führt, die die Methode aufruft. Voraussetzung für das Erben einer Klasse von `AdminAbsoluteUrlMixin` ist daher, dass sie ebenfalls von der Klasse `Model` erbt.

\chapter{Die zwei Erweiterungsmodule des Serversystems}
\label{die_zwei_module}

Die vorliegende Diplomarbeit erweitert das Ralph-System um 2 Module. Dabei handelt es sich um die beiden Pakete "Capentory" und "Stocktaking". Das Paket "Capentory" behandelt die Führung der Inventardaten und wurde speziell an die Inventardaten der HTL Rennweg angepasst. Das Paket "Stocktaking" ermöglicht die Verwaltung der durch die mobile Applikation durchgeführten Inventuren. Dazu zählen Aufgaben wie das Erstellen der Inventuren, das Einsehen von Inventurberichten oder das Anwenden der aufgetretenen Änderungen.

Dieses Kapitel beschreibt die grundlegende Funktionsweise der beiden Module. Eine Anleitung zur Bedienung der \emph{Weboberfläche} \index{Weboberfläche: graphische Oberfläche für administrative Tätigkeiten, die über einen Webbrowser erreichbar ist} ist dem Handbuch zum Server (\siehe{chap:Anhang-1}) zu entnehmen.

# Das Capentory-Modul

Das Capentory-Modul beherbergt drei wichtige Modelle: 

1. HTLItem
2. HTLRoom
3. HTLItemType

Die wichtigsten Eigenschaften der Modelle und damit verbundenen Funktionsweisen werden in diesem Unterkapitel beschrieben.

## Das HTLItem Modell

Das `HTLItem`-Modell repräsentiert die Gegenstandsdaten des Inventars der HTL Rennweg. Es sind typische Merkmale aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System vertreten. Die Attribute `anlage`, `asset_subnumber` und `company_code` werden direkt aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System übernommen. 

### Die wichtigsten Attribute

Zu den wichtigsten Attributen des `HTLItem` Modells zählen:

* `anlage` und `asset_subnumber`: Diese Attribute bilden den Barcode eines Gegenstandes.
* `barcode_prio`: Wenn dieses Attribut gesetzt ist, überschreibt es den durch die Attribute `anlage` und `asset_subnumber` entstandenen Barcode.
* `anlagenbeschreibung`: Dieses Attribut repräsentiert die aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System entnommene Gegenstandsbeschreibung und kann nur durch den Import von Daten direkt aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System geändert werden.
* `anlagenbeschreibung_prio`: Dieses Attribut dient als interne Gegenstandsbeschreibung, die auch ohne einen Import aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System geändert werden kann.
* `room`: Dieses Attribut referenziert auf ein `HTLRoom` Objekt, in dem sich ein `HTLItem` Objekt befindet.
* `is_in_sap`: Der Wert dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Attributs ist `Wahr`, wenn der `HTLItem`-Datensatz aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System importiert wurde. Umgekehrt ist der Wert dieses Attributes `Falsch`, wenn der `HTLItem`-Datensatz aus einer anderen Quelle entstanden ist. Ein manuell hinzugefügter `HTLItem`-Datensatz hat für dieses Attribut den Wert `Falsch`.
* `item_type`: Dieses Attribut referenziert auf das `HTLItemType` Objekt, das einem `HTLItem` Objekts zugeordnet ist. Es repräsentiert die Gegenstandskategorie eines `HTLItem` Objekts. 


### Einzigartigkeit von HTLItem Objekten

Bezüglich der Einzigartigkeit von `HTLItem` Objekten gelten einige Bestimmungen. 

Sind die Attribute `anlage`,  `asset_subnumber` und `company_code` je mit einem nichtleeren Wert befüllt, so repräsentieren sie ein `HTLItem` Objekt eindeutig. Es dürfen keine 2 `HTLItem` Objekte denselben Wert dieser Attribute haben. Um diese Bedingung erfüllen zu können, muss eine eigens angepasste Validierungslogik implementiert werden. Standardverfahren wäre in diesem Anwendungsfall, die Metavariable `unique_together` \cite{django-doku-models-options} anzupassen:

```python
unique_together = [["anlage",  "asset_subnumber", "company_code"]]
```

Dieses Verfahren erfüllt die geforderte Bedingung nur in der Theorie. Praktisch werden leere Werte von Attributen dieser Art nicht als `None` (Python) \bzw{} `null` (MySQL), sondern als Leerstrings `""` gespeichert. Um diese Werte ebenfalls von der Regel auszuschließen, muss die `validate_unique()` Methode \cite{django-doku-models-instances} überschrieben werden. Die Methode wird unter Normalzuständen immer vor dem Speichern eines Objekts des Modells aufgerufen. Wirft sie einen Fehler auf, wird das Objekt nicht gespeichert und der Benutzer erhält eine entsprechende Fehlermeldung. 

Ist das `barcode_prio` Attribut eines `HTLItem` Objekts gesetzt, darf dessen Wert nicht mit jenem eines anderen `HTLItem` Objekts übereinstimmen. Standardverfahren wäre in diesem Anwendungsfall das Setzen des `unique` Parameters des Attributes auf den Wert `True`. Da dieses Verfahren ebenfalls das \oa{} Problem aufwirft, muss die Logik stattdessen in die `validate_unique()` Methode aufgenommen werden. Zusätzlich darf der Wert des `barcode_prio` Attributs nicht mit dem aus den beiden Attributen `anlage` und `asset_subnumber` generierten Barcode übereinstimmen. Um diese Bedingung zu erfüllen kann nur die `validate_unique()` Methode herbeigezogen werden. 

### Änderungsverlauf

Besonders für das `HTLItem` Modell ist es von besonderer Wichtigkeit, ein Objekt auf den Zustand vor einer unbeabsichtigten Änderung zurücksetzen und gelöschte Objekte wiederherstellen zu können. Durch das bereits in Ralph inkludierte Paket `django-reversion` können die aktuellen Zustände von Datenbankobjekten gesichert werden, um später darauf zugreifen zu können \cite{django-reversion-doku}. Das Paket bietet die Funktion, der graphischen Administrationsoberfläche entsprechende Funktionen zur Wiederherstellung oder Zurücksetzung einzelner Objekte hinzuzufügen. 

Um einen Gegenstand zu entinventarisieren, kann er gelöscht werden. Referenzen auf den gelöschten Gegenstand, die durch eine Inventur entstehen, bleiben in einem ungültigen Zustand erhalten. Der gelöschte Gegenstand kann zu einem späteren Zeitpunkt vollständig wiederhergestellt werden. Die Referenzen auf den wiederhergestellten Gegenstand werden damit wieder gültig. 

### Speichern von Bildern und Anhängen

Die in Ralph verfügbare Klasse `AttachmentsMixin` wird verwendet, um Instanzen der Modellklasse `HTLItem` über dessen graphische Administrationsoberfläche diverse Anhänge zuzuordnen. Ein Anhang ist eine ordinäre Datei, die auf den Server hochgeladen werden kann. Alle hochgeladenen Dateien werden vor dem Speichern anhand deren \emph{Hash-Werten}\index{Hash: Eine Funktion, die für einen Input immer einen (theoretisch) einzigartigen und gleichen Wert generiert.} verglichen. Ist eine Datei mit dem \emph{Hash-Wert}\index{Hash: Eine Funktion, die für einen Input immer einen (theoretisch) einzigartigen und gleichen Wert generiert.} einer hochgeladenen Datei bereits vorhanden, wird die Datei nicht erneut gespeichert und ein Verweis auf die vorhandene Datei wird erstellt. 

## Das HTLRoom Modell

Das `HTLRoom`-Modell repräsentiert die Raumdaten der HTL Rennweg. Es sind typische Merkmale aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System vertreten. Die Attribute `room_number`, `main_inv` und `location` werden direkt aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System übernommen. 

### Die wichtigsten Attribute

Zu den wichtigsten Attributen des `HTLRoom` Modells zählen:

* `room_number`: Dieses Attribut bildet den Barcode eines Raumes.
* `barcode_override`: Wenn dieses Attribut gesetzt ist, überschreibt es den durch das Attribut `room_number` gebildeten Barcode.
* `internal_room_number`: Dieses Attribut repräsentiert die schulinterne Raumnummer und ist somit von den Daten aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System unabhängig.
* `is_in_sap`: Der Wert dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Attributs ist `Wahr`, wenn der `HTLRoom`-Datensatz einen aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System vorhandenen Raum repräsentiert.
* `children`: Mit dieser Beziehung können einem übergeordneten `HTLRoom` Objekt mehrere `HTLRoom` Objekte untergeordnet werden. Anwendungsfall für dieses Attribut ist die Definition von Schränken oder Serverracks, die je einem übergeordneten Raum zugeteilt sind, selbst aber eigenständige Räume repräsentieren.
* `type`: Dieses Attribut spezifiziert die Art eines `HTLRoom` Objekts. So kann ein `HTLRoom` Objekt einen ganzen Raum oder auch nur einen Kasten in einem übergeordneten Raum repräsentieren.
* `item`: Dieses Attribut kann gesetzt werden, um ein `HTLRoom` Objekt durch ein `HTLItem` Objekt zu repräsentieren. Anwendungsbeispiel ist ein Schrank, der sowohl als `HTLRoom` Objekt als auch als `HTLItem` Objekt definiert ist. Sind die beiden Objekte durch das `item` Attribut verbunden, ist der Barcode des `HTLRoom` Objekts automatisch jener des `HTLItem` Objekts.

### Einzigartigkeit von HTLRoom Objekten

Bezüglich der Einzigartigkeit von `HTLRoom` Objekten gelten ähnliche Bestimmungen wie zu jener von `HTLItem` Objekten.

Die Attribute `room_number`, `main_inv` und `location` sind gemeinsam einzigartig. Leere Werte sind von dieser Regel ausgeschlossen. Gleichzeitig darf der Wert des `barcode_override` Attribut nicht mit dem Wert des `room_number` Attributes eines anderen `HTLRoom` Objekts übereinstimmen und vice versa.
Beide Bedingungen müssen wie im Falle des `HTLItem` Modells durch Überschreiben der `validate_unique()` Methode \cite{django-doku-models-instances} implementiert werden. Die Methode wird unter Normalzuständen immer vor dem Speichern eines Objekts des Modells aufgerufen. Wirft sie einen Fehler auf, wird das Objekt nicht gespeichert und der Benutzer erhält eine entsprechende Fehlermeldung. 

### Subräume

Wie im Abschnitt ["Die wichtigsten Attribute"](#die-wichtigsten-attribute-1) festgehalten, können einem `HTLRoom` Objekt mehrere `HTLRoom` Objekte untergeordnet werden. Diese untergeordneten Räume werden "Subräume" genannt. Bei "Subräumen" handelt es sich beispielsweise um einen Kasten, der als eigenständiger Raum in einem ihm übergeordneten Raum steht. Logisch betrachtet ist der Kasten auch nur ein Raum, in dem sich Gegenstände befinden. Ob der Raum ein Klassenraum oder ein Kasten in einem Klassenraum ist, hat keine logischen Auswirkungen auf seine Eigenschaften als "Standort von Gegenständen".

Um eine valide Hierarchie beizubehalten, muss diese Beziehung bei jedem Speicherprozess eines `HTLRoom` Objekts überprüft werden. Das geschieht durch die Methode `clean_children()`, die beim Speichern durch die graphische Administrationsoberfläche automatisch aufgerufen wird. Bei Speichervorgängen, die nicht direkt durch die Administrationsoberfläche initiiert werden[^clean_children_manually], muss `clean_children()` manuell aufgerufen werden.

[^clean_children_manually]: etwa das automatisierte Speichern beim Datenimport

## Das HTLItemType Modell

Das `HTLItemType` Modell repräsentiert Kategorien von Gegenständen. Das Modell besteht aus 2 Eigenschaften. Die Eigenschaft `item_type` beschreibt ein `HTLItemType` kurz, die Eigenschaft `description` bietet Platz für Kommentare. 

Durch das Setzen eines `HTLItemType` Objekts für ein `HTLItem` Objekt durch seine Eigenschaft `item_type` werden dem `HTLItem` Objekt alle \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt unabhängig definierbar sind.} des `HTLItemType` Objekts zugewiesen. Anwendungsbeispiel ist das Setzen eines \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt unabhängig definierbar sind.} namens "Anzahl Ports" für den `HTLItemType` namens "Switch". Jedes `HTLItem` Objekt mit dem `item_type` "Switch" hat nun ein \emph{Custom-Field} namens "Anzahl Ports".

Das für \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt unabhängig definierbar sind.} erforderliche Mixin (\siehe{designgrundlagen}) `WithCustomFieldsMixin` bietet die Funktionalität der `custom_fields_inheritance`. Sie ermöglicht das Erben von allen \emph{Custom-Field} Werten eines bestimmten Objekts an ein anderes. Diese Funktion macht sich das `HTLItem` Modell zunutze. Um beim Speichern automatisch vom `HTLItemType` Objekt unabhängige \emph{Custom-Field} Werte zu erstellen, die sofort vom Benutzer bearbeitet werden können, muss der Speicherlogik eine Funktion hinzugefügt werden. Dazu wird eine `@receiver` Funktion genutzt, die automatisch bei jedem Speichervorgang eines `HTLItemType` oder `HTLItem` Objekts aufgerufen wird:

```python
@receiver(post_save, sender=HTLItemType)
def populate_htlitem_custom_field_values(sender, instance, **kwargs):
    populate_inheritants_custom_field_values(instance)
    
@receiver(post_save, sender=HTLItem)
def populate_htlitemtype_custom_field_values(sender, instance, **kwargs):
    populate_with_parents_custom_field_values(instance)
```

Die beiden angeführten Funktionen werden je beim Aufkommen eines `post_save` Signals \cite{django-doku-signals} ausgeführt, dessen `sender` ein `HTLItemType` oder `HTLItem` ist. Die Funktionen rufen jeweils eine weitere Funktion auf, welche die \emph{Custom-Fields} entsprechend aggregiert. 

## Datenimport

Um die Gegenstands- und Raumdaten des Inventars der HTL Rennweg in das erstellte System importieren zu können, muss dessen standardmäßig verfügbare Importfunktion entsprechend erweitert werden. Dazu sind 4 spezielle Importverhalten notwendig. Die Datenquellen sind das \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System der HTL Rennweg (\siehe{import-aus-dem-sap-erp-system}) und intern von Lehrkräften und Inventarverantwortlichen geführte Listen (\siehe{import-aus-sekunduxe4rer-und-tertiuxe4rer-quelle}). 

Implementiert wird das Importverhalten nicht innerhalb der entsprechenden Modellklasse, sondern in deren verknüpften `ModelAdmin` Klasse. Durch das Attribut `resource_class` wird spezifiziert, mithilfe welcher Python-Klasse die Daten importiert werden. Um für ein einziges Modell mehrere `resource_class` Einträge zu setzen, müssen mehrere `ModelAdmin` Klassen für Proxy-Modelle \cite{django-doku-models} des eigentlichen Modells definiert werden. Ein Proxy-Modell eines Modells verweist auf dieselbe Tabelle in der Datenbank, kann aber programmiertechnisch als unabhängiges Modell betrachtet werden. Die Daten, die durch das Proxy-Modell ausgelesen oder eingefügt werden, entsprechen exakt jenen des eigentlichen Modells.

```python
# Definition eines Proxy-Modells zu dem Modell "HTLItem"
class HTLItemSecondaryImportProxy(HTLItem):
    class Meta:
        proxy = True

# Diese Datenbankabfragen liefern beide dasselbe Ergebnis:
print(HTLItem.objects.all())
print(HTLItemSecondaryImportProxy.objects.all())

# Für das Proxy-Modell kann eine ModelAdmin-Klasse definiert werden.
# Diese bekommt eine eigene "resource_class".
@register(HTLItemSecondaryImportProxy)
class HTLItemSecondaryImportProxyAdmin(HTLItemSecondaryImportMixin, RalphAdmin):
    resource_class = HTLItemSecodaryResource
```

Das Importverhalten wird in der Klasse, die in das Attribut `resource_class` eingetragen wird, programmiertechnisch festgelegt. Es werden alle Zeilen der zu importierenden Datei nacheinander abgearbeitet. Bei sehr großen Datenmengen oder aufwändigem Importverhalten kann es zu Performanceverlusten kommen. Da nahezu jedes durch das Diplomarbeitsteam erstellte Importverhalten die zu importierenden Daten überprüfen oder anderweitig speziell behandeln muss, kann es hier besonders zu Performanceengpässen kommen. Beispielsweise muss beim Import von Daten aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System auch geprüft werden, ob der Raum eines Gegenstandes existiert. Die oft sehr großen Datenmengen, die aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System importiert werden müssen, sorgen ebenfalls für Performanceverluste.

Die in den zu importierenden Dateien vorhandenen Überschriften werden vor dem Importprozess auf Modelleigenschaften abgebildet. Manche Werte der zu importierenden Datensätze müssen in Werte gewandelt werden, die in der Datenbank gespeichert werden können. In der Datei `import_settings.py` sind diese Abbildungen \bzw{} Umwandlungen als "\emph{Aliases} \index{Alias: ein Pseudonym}" deklariert. In dem folgenden Beispiel werden die Überschriften "Erstes Attribut" und "Zweites Attribut" auf die zwei Modelleigenschaften `field_1` und `field_2` abgebildet. Importierte Werte für "Zweites Attribut" werden von den Zeichenketten  "Ja" und "Nein" auf die \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Werte `True` und `False` übersetzt.

```python
# Beispielhafte Definition von Aliases für einen Import
ALIASES_HTLITEM = {
    "field_1": (["Erstes Attribut"], {

    }),
    "field_2": (["Zweites Attribut"], {
      "Ja": True,
      "Nein": False
    }),
```

Der Datenimport wird immer zweimal durchlaufen. Zuerst werden die importierten Daten zwar generiert, aber nicht gespeichert und dem Benutzer nur zur Validierung vorgelegt. Nach einer Bestätigung des Benutzers werden die Daten ein weiteres Mal von Neuem generiert und gespeichert.

Details über das Format einer zu importierenden Quelldatei sind dem Handbuch zum Server (\siehe{chap:Anhang-1}) zu entnehmen.

### Import aus dem SAP ERP System

Die Daten aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System der Schule können in ein gängiges Datenformat exportiert werden. Das üblich gewählte Format ist eine Excel-Tabelle (Dateiendung "`.xlsx`").

Die aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System exportierten Daten haben grundsätzlich immer volle Gültigkeit. Bereits im "Capentory" System vorhandene Daten werden überschrieben. 

Zu importierende Datensätze werden anhand der Werte "BuKr", "Anlage" und "UNr." aus der Quelldatei verglichen. Diese Werte werden auf die `HTLItem` Modellattribute `company_code`, `anlage` und `asset_subnumber` abgebildet. Zusätzlich wird auch das `barcode_prio` Attribut mit dem zusammengefügten Wert der "Anlage" und "UNr." Felder der Quelldatei verglichen. Diese Felder repräsentieren den Barcode eines `HTLItem` Objekts. Stimmt ein `HTLItem` Datensatz aus dem "Capentory" System mit einem zu importierenden Datensatz aufgrund einer der beiden verglichenen Wertepaare überein, wird dieser damit assoziiert und überschrieben. 

Vor dem Verarbeiten der Daten des `HTLItem` Objekts werden die Daten des zugehörigen `HTLRoom` Objekts verarbeitet. Es wird nach einem existierenden `HTLRoom` Objekt mit einem übereinstimmenden `room_number` Attribut gesucht. Das "Raum" Feld der Quelldatei wird auf das `room_number` Attribut abgebildet. Sollten mehrere `HTLRoom` Objekte übereinstimmen[^multiple_matches_case], wird jener mit übereinstimmenden `main_inv` und `location` Attributen ausgewählt. Die "Hauptinven" und "Standort" Felder der Quelldatei werden auf die `main_inv` und `location` Attribute abgebildet. Ein gefundenes `HTLRoom` Objekt wird mit dem importierten `HTLItem` Objekt verknüpft. Sollte kein `HTLRoom` Objekt gefunden werden, wird es mit den entsprechenden Werten erstellt. In beiden Fällen wird das `is_in_sap` \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Attribut des `HTLRoom` Objekts gesetzt. 

Es gibt eine Ausnahme der absoluten Gültigkeit der Daten aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System. Stimmt das gefundene `HTLRoom` Objekt nicht mit jenem aktuell verknüpften `HTLRoom` Objekt eines `HTLItem` Objekts überein, wird es grundsätzlich aktualisiert. Sollte das aktuell verknüpfte `HTLRoom` Objekt ein "Subraum" des gefundenen Objekts sein, wird dieses nicht aktualisiert. So wird verhindert, dass Gegenstände durch den Import aus einem Subraum in den übergeordneten Raum wandern. In dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System existieren die "Subräume" grundsätzlich nicht. 

[^multiple_matches_case]: Dieser Fall sollte bei einem einzigen Schulstandort nicht auftreten.

### Import aus sekundärer und tertiärer Quelle

Bei der sekundären und tertiären Quelle handelt es sich um schulinterne Inventarlisten. Beide Quellen enthalten Informationen über den Raum, in dem sich ein bestimmter Gegenstand befindet. Die sekundäre Quelle enthält Informationen über die Kategorie eines EDV-spezifischen Gegenstands. Die tertiäre Quelle enthält Informationen über "Subräume" und welche Gegenstände sich darin befinden. 

Beide Quellen besitzen allerdings keine absolute Gültigkeit wie der Import aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System. Aus diesem Grund werden alle Änderungen von Eigenschaften eines `HTLItem` Objekts in Änderungsvorschläge einer Inventur ausgelagert und nicht direkt angewendet. Die Änderungen können zu einem späteren Zeitpunkt eingesehen, bearbeitet und schlussendlich angewendet werden. Grund für dieses spezielle Importverhalten ist die Vertrauenswürdigkeit der Informationen, die der sekundären \bzw{} tertiären Quelle entnommen werden. Die Listen haben offiziell kein einheitliches Format und können daher bei sofortigem Übernehmen der Änderungen zu ungewollten Fehlinformationen führen. Der Import einer sekundären oder tertiären Quelle kann wie eine eigenständige Inventur angesehen werden. Es können durch den Import auch neue `HTLItem` Objekte hinzugefügt werden, wenn ein Datensatz mit keinem bestehenden Objekt assoziiert werden kann. 

Das Importverhalten für die sekundäre Quelle erstellt zusätzlich durch die Methode `get_or_create_item_type()` der Klasse `HTLItemSecodaryResource` definierte `HTLItemType` Objekte. Die erstellten `HTLItemType` Objekte werden den `HTLItem` Objekten indirekt über Änderungsvorschläge zugewiesen.

Die bereits erwähnten Informationen über "Subräume" der tertiären Quelle werden sofort auf die entsprechenden `HTLRoom` Objekte angewendet. Es bedarf keiner weiteren Bestätigung, um die "Subräume" zu erstellen und den übergeordneten `HTLRoom` Objekten zuzuweisen.

### Import der Raumliste

Die Importfunktion der Raumliste dient zur Verlinkung von interner Raumnummer (`HTLRoom`-Attribut `internal_room_number`) und der Raumnummer im \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System (`HTLRoom`-Attribut `room_number`). Es werden dadurch bestehenden `HTLRoom` Objekten eine interne Raumnummer und eine Beschreibung zugewiesen, oder gänzlich neue `HTLRoom` Objekte anhand aller erhaltenen Informationen erstellt. Der Import geschieht direkt, ohne Auslagerung von Änderungen in Änderungsvorschläge.

## Datenexport

Die Daten aller `HTLItem` Gegenstände, die aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System importiert wurden können unter spezieller Verarbeitung exportiert werden.  Die exportierten Daten können in das \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System importiert werden und beinhalten \ua{} Raumänderungen, die durch Inventuren aufgetreten sind. Bei der Verarbeitung der zu exportierenden Daten wird eine Funktion des `reversion` Pakets \cite{django-reversion-doku} genutzt. Diese Funktion besteht darin, den Zustand eines Gegenstandes zum Zeitpunkt des letzten Imports aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System abzufragen. Es wird der Zustand zu dem angegebenen Zeitpunkt mit dem aktuellen Zustand verglichen. Anhand der erkannten Änderungen wird die Export-Datei erstellt. 
Weitere Informationen zum Datenexport sind dem Handbuch zum Server (\siehe{chap:Anhang-1}) zu entnehmen. 

# Das Stocktaking-Modul

Das \emph{Stocktaking}\index{Stocktaking: Inventur}-Modul ermöglicht das Erstellen und Verwalten von Inventuren. Durch das neuartige Konzept der Änderungsvorschläge wird die Verwaltung von Inventuren erheblich erleichtert, indem Änderungen effizient angenommen, verworfen und rückverfolgt werden können (\siehe{uxe4nderungsvorschluxe4ge}). 

Um das Datenbankmodell möglichst modular und übersichtlich zu gestalten, werden diverse Modelle miteinander hierarchisch verknüpft. Die oberste Ebene der Modellhierarchie bildet das `Stocktaking` Modell. In der \abb{fig:stocktaking_klassendiagramm} ist die Hierarchie in Form eines Klassendiagramms abgebildet. Das Klassendiagramm wurde mithilfe der Erweiterungen `django_extensions` und `pygraphviz` erstellt. Folgendes Kommando wurde dafür aufgerufen \cite{django-graph-models}:

```bash
dev_ralph graph_models stocktaking \
-X ItemSplitChangeProposal,MultipleValidationsChangeProposal,
   ValueChangeProposal,TimeStampMixin \
-g -o stocktaking_klassendiagramm.png
```

Um die \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} der Klasse `ChangeProposalBase` auszublenden, wurden diese mit der Option `-X` exkludiert.

\begin{figure}
\centering
\includegraphics{stocktaking_klassendiagramm.png}
\caption{Das automatisch generierte Klassendiagramm der Modelle des
Stocktaking-Moduls.}
\label{fig:stocktaking_klassendiagramm}
\end{figure}


## Das Stocktaking Modell

Eine Instanz des `Stocktaking` Modells repräsentiert eine Inventur. 

Zu den wichtigsten Attributen des `Stocktaking` Modells zählen:

* `name`: Durch dieses Attribut kann eine Inventur benannt werden. Dieser Name erscheint auf der mobilen Applikation oder dem Inventurbericht.
* `user`: Dieses Attribut referenziert einen hauptverantwortlichen Benutzer einer Inventur.
* `date_started` und `time_started`: Diese Attribute sind Zeitstempel und werden automatisch auf den Zeitpunkt der Erstellung einer `Stocktaking` Instanz gesetzt. Eine Inventur beginnt zum Zeitpunkt ihrer Erstellung.
* `date_finished` und `time_finished`: Diese Attribute sind Zeitstempel und werden gesetzt, wenn ein Administrator eine Inventur beenden möchte. Wenn beide Attribute mit einem Wert befüllt sind, werden keine über die mobile Applikation empfangene Validierungen verarbeitet. Der Zeitpunkt, der sich aus den beiden Attributen ergibt, darf nicht vor dem Zeitpunkt aus `date_started` und `time_started` liegen.
* `neverending_stocktaking`: Wenn dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann} Attribut gesetzt ist, hat der Wert der `date_finished` und `time_finished` Attribute keine Bedeutung. Es werden alle Validierungen der mobilen Applikation verarbeitet. Ob in einem Raum bereits einmal validiert wurde oder die Inventur beendet ist, wird nicht mehr überprüft. 

## Das StocktakingUserActions Modell

Das `StocktakingUserActions` Modell ist die Verbindung zwischen einer Inventur und den Benutzern, die zu dieser Inventur beigetragen haben. Das Attribut `stocktaking` verlinkt je eine bestimmte `Stocktaking` Instanz. Das Attribut `user` verlinkt je eine bestimmte `RalphUser` Instanz, die einem angemeldeten Benutzer entspricht. Validierungen, die ein Benutzer während einer Inventur tätigt, sind mit der entsprechenden `StocktakingUserActions` Instanz verknüpft. Pro Inventur kann es nur eine `StocktakingUserActions` Instanz für einen bestimmten Benutzer geben. Diese Bedingung wird durch die `unique_together` Metavariable \cite{django-doku-models-options} festgelegt:

```python
unique_together = [["user", "stocktaking"]]
```

## Das StocktakingRoomValidation Modell

Das `StocktakingRoomValidation` Modell ist die Verbindung zwischen einer `StocktakingUserActions` Instanz und einer bestimmten Raum-Instanz. Die Raum-Instanz kann dank der `GenericForeignKey` Funktionalität \cite{django-doku-contenttypes} von jedem beliebigen Modell stammen. In der Implementierung einer Client-Schnittstelle wird ein konkretes Modell spezifiziert. Zum Zweck der Inventur an der HTL Rennweg ist das `HTLRoom` Modell in der entsprechenden Client-Schnittstelle spezifiziert. Eine Instanz des `StocktakingRoomValidation` Modells repräsentiert das Validieren von Gegenständen in einem bestimmten Raum. 

## Das StocktakingItem Modell

Das `StocktakingItem` Modell repräsentiert die Validierung einer Gegenstands-Instanz während einer Inventur. Die Gegenstands-Instanz kann dank der `GenericForeignKey` Funktionalität \cite{django-doku-contenttypes} von jedem beliebigen Modell stammen. In der Implementierung einer Client-Schnittstelle wird ein konkretes Modell spezifiziert. Zum Zweck der Inventur an der HTL Rennweg ist das `HTLItem` Modell in der entsprechenden Client-Schnittstelle spezifiziert. 
Durch die Beziehungen zu den Modellen `StocktakingRoomValidation` und dadurch zu `StocktakingUserActions` und `Stocktaking` ist erkennbar, in welchem Raum durch welchen Benutzer im Rahmen welcher Inventur ein Gegenstand validiert wurde. Durch das `ChangeProposalBase` Modell sind die Änderungsvorschläge einer Gegenstandsvalidierung verknüpft.

Zu den wichtigsten Attributen des `StocktakingItem` Modells zählen:

* `date_validated` und `time_validated`: Diese Attribute bilden den Zeitstempel der Gegenstandsvalidierung. Durch die Client-Schnittstelle erstellten `StocktakingItem` Instanzen wird der Wert der aktuellen Systemzeit zum Zeitpunkt des Datenempfangs zugewiesen. Wird ein Gegenstand mehrmals validiert[^multiple_validations_timestamp], ist der Zeitstempel jener der jüngsten Validierung.
* `mark_for_later_validation`: Dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann} Attribut dient zur erleichterten Verifikation der Validierungen durch einen Administrator. Ist das Attribut gesetzt, repräsentiert es eine unvertrauenswürdige Gegenstandsvalidierung. Das Attribut wird gesetzt, wenn der Benutzer durch die mobile Applikation angibt, sich bei einer Gegenstandsvalidierung nicht sicher zu sein und daher das "Später entscheiden" Feld setzt.  Innerhalb des Inventur-Berichts werden `StocktakingItem` Instanzen, dessen `mark_for_later_validation` Attribut gesetzt ist, besonders markiert. 

[^multiple_validations_timestamp]: Dieser Fall tritt bei Inventuren auf, dessen `neverending_stocktaking` Attribut gesetzt ist. So kann derselbe Gegenstand Monate nach der ersten Validierung erneut während derselben Inventur validiert werden. 

## Änderungsvorschläge

Änderungsvorschläge beziehen sich auf eine bestimmte `StocktakingItem` Instanz. Ein Änderungsvorschlag ist eine Instanz einer \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ChangeProposalBase`. Ein Änderungsvorschlag besagt, dass der aktuelle Zustand eines Datensatzes in der Datenbank nicht der Realität entspricht. 

Um während einer Inventur nicht sofort jegliche erkannte Änderungen von Gegenstandseigenschaften anwenden zu müssen, werden sie in Änderungsvorschläge ausgelagert. So kann Fehlern, die während einer Inventur entstehen können, vorgebeugt werden. Der Benutzer der mobilen Applikation ist nur damit beauftragt, die aktuellen Inventardaten aus dessen Sicht zu erheben. Es ist die Aufgabe eines Administrators, Änderungsvorschläge als vertrauenswürdig einzustufen und diese tatsächlich anzuwenden.

Als Basis für Änderungsvorschläge dient die Klasse `ChangeProposalBase`. Sie beschreibt nicht, welche Änderung während einer Inventur festgestellt wurde. Sie erbt von der Klasse `Polymorphic`. Die Klasse `Polymorphic` ermöglicht die Vererbung von Modellklassen auf der Datenbankebene. So können alle \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} der Klassen `ChangeProposalBase` einheitlich betrachtet werden. Über die Verbindung von `StocktakingItem` zu `ChangeProposalBase` in \abb{fig:stocktaking_klassendiagramm} sind nicht nur alle verknüpften `ChangeProposalBase` Instanzen, sondern auch alle Instanzen der \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ChangeProposalBase`, verbunden. Eine \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ChangeProposalBase` beschreibt eine Änderung, die von einem Benutzer während einer Inventur festgestellt würde. Es sind 4  \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ChangeProposalBase` und damit 4 Arten von Änderungsvorschlägen implementiert:

1. `ValueChangeProposal`
2. `MultipleValidationsChangeProposal`
3. `ItemSplitChangeProposal`
4. `SAPExportChangeProposal`

Weitere Informationen über die Arten von Änderungsvorschlägen und deren Handhabung sind dem Handbuch zum Server (\siehe{chap:Anhang-1}) zu entnehmen.

## Die Client-Schnittstelle

Jede Art der Inventur benötigt eine eigene Client-Schnittstelle. Eine Art der Inventur unterscheidet sich von anderen durch das Gegenstands- und Raummodell, gegen welches inventarisiert wird. Ein Beispiel ist die Inventur für die Gegenstände der HTL Rennweg, durch die gegen die `HTLItem` und `HTLRoom` Modelle inventarisiert wird. 

Um die Kommunikation zwischen Server und mobiler Client-Applikation für eine Art der Inventur zu ermöglichen, werden 2 `APIView` Klassen erstellt (\siehe{views}):

* Eine Klasse, die von der Klasse `BaseStocktakeItemView` erbt.
* Eine Klasse, die von der Klasse `BaseStocktakeRoomView` erbt.

In den \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} wird \ua{} definiert, um welche Gegenstands- und Raummodelle es sich bei der Art der Inventur handelt. Die beiden vordefinierten Klassen `BaseStocktakeItemView` und `BaseStocktakeRoomView` wurden dazu konzipiert, möglichst wenig Anpassung für die Implementierung einer Art der Inventur zu benötigen. Um das Anpassungspotenzial allerdings nicht einzuschränken, sind die Funktionalitäten der Klassen möglichst modular. Eine Funktionalität wird durch eine Methode implementiert, die von den \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} angepasst werden kann. In dem unten angeführten Beispiel repräsentiert jede Methode eine Anpassung gegenüber dem von `BaseStocktakeItemView` und `BaseStocktakeRoomView` definierten Standardverhalten. 

Zu Demonstrationszwecken wurde eine voll funktionstüchtige Client-Schnittstelle für die Inventur des Gegenstandsmodells `BackOfficeAsset` und Raummodells `Warehouse` in weniger als 30 Code-Zeilen erstellt. 

```python
class BackOfficeAssetStocktakingView(StocktakingGETWithCustomFieldsMixin,
                                     ClientAttachmentsGETMixin,
                                     BaseStocktakeItemView):
    item_model = BackOfficeAsset
    pk_url_kwarg = None
    slug_url_kwarg = "barcode_final"
    slug_field = "barcode_final"

    display_fields = ("hostname", "status", "location", "price", "provider")
    extra_fields = (
        "custom_fields", "office_infrastructure", "depreciation_rate",
        "budget_info", "property_of", "start_usage"
    )
    explicit_readonly_fields = ("hostname", "price")

    def get_queryset(self, request=None, previous_room_validation=None):
        # annotate custom barcode as either "barcode" or "sn"
        return super(BackOfficeAssetStocktakingView, self).get_queryset(
            request=request, previous_room_validation=previous_room_validation
        ).annotate(
            barcode_final=Case(
                When(Q(barcode__isnull=True) | Q(barcode__exact=""),
                     then=(F("sn"))),
                default=F("barcode"), output_field=CharField()))

    def get_room_for_item(self, item_object):
        return str(item_object.warehouse)

    def item_display_name_getter(self, item_object):
        return str(item_object)

    def item_barcode_getter(self, item_object):
        return str(
            item_object.barcode_final
        ) or "" if hasattr(
            item_object, "id"
        ) else ""


class WarehouseStocktakingView(StocktakingPOSTWithCustomFieldsMixin,
                               ClientAttachmentsPOSTMixin,
                               BaseStocktakeRoomView):
    room_model = Warehouse
    item_detail_view = BackOfficeAssetStocktakingView()

    item_room_field_name = "warehouse"
```

Die Modelle `BackOfficeAsset` und `Warehouse` sind in dem unveränderten Ralph-System vorhanden.

Die Klassen `StocktakingGETWithCustomFieldsMixin`,  `StocktakingPOSTWithCustomFieldsMixin`, `ClientAttachmentsGETMixin` und `ClientAttachmentsPOSTMixin` ermöglichen das Miteinbeziehen von \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt unabhängig definierbar sind.} und Anhängen (\siehe{speichern-von-bildern-und-anhuxe4ngen}). Weitere Informationen zu den `Mixin`-Klassen kann der im Source-Code enthaltenen Dokumentation entnommen werden.  

### Kommunikationsformat

Die Kommunikation zwischen Server und mobilem Client erfolgt über HTTP Abfragen. Das Datenformat wurde auf das JSON-Format \cite{json-format-doku} festgelegt. Grund dafür ist die weit verbreitete Unterstützung und relativ geringe Komplexität des Formats. 

### Modell-Voraussetzungen

Um eine Client-Schnittstelle für die Inventarisierung bestimmter Gegenstands- und der dazugehörigen Raummodelle zu implementieren, müssen diese bestimmte Voraussetzungen erfüllen. Die Voraussetzungen werden in diesem Abschnitt beschrieben.

\subsubsection*{Raum-Gegenstand-Verknüpfung}

Das Gegenstandsmodell muss ein Attribut der Klasse `ForeignKey` besitzen, das auf das Raummodell verweist. Der Name dieses Attributs wird in der Klasse, die von `BaseStocktakeRoomView` erbt, in dem Attribut `item_room_field_name` angegeben. Zusätzlich kann in der von `BaseStocktakeItemView` erbenden Klasse durch die `get_room_for_item()` Methode der Raum einer Gegenstandsinstanz als \emph{String} \index{String: Bezeichnung des Datentyps: Zeichenkette} zurückgegeben werden. Ein Implementierungsbeispiel ist in der \oa{}  angegebenen Client-Schnittstelle zu finden. 

\subsubsection*{String-Repräsentation}

Gegenstands- und Rauminstanzen müssen eine vom Menschen lesbare Repräsentation ermöglichen. Standardmäßig wird dazu die `__str__()` Methode der jeweiligen Instanz aufgerufen. Das Verhalten kann durch das Überschreiben der Methoden `item_display_name_getter()` (aus der Klasse `BaseStocktakeItemView`) und `room_display_name_getter()` (aus der Klasse `BaseStocktakeRoomView`) angepasst werden.

\subsubsection*{Barcode eines Gegenstandes}\label{barcode-eines-gegenstandes}

Es muss für eine Instanz des Gegenstandsmodells ein Barcode generiert werden können. Diese Funktion wird in der Methode `item_barcode_getter()` der von `BaseStocktakeItemView` erbenden Klasse definiert. Um über den von der mobilen Applikation gescannten Barcode auf einen Gegenstand schließen zu können, müssen die Attribute `slug_url_kwarg` und `slug_field` auf den Namen eines Attributes gesetzt werden, das den Barcode eines Gegenstandes repräsentiert. Dieses Attribut muss für jeden Datensatz, der durch die `get_queryset()` Methode entsteht, vorhanden sein. In dem in \kap{die-client-schnittstelle} angegebenen Beispiel wird dieses Attribut in der `get_queryset()` Methode durch `annotate()` \cite{django-doku-querysets} hinzugefügt.

### Einbindung und Erreichbarkeit der APIView-Klassen

Um die für eine Art der Inventur implementierten Klassen über das \emph{DRF}\index{DRF: Django REST Framework -- Implementierung einer \emph{REST-API}\index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface -- Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} -- eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} unter Django \cite{django-rest-framework}} ansprechbar zu machen, werden diese in die Variable `urlpatterns` \cite{django-doku-urls} eingebunden. Die Einbindung erfolgt beispielsweise in der Datei `api.py` eines beliebigen Pakets. Bei der Einbindung müssen den jeweiligen Schnittstellen zur späteren Verwendung interne Namen vergeben werden. Folgendes Implementierungsbeispiel bindet die oben definierten Klassen `BackOfficeAssetStocktakingView` und `WarehouseStocktakingView` ein:

```python
urlpatterns = [
    url(
        r"^warehousesforstocktaking/(?:(?P<pk>[\w/]+)/)?$", 
        WarehouseStocktakingView.as_view(),
        name="warehousesforstocktaking"),
    url(
        r"^backofficeassetsforstocktaking/(?:(?P<barcode_final>[\w/]+)/)?$", 
        BackOfficeAssetStocktakingView.as_view(),
        name="backofficeassetsforstocktaking"),
]
```
Die Namen der Schnittstellen werden auf `"warehousesforstocktaking"` und `"backofficeassetsforstocktaking"` gesetzt. In der Einbindung der Klasse `BackOfficeAssetStocktakingView` wird definiert, dass der Name des Barcode-Attributes eines Gegenstandes `barcode_final` lautet. Das `barcode_final` Attribut wird in der Definition von `BackOfficeAssetStocktakingView` hinzugefügt (\sa{} \kap{barcode-eines-gegenstandes}).

Die Klassen `BackOfficeAssetStocktakingView` und `WarehouseStocktakingView` sind dadurch über die \emph{URLs} \index{URL: Addressierungsstandard im Internet} `/api/backofficeassetsforstocktaking/` und `/api/warehousesforstocktaking/` erreichbar. 

Für die Implementierung der Inventur der HTL Rennweg wurden die  \emph{URLs} \index{URL: Addressierungsstandard im Internet} `/api/htlinventoryitems/` und `/api/htlinventoryrooms/` definiert. 

### Statische Ausgangspunkte

Der mobilen Client-Applikation müssen notwendige Informationen der dynamisch erweiterbaren Inventuren und Inventurarten über statisch festgelegte \emph{URLs} \index{URL: Addressierungsstandard im Internet} mitgeteilt werden:

\subsubsection*{Verfügbare Inventuren}

Der mobilen Client-Applikation muss mitgeteilt werden, welche Inventuren zurzeit durchgeführt werden können. Für das Modell `Stocktaking` ist eine \emph{API}\index{API: Application-Programming-Interface -- Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht}-Schnittstelle implementiert (\siehe{api-und-drf}). Über diese Schnittstelle können durch folgende Abfrage-URL mittels eines HTTP GET-Requests \cite{rest-http-methods} alle verfügbaren Inventuren und dessen einzigartige \emph{IDs} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} abgefragt werden:

```bash
/api/stocktaking/?date_finished__isnull=True&time_finish_isnull=True&format=json
```

Durch `&format=json` wird festgelegt, dass der Server eine Antwort im JSON-Format \cite{json-format-doku} liefert. Die Antwort des Servers auf die Abfrage sieht beispielsweise wie folgt aus (Die Daten wurden zwecks Lesbarkeit auf die wesentlichsten Attribute gekürzt.):

```json
[
    {
        "stocktake_id": 1,
        "name": "Inventur 1",
        "comment": "Diese Inventur endet nie!",
        "neverending_stocktaking": true,
    },
    {
        "stocktake_id": 2,
        "name": "Inventur 2",
        "comment": "",
        "neverending_stocktaking": false,
    }
]
```
Laut der Antwort des Servers sind aktuell 2 Inventuren -- "Inventur 1" und "Inventur 2" -verfügbar. 
Die mobile Client-Applikation benötigt den Wert des Attributs `stocktake_id`, um bei späteren Abfragen festzulegen, welche Inventur von einem Benutzer ausgewählt wurde.


\subsubsection*{Verfügbare Arten der Inventur}

Der mobilen Client-Applikation muss mitgeteilt werden, welche Arten der Inventur verfügbar sind und über welche \emph{URLs} \index{URL: Addressierungsstandard im Internet} die jeweiligen `APIView` Klassen ansprechbar sind. Unter der \emph{URL} \index{URL: Addressierungsstandard im Internet} `/api/inventoryserializers/` werden je eine Beschreibung der Inventurart und die benötigten \emph{URLs} \index{URL: Addressierungsstandard im Internet} ausgegeben. Bei korrekter Implementierung der Inventurarten für die Gegenstandsmodelle `HTLItem` und `BackOfficeAsset` liefert der Server folgende Antwort im JSON-Format \cite{json-format-doku}:

```json
{
    "HTL": {
        "roomUrl": "/api/htlinventoryrooms/",
        "itemUrl": "/api/htlinventoryitems/",
        "description": "Inventur der HTL-Items"
    },
    "BackOfficeAsset-Inventur": {
        "roomUrl": "/api/warehousesforstocktaking/",
        "itemUrl": "/api/backofficeassetsforstocktaking/",
        "description": "Demonstrations-Inventur fuer BackOfficeAssets"
    }
}
```

Um eine Art der Inventur in diese Ausgabe miteinzubeziehen, muss in der Datei `api.py` des Stocktaking-Moduls die Variable `STOCKTAKING_SERIALIZER_VIEWS` entsprechend erweitert werden. Dazu wird der Name der Schnittstelle verwendet, wie er in der Variable `urlpatterns` gesetzt wurde. Die zu dem angeführten Beispiel gehörige `STOCKTAKING_SERIALIZER_VIEWS` Variable ist wie folgt definiert:

```python
STOCKTAKING_SERIALIZER_VIEWS = {
    "HTL": (
        "htlinventoryrooms", 
        "htlinventoryitems", 
        "Inventur der HTL-Items"
    ),
    "BackOfficeAsset-Inventur": (
        "warehousesforstocktaking", 
        "backofficeassetsforstocktaking", 
        "Demonstrations-Inventur fuer BackOfficeAssets"
    )
}
```

`"BackOfficeAsset-Inventur"` ist der Name der Inventurart für das Gegenstandsmodell `BackOfficeAsset`. `"warehousesforstocktaking"` ist der Name der Schnittstelle für die Klasse `WarehouseStocktakingView`. `"backofficeassetsforstocktaking"` ist der Name der Schnittstelle für die Klasse `BackOfficeAssetStocktakingView`. `"Demonstrations-Inventur fuer BackOfficeAssets"` ist eine Beschreibung der Inventurart. 

### JSON Schema

Die Daten, die durch die implementierten `APIView` Klassen gesendet oder empfangen werden, müssen einer bestimmten Struktur folgen. In diesem Abschnitt werden die durch die Klassen akzeptierten HTTP-Methoden \cite{rest-http-methods} aufgezählt und deren JSON Schema anhand mehrerer Beispiele dargestellt. Die Beispiele können zwecks Lesbarkeit gekürzt sein. Gekürzte Bereiche werden mit `[...]` markiert.

\subsubsection*{`BaseStocktakeItemView` GET-Methode}

Die Klasse `BaseStocktakeItemView` akzeptiert 2 unterschiedliche Abfragen der GET-Methode. 

Eine Abfrage über die \emph{URL} \index{URL: Addressierungsstandard im Internet} ohne weitere Zusätze liefert eine Liste aller Gegenstände und deren Eigenschaften:

`GET /api/htlinventoryitems/` liefert:
```json
{
    "items": [{
            "itemID": 2,
            "displayName": "Tischlampe",
            "displayDescription": "",
            "barcode": "46010000",
            "room": "111 (Klassenraum)",
            "fields": {
                "anlagenbeschreibung": "Tischlampe",
                [...]
            },
            "attachments": []
        },
		[...]
    ]
}
```

Eine Abfrage über die \emph{URL} \index{URL: Addressierungsstandard im Internet} mit Zusatz des Barcodes eines Gegenstandes liefert eine Liste aller Gegenstände mit dem entsprechenden Barcode und dessen Eigenschaften:

`GET /api/htlinventoryitems/46010000/` liefert:
```json
{
    "items": [{
            "itemID": 2,
            "displayName": "Tischlampe",
            "displayDescription": "",
            "barcode": "46010000",
            "room": "111 (Klassenraum)",
            "fields": {
                "anlagenbeschreibung": "Tischlampe",
                [...]
            },
            "attachments": []
        }
    ]
}
```

\subsubsection*{`BaseStocktakeItemView` OPTIONS-Methode}

Der `OPTIONS` Request wird von der mobilen Client-Applikation benötigt, um ein Formular für Gegenstandsdaten zu erstellen. Dafür werden die Eigenschaften eines Gegenstandes nach Relevanz in `displayFields` (hohe Priorität; wichtige Eigenschaften) und  `extraFields` (niedrige Priorität; unwichtige Eigenschaften) geteilt. Welche Eigenschaften welcher Kategorie zugeordnet werden ist in der Implementierung der `BaseStocktakeItemView` \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} durch die Variablen `display_fields` und `extra_fields` definiert. Jede Eigenschaft wird anhand folgender Kenngrößen beschrieben:

* `verboseFieldName`: Eine vom Menschen lesbare Beschreibung der Eigenschaft.
* `readOnly`: Dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann} Feld ist `true`, wenn die Eigenschaft durch Eintragen in die `explicit_readonly_fields` Variable der `BaseStocktakeItemView` \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} als schreibgeschützt markiert wurde.
* `type`: Dieses Feld gibt die Art der Eigenschaft an. Beispiele sind `"string"`, `"boolean"`, `"integer"`, die jeweils eine Zeichenkette, \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann} oder ganzzahlige Nummer identifizieren. Besondere Eigenschaftsarten sind `"choice"` und`"dict"`.
* `choices`: Dieses Feld ist nur angeführt, wenn das `type` Feld den Wert `"choice"` hat. Es beinhaltet eine Liste aller möglichen Werte \inkl{} vom Menschen lesbare Repräsentation der Werte für die Eigenschaft.
* `fields`: Dieses Feld ist nur angeführt, wenn das `type` Feld den Wert `"dict"` hat. Es beinhaltet eine Kollektion an Eigenschaften, je mit eigenen `verboseFieldName`, `readOnly` und `type` Feldern. Dadurch können Felder rekursiv verschachtelt werden. Ein Beispiel ist unten angeführt.

`OPTIONS /api/htlinventoryitems/` liefert:

```json
{
    "displayFields": {
        "anlagenbeschreibung_prio": {
            "verboseFieldName": "interne Gegenstandsbeschreibung",
            "readOnly": false,
            "type": "string"
        },
        "item_type": {
            "verboseFieldName": "Kategorie",
            "readOnly": false,
            "type": "choice",
            "choices": [
                {
                    "value": null,
                    "displayName": "--- kein Wert ---"
                },
                {
                    "value": 1,
                    "displayName": "IT-Infrastruktur"
                },
                [...]
            ]
        },
		[...]
    },
    "extraFields": {
        "anlagenbeschreibung": {
            "verboseFieldName": "SAP Anlagenbeschreibung",
            "readOnly": true,
            "type": "string"
        },
        "custom_fields": {
            "verboseFieldName": "Custom Fields",
            "readOnly": false,
            "type": "dict",
            "fields": {
                "Beispiel-Custom-Field": {
                    "verboseFieldName": "Beispiel-Custom-Field",
                    "type": "string",
                    "readOnly": false
                }
            }
        },
    }
}
```

\subsubsection*{`BaseStocktakeRoomView` GET-Methode}

Die Klasse `BaseStocktakeRoomView` akzeptiert 2 unterschiedliche Abfragen der GET-Methode.  Beide Abfragen benötigen die \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} der ausgewählten Inventurinstanz als \emph{URL} \index{URL: Addressierungsstandard im Internet}-Parameter `stocktaking_id`. 

Eine Abfrage über die \emph{URL} \index{URL: Addressierungsstandard im Internet} ohne weitere Zusätze liefert eine Liste aller Räume:

`GET /api/htlinventoryrooms/?stocktaking_id=1` liefert:
```json
{
    "rooms": [{
            "roomID": 1,
            "displayName": "111",
            "displayDescription": "Klassenraum",
            "barcode": "11111111",
            "itemID": null
        }, {
            "roomID": 2,
            "displayName": "222",
            "displayDescription": "Labor",
            "barcode": "22222222",
            "itemID": null
        },
    ]
}
```
Eine Abfrage über die \emph{URL} \index{URL: Addressierungsstandard im Internet} mit Zusatz der \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} einer Rauminstanz[^id_detail] liefert die Details der angegebenen Rauminstanz \inkl{} aller Gegenstände, die sich in dem Raum befinden sollten, deren Eigenschaften und allen Subräumen der Rauminstanz:

`GET /api/htlinventoryrooms/1/?stocktaking_id=1` liefert:
```json
{
    "roomID": 1,
    "displayName": "111",
    "displayDescription": "Klassenraum",
    "barcode": "11111111",
    "itemID": null,
    "subrooms": [],
    "items": [{
            "times_found_last": 1,
            "itemID": 2,
            "displayName": "Tischlampe",
            "displayDescription": "",
            "barcode": "46010000",
            "room": "111 (Klassenraum)",
            "fields": {
                "anlagenbeschreibung": "Tischlampe",
                [...]
            },
            "attachments": []
        }
    ]
}

```

[^id_detail]: Diese ID wird etwa der Antwort auf die Abfrage ohne URL-Zusatz entnommen. Die entsprechende Eigenschaft ist `roomID`

\subsubsection*{`BaseStocktakeRoomView` POST-Methode}

Mit dieser Methode sendet die mobile Client-Applikation den aufgezeichneten Ist-Zustand der in einem Raum befindlichen Gegenstände. Eine Abfrage kann nur über die \emph{URL} \index{URL: Addressierungsstandard im Internet} mit Zusatz der \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} einer Rauminstanz[^id_detail] getätigt werden. Die \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} der ausgewählten Inventurinstanz muss als Teil der übermittelten Daten im Feld `stocktaking`  angegeben werden. Unter dem Feld `validations` werden alle validierten Gegenstände mit mindestens deren \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} als Feld `itemID` angegeben. Zusätzlich können alle Gegenstandseigenschaften spezifiziert werden. Die Eigenschaften jedes Gegenstandes werden von der Klasse `BaseStocktakeRoomView` oder der davon erbenden Klasse verarbeitet und mit dem aktuell in der Datenbank eingetragenen Wert verglichen. Bei einer Differenz wird automatisch ein Änderungsvorschlag für diesen Gegenstand erstellt. 

`POST /api/htlinventoryrooms/1/` mit folgenden Daten
```json
{
    "stocktaking": 1,
    "validations": [
    	{
    		"itemID": 1
    	},
    	{
    		"itemID": 2
    	}
    ]
}
```
liefert die Antwort:
```json
{
    "success:": true
}
```

Tritt ein Fehler während der Verarbeitung der Daten auf, wird der Zustand der Datenbank vor dem Empfangen der Daten wiederhergestellt. In diesem Fall teilt der Server dem Client in seiner Antwort mit, welcher Fehler aufgetreten ist.

`POST /api/htlinventoryrooms/1/` mit folgenden Daten
```json
{
    "stocktaking": 100,
    "validations": [
    	{
    		"itemID": 1
    	},
    	{
    		"itemID": 2
    	}
    ]
}
```
liefert die Antwort:
```json
{
    "errors": [
        "No stocktaking with ID 100"
    ]
}
```

Um eine Gegenstandsvalidierung zur erneuten Validierung zu einem späteren Zeitpunkt durch einen Administrator zu markieren, kann das Feld `mark_for_later_validation` auf `true` gesetzt werden:
```json
{
    "stocktaking": 1,
    "validations": [
    	{
    		"itemID": 1,
    		"mark_for_later_validation": true
    	}
    ]
}
```
Dadurch wird die  \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Eigenschaft der erstellten `StocktakingItem` Instanz gesetzt (\siehe{das-stocktakingitem-modell}).

Um einen Gegenstand in einem Subraum (\siehe{subruxe4ume}) zu validieren, kann eine der folgenden 3 Maßnahmen gesetzt werden:

1. Eine separate POST-Abfrage auf die \emph{URL} \index{URL: Addressierungsstandard im Internet} mit Zusatz der \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} des Subraums mit den Daten der entsprechenden Gegenstände. Für einen `HTLRoom`-Subraum mit \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} 100 ist die anzuwendende \emph{URL} \index{URL: Addressierungsstandard im Internet} `/api/htlinventoryrooms/100/`.
2. Das Setzen des `room` Feldes auf die \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} des Subraums innerhalb einer Gegenstandsvalidierung. Ein Beispiel bietet die folgende Abfrage. Die \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} des Subraums ist 100. Die \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} des übergeordneten Raumes ist 1.

`POST /api/htlinventoryrooms/1/` mit folgenden Daten:
```json
{
    "stocktaking": 1,
    "validations": [
    	{
    		"itemID": 1,
    		"room": 100
    	}
    ]
}
```

3. Das Auslagern der Gegenstandsvalidierungen in das Feld `subroomValidations` wie in folgendem Beispiel. Das Ergebnis gleicht dem Beispiel aus Alternative 2:

`POST /api/htlinventoryrooms/1/` mit folgenden Daten:
```json
{
    "stocktaking": 1,
    "validations": [
    ],
    "subroomValidations": [{
            "roomID": 100,
            "validations": [{
                    "itemID": 1
                }
            ],
            "subroomValidations": [
            ]
        }
    ]
}

```

`subroomValidations` können rekursiv definiert werden. In dem Beispiel aus Alternative 3 muss darauf geachtet werden, dass der Raum mit \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} 100 direkter "Subraum" des Raums mit \emph{ID} \index{ID: einzigartige Identifikationsnummer für eine Instanz eines Django-Modells} 1 ist. 

Weitere Details zur Funktionsweise und Anpassung der Client-Schnittstelle ist der im Source-Code enthaltenen Dokumentation zu entnehmen. 

## Pull-Request

Es wurde versucht, das `Stocktaking` Modul durch einen \emph{Pull-Request}\index{Pull-Request: Das Miteinbeziehen von individuell entwickeltem Quellcode in die offizielle Quell-Version der Software} in das offizielle Ralph-System einzubinden. Durch die in \kap{das-stocktakingroomvalidation-modell} und \kap{das-stocktakingitem-modell} behandelte `GenericForeignKey` Funktionalität \cite{django-doku-contenttypes} ist es möglich, das Stocktaking-Modul für jegliche Modellklassen zu verwenden. Es gibt keine Beschränkung auf die durch das Diplomarbeitsteam implementierten Klassen.

Die erstellte Lösung wurde im Entwicklungsforum der Ralph Plattform vorgestellt \cite{ralph-forum-1} \cite{ralph-forum-2}. Die Vorbereitungen für einen Pull-Request wurden getätigt. Das Entwicklerteam der Ralph Plattform hat entschieden, für die erstellte Lösung ein eigenes Git-Repository anzulegen. Das Repository ist dazu vorgesehen, optionale Module zu beherbergen. Ein Pull-Request erfolgt, sobald das Repository von den Entwicklern erstellt wurde.