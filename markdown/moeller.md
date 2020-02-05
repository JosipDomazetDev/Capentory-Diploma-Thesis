\chapter{Einführung in die Server-Architektur}

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

Alle genannten Alternativen sind \emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} der Programmiersprache Python. Gewählt wurde "Django" aufgrund einer bestehenden und frei verfügbaren Inventarverwaltungsplattform "Ralph" \cite{ralph}, die auf Django aufbaut und durch die vorliegende Diplomarbeit hinreichend erweitert wird. 


# Django und Ralph

Django ist ein in der Programmiersprache Python geschriebenes Webserver-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}. Ralph ist eine auf dem Django-\emph{Framework}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt} basierende \emph{DCIM}\index{DCIM: Data Center Infrastructure Management - Software, die zur Verwaltung von Rechenzentren entwickelt wird } und \emph{CMDB}\index{CMDB: Configuration Management Database - Eine Datenbank, die für die Konfiguration von IT-Geräten entwickelt ist \cite{cmdb}} Softwarelösung. Haupteinsatzgebiet dieser Software sind vor allem Rechenzentren mit hoher Komplexität, die externe Verwaltungsplattformen benötigen. Zusätzlich können aber auch herkömmliche Inventardaten von IT-spezifischen Gegenständen in die Ralph-Plattform aufgenommen werden. 

Ralph wurde von der polnischen Softwarefirma "Allegro" entwickelt und ist unter der Apache 2.0 Lizenz öffentlich verfügbar. Dies ermöglicht auch Veränderungen und Erweiterungen. Zu Demonstrationszwecken bietet Allegro eine öffentlich nutzbare Demo-Version \cite{ralph-demo} von Ralph an.

Die vorliegende Diplomarbeit bietet eine Erweiterung des Ralph-Systems.

## Begründung der Wahl von Django und Ralph
Django bietet eine weit verbreitete Open-Source Lösung für die Entwicklung von Web-Diensten. Bekannte Webseiten, die auf Django basieren sind u.a. Instagram, Mozilla, Pinterest und Open Stack. \cite{django-overview}
Django zeichnet sich besonders durch die sog. \emph{"Batteries included"}\index{Batteries included: Das standardmäßige Vorhandensein von erwünschten bzw. gängigen \emph{Features}\index{Feature: Eigenschaft bzw. Funktion eines Systems}, zu Deutsch: Batterien einbezogen} Mentalität aus. Das heißt, dass Django bereits die gängigsten \emph{Features}\index{Feature: Eigenschaft bzw. Funktion eines Systems} eines Webserver-Backends standardmäßig innehat. Diese sind (im Vergleich zu Alternativen wie etwa "Flask") u.a.:

* Authentifikation und Autorisierung, sowie eine damit verbundene Benutzerverwaltung
* Schutz vor gängigen Attacken (wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF}\index{CSRF: Cross-Site-Request-Forgery - eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken \cite{csrf}}\cite{csrf}), siehe [Views][views]

Zusätzlich bietet Ralph bereits einige \emph{Features}\index{Feature: Eigenschaft bzw. Funktion eines Systems}, die die grundlegende Führung und Verwaltung eines herkömmlichen Inventars unterstützen (beispielsweise eine Suchfunktion mit automatischer Textvervollständigung). 

# Kurzfassung der Funktionsweise von Django und Rlaph
Im folgenden Kapitel wird die Funktionsweise des Django-\emph{Frameworks}\index{Framework: Eine softwaretechnische Architektur, die bestimmte Funktionen und Klassen zur Verfügung stellt}, sowie Ralph beschrieben. Ziel dieses Kapitels ist es, eine Wissensbasis für die darauffolgenden Kapitel zu schaffen.

## Datenbank-Verbindung, Pakete und Tabellen-Definition
Folgende Datenbank-Typen werden von Django unterstützt:

* PostgreSQL
* MariaDB
* MySQL
* Oracle
* SQLite

Die Konfiguration der Datenbank-Verbindung geschieht unter Standard-Django in der Datei `settings.py`
, unter Ralph in der jeweiligen Datei im Verzeichnis `settings`.
Eine detaillierte Anleitung zur Verbindung mit einer Datenbank ist in der offiziellen Django-Dokumentation \cite{django-doku-db} zu finden.


Die verschiedenen Funktionsbereiche des Servers sind in Pakete bzw. Module gegliedert. Jedes Paket ist ein Ordner, der verschiedene Dateien und Unterordner beinhalten kann. Die Dateinamen-Nomenklatur eines Packets ist normiert.\cite{django-file-nomenklatur} Der Name eines Pakets wird fortan "App-Label" genannt. Standardmäßig ist dieser Name erster Bestandteil einer URL zu einer beliebigen graphischen Administrationsoberfläche des Pakets.
Pakete werden durch einen Eintrag in die Variable `INSTALLED_APPS`
innerhalb der o.a. Einstellungsdatei registriert. Beispiele sind die beiden durch die vorliegende Diplomarbeit registrierten Pakete `"ralph.capentory"` und  `"ralph.stocktaking"`


Ist ein Python-Paket erfolgreich registriert, können in der Datei `models.py`
Datenbank-Tabellen als python Klassen[^model-class-inheritance] definiert werden. Diese Klassen werden fortan als "Modell" bezeichnet. 
Tabellenattribute werden als Attribute dieser Klassen definiert und sind jeweils Instanzen der Klasse `Field`\cite{django-doku-models}[^field-class-inheritance]. Datenbankeinträge können demnach als Instanzen der Modellklassen betrachtet und behandelt werden. Standardmäßig besitzt jedes Modell ein Attribut `id`, welches als \emph{primärer Schlüssel}\index{primärer Schlüssel: engl. "primary key", abgek. "pk" - ein Attribut, das einen Datensatz eindeutig identifiziert} dient. Der Wert des `id` Attributs ist unter allen Instanzen eines Modells einzigartig. Die Anpassung dieses Attributs wird in der offiziellen Django-Dokumentation genauer behandelt. \cite{django-doku-models}

Jedes Modell benötigt eine innere Klasse `Meta`. Sie beschreibt  die \emph{Metadaten}\index{Metadaten: Daten, die einen gegebenen Datensatz beschreiben, beispielsweise der Autor eines Buches} der Modellklasse. Dazu gehört vor allem der von Benutzern lesbare Name des Modells `verbose_name`. \cite{django-doku-models-options}

[^model-class-inheritance]: erbend von der Superklasse `Model`\cite{django-doku-models}
[^field-class-inheritance]: oder davon erbende Klassen

## Administration über das Webinterface

Um die Administration von Modelldaten über das Webinterface des Servers zu ermöglichen, werden grundsätzlich zwei Ansichten der Daten benötigt: Eine Listenansicht aller Datensätze und eine Detailansicht einzelner Datensätze. 

Die Listenansicht aller Datensätze eines Modells wird in der Datei `admin.py` als \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ModelAdmin`[^ralph-admin] definiert. Attribute dieser Klasse beeinflussen das Aussehen und die Funktionsweise der Weboberfläche. Durch  das Setzen von `list_display` werden beispielsweise die in der Liste anzuzeigenden Attribute definiert. 

Die Detailansicht einzelner Datensätze wird grundsätzlich durch die `ModelAdmin` Klasse automatisch generiert, kann aber durch Setzen dessen `form` Attributs auf eine eigens definierte \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} von `ModelForm`[^ralph-adminform]  angepasst werden. Diese Klassen werden in der Datei `forms.py` definiert und besitzen, ähnlich der `Model` Klasse, auch eine innere Klasse  `Meta`.

Um die `ModelAdmin`  \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} über eine URL erreichbar zu machen, müssen diese registriert werden. Dies geschieht durch den `register` \emph{Dekorator}\index{Dekorator: Fügt unter Python einer Klasse oder Methode eine bestimmte Funktionsweise hinzu \cite{python-decorators}}. Dieser Dekorator akzeptiert die zu registrierende Modellklasse, die zu dem `ModelAdmin` gehört, als Parameter.
Die Listenansicht einer registrierten `ModelAdmin` Subklasse ist standardmäßig unter der URL 
```
/<App-Label>/<Modell-Name>/
```
erreichbar, die Detailansicht einer Modellinstanz unter der URL 
```
/<App-Label>/<Modell-Name>/<Modellinstanz-ID>/
```
. Somit repräsentiert die URL der Listenansicht gleichzeitig den Pfad, unter der sie definiert wurde. 

Die Dokumentation der Administrationsfeatures von Django ist auf der offiziellen Dokumentationswebseite von Django  \cite{django-doku-admin} zu finden.

[^ralph-admin]: Unter Ralph steht hierfür die Klasse `RalphAdmin` zur Verfügung.\cite{ralph-admin-doku}
[^ralph-adminform]: Unter Ralph steht hierfür die Klasse `RalphAdminForm` zur Verfügung.

## API und DRF

Um Daten außerhalb der graphischen Administrationsoberfläche zu bearbeiten, wird eine \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} benötigt. Eine besondere und weit verbreitete Form einer API ist eine \emph{REST-API} \index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} - eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} \cite{rest-api}, die unter Django durch das integrierte \emph{DRF}\index{DRF: Django REST Framework - Implementierung einer \emph{REST-API}\index{REST-API: Representational State Transfer \emph{API}\index{API: Application-Programming-Interface - Eine Schnittstelle, die die programmiertechnische Erstellung, Bearbeitung und Einholung  von Daten auf einem System ermöglicht} - eine zustandslose Schnittstelle für den Datenaustausch zwischen Clients und Servern \cite{rest-api}} unter Django \cite{django-rest-framework}} implementiert wird.\cite{django-rest-framework}
API Definitionen werden unter Django in einem Paket in der Datei `api.py` getätigt. 

Um den API-Zugriff auf ein Modell zu ermöglichen werden üblicherweise eine `APIView`[^ralph-viewset] \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} und eine `Serializer`[^ralph-serializer] \emph{Subklasse}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} definiert. `APIView` Klassen sind zuständig für das Abarbeiten von Anfragen mithilfe einer `Serializer` Klasse, die die Daten aus der Datenbank repräsentiert und in das gewünschte Format konvertiert. Durch `APIView` Klassen werden Berechtigungen und sonstige Attribute definiert, die sich auf das wahrgenommene Erscheinungsbild des Servers auf einen Client auswirken. Beispiel dafür ist die Art der \emph{Paginierung} \cite{django-rest-framework}\index{Paginierung: engl. pagination - Die Aufteilung von Datensätzen in diskrete Seiten \cite{django-rest-framework-pagination}}.  Die erstellten `APIView` Klassen können dann mithilfe einer `Router`[^ralph-router] Instanz registriert werden. Anleitungen zur Erstellung dieser API-Klassen sind auf der offiziellen Webseite des DRF \cite{django-rest-framework} und der offiziellen Ralph-Dokumentationsseite \cite{ralph-api-doku} zu finden.

[^ralph-serializer]: Unter Ralph steht hierfür die Klasse `RalphAPIViewSet` zur Verfügung. \cite{ralph-api-doku}
[^ralph-viewset]: Unter Ralph steht hierfür die Klasse `RalphAPISerializer` zur Verfügung. \cite{ralph-api-doku}
[^ralph-router]: Unter Ralph steht hierfür die globale `RalphRouter` Instanz `router` zur Verfügung. \cite{ralph-api-doku}

## Views

Schnittstellen, die keiner der beiden o.a. Kategorien zugeordnet werden können, werden in der Datei `views.py` definiert. Bei diesen \emph{generischen}\index{generisch: in einem allgemeingültigen Sinn} Schnittstellen handelt es sich entweder um \emph{Subklassen}\index{Subklasse: Eine programmiertechnische Klasse, die eine übergeordnete Klasse, auch "Superklasse", erweitert oder verändert, indem sie alle Attribute und Methoden der Superklasse erbt} der Klasse `View`[^view-apiview-inheritance] \cite{django-doku-class-based-views} oder vereinzelte Methoden mit einem `request`[^request-german] Parameter. \cite{django-doku-views}
Diese Schnittstellen werden fortan Views genannt.

Soll ein View als Antwort auf eine Anfrage HTML-Daten liefern, so sollte dazu ein \emph{Template}\index{Template: zu Deutch: Vorlage, Schablone} verwendet werden. Mithilfe von Templates können Daten, die etwa durch Datenbankabfrage entstehen, zu einer HTML Antwort aufbereitet werden. Besonders ist hierbei die zusätzlich zu HTML verfügbare Django-Template-\emph{Syntax}\index{Syntax: Regelwerk, sprachliche Einheiten miteinander zu verknüpfen \cite{syntax}} \cite{django-doku-template}. Damit können HTML Elemente auf den Input-Daten basierend dynamisch generiert werden. So stehen beispielsweise `if` Statements direkt in der Definition des Templates zur Verfügung.
Die Benutzung von Templates schützt standardmäßig gegen gängige Attacken, wie \emph{SQL-Injections}\index{SQL-Injections: klassischer Angriff auf ein Datenbanksystem} oder \emph{CSRF} \index{CSRF: Cross-Site-Request-Forgery - eine Angriffsart, bei dem ein Opfer dazu gebracht wird, eine von einem Angreifer gefälschte Anfrage an einen Server zu schicken \cite{csrf}}\cite{csrf} und gilt daher als besonders sicher.

Da reguläre Views nicht automatisch registriert werden, müssen sie manuell bekanntgegeben werden. Dies geschieht durch einen Eintrag in die Variable `urlpatterns` in der Datei `urls.py`.  \cite{django-doku-urls}

[^view-apiview-inheritance]: die ebenfalls Superklasse der Klasse `ApiView` ist
[^request-german]: zu Deutch: Anfrage; entspricht dem empfangenen Packet, meist als HTML. 

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
Weitere Beispiele und Methoden sind der offiziellen Django-Dokumentation zu entnehmen. \cite{django-doku-queries}

[^django-query-example]: entnommen aus der offiziellen Django Dokumentation \cite{django-doku-queries}

# Designgrundlagen

Designgrundlagen für Django-Entwickler sind auf der offiziellen Dokumentationsseite von Django abrufbar. \cite{django-doku-coding-style}
Die Erweiterung von Django durch die vorliegende Diplomarbeit wurde anhand dieser Grundlagen entwickelt. 

Das Konzept des `Mixin`s wird von der Ralph-Plattform besonders häufig genutzt. `Mixin`s sind Klassen, die anderen von ihnen erbende Klassen, bestimmte Attribute und Methoden hinzufügen. Manche `Mixin`s setzen implizit voraus, dass die davon erbenden Klassen ebenfalls von bestimmten anderen Klassen erben. Beispiel ist die Klasse `AdminAbsoluteUrlMixin`, die eine Methode `get_absolute_url` zur Verfügung stellt. Diese Methode liefert die URL, die zu der Detailansicht der Modellinstanz führt, die die Methode aufruft. Voraussetzung für das Erben einer Klasse von `AdminAbsoluteUrlMixin` ist daher, dass sie ebenfalls von der Klasse `Model` erbt.

\chapter{Die 2 Erweiterungsmodule}

Die vorliegende Diplomarbeit erweitert das ["Ralph"](#django-und-ralph) System um 2 Module. Dabei handelt es sich um die beiden Pakete "Capentory" und "Stocktaking". Das Paket "Capentory" behandelt die Führung der Inventardaten und wurde speziell an die Inventardaten der HTL Rennweg angepasst.. Das Paket "Stocktaking" ermöglicht die Verwaltung der durch die mobile Applikation durchgeführten Inventuren. Dazu zählen Aufgaben wie das Erstellen der Inventuren, das Einsehen von Inventurberichten oder das Anwenden der aufgetretenen Änderungen.

Dieses Kapitel beschreibt die Funktionsweise der beiden Module. Die Bedienung der \emph{Weboberfläche} \index{Weboberfläche: graphische Oberfläche für administrative Tätigkeiten, die über einen Webbrowser erreichbar ist} ist dem \todo{Add reference} Handbuch zum Server zu entnehmen.

# Das "Capentory" Modul

Das "Capentory" Modul beherbergt 3 wichtige Modelle: 

1. HTLItem
2. HTLRoom
3. HTLItemType

Die wichtigsten Eigenschaften der Modelle und damit verbundenen Funktionsweisen werden in diesem Unterkapitel beschrieben.

## Das HTLItem Modell

Das `HTLItem`-Modell repräsentiert die Gegenstandsdaten des Inventars der HTL Rennweg. Es sind typische Merkmale aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System vertreten. Die Attribute `anlage`, `asset_subnumber` und `company_code` werden direkt aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System übernommen. 

### Die wichtigsten Attribute

Zu den wichtigsten Attributen des `HTLItem` Modells zählen u.a.:

* `anlage` und `asset_subnummer`: Diese Attribute bilden den Barcode eines Gegenstandes.
* `barcode_prio`: Wenn dieses Attribut gesetzt ist, überschreibt es den durch die Attribute `anlage` und `asset_subnummer` entstandenen Barcode.
* `anlagenbeschreibung`: Dieses Attribut repräsentiert die aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System entnommene Gegenstandsbeschreibung und kann nur durch den Import von Daten direkt aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System geändert werden.
* `anlagenbeschreibung_prio`: Dieses Attribut dient als interne Gegenstandsbeschreibung, die auch ohne einen Import aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System geändert werden kann.
* `room`: Dieses Attribut referenziert auf ein `HTLRoom` Objekt, in dem sich ein `HTLItem` Objekt befindet.
* `is_in_sap`: Der Wert dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Attributs ist `Wahr`, wenn der `HTLItem`-Datensatz aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System importiert wurde. Umgekehrt ist der Wert dieses Attributes `Falsch`, wenn der `HTLItem`-Datensatz aus einer anderen Quelle entstanden ist. Ein manuell hinzugefügter `HTLItem`-Datensatz hat für dieses Attribut den Wert `Falsch`.
* `item_type`: Dieses Attribut referenziert auf das `HTLItemType` Objekt, das einem `HTLItem` Objekts zugeordnet ist. Es repräsentiert die Gegenstandskategorie eines `HTLItem` Objekts. 


### Einzigartigkeit von HTLItem Objekten

Bezüglich der Einzigartigkeit von `HTLItem` Objekten gelten einige Bestimmungen. 

Sind die Attribute `anlage`,  `asset_subnummer` und `company_code` je mit einem nichtleeren Wert befüllt, so repräsentieren sie ein `HTLItem` Objekt eindeutig. Es dürfen keine 2 `HTLItem` Objekte denselben Wert dieser Attribute haben. Um diese Bedingung erfüllen zu können muss eine eigens angepasste Validierungslogik implementiert werden. Standardverfahren wäre in diesem Anwendungsfall, die Metavariable `unique_together` \cite{django-doku-models-options} anzupassen:

```python
unique_together = [["anlage",  "asset_subnummer", "company_code"]]
```

Dieses Verfahren erfüllt nicht die geforderte Bedingung nur in der Theorie. Praktisch werden leere Werte von Attributen dieser Art nicht als `None` (Python) bzw. `null` (MySQL), sondern als Leerstrings `""` gespeichert. Um diese Werte ebenfalls von der Regel auszuschließen, muss die `validate_unique()`[^validate-unique] Methode \cite{django-doku-models-instances} überschrieben werden. 

[^validate-unique]: Eine Methode einer Modell-Klasse, die unter Normalzuständen immer vor dem Speichern eines Objekts des Modells aufgerufen wird. Wirft sie einen Fehler auf, kann das Objekt nicht gespeichert werden. 

Ist das `barcode_prio` Attribut eines `HTLItem` Objekts gesetzt, darf dessen Wert nicht mit jenem eines anderen `HTLItem` Objekts übereinstimmen. Standardverfahren wäre in diesem Anwendungsfall das Setzen des `unique` Parameters des Attributes auf den Wert `True`. Da dieses Verfahren ebenfalls das o.a. Problem aufwirft, muss die Logik stattdessen in die `validate_unique()` Methode aufgenommen werden. Zusätzlich darf der Wert des `barcode_prio` Attributs nicht mit dem aus den beiden Attributen `anlage` und `asset_subnummer` generierten Barcode übereinstimmen. Um diese Bedingung zu erfüllen kann nur die `validate_unique()` Methode herbeigezogen werden. 

## Das HTLRoom Modell

Das `HTLRoom`-Modell repräsentiert die Raumdaten der HTL Rennweg. Es sind typische Merkmale aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System vertreten. Die Attribute `room_number`, `main_inv` und `location` werden direkt aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System übernommen. 

### Die wichtigsten Attribute

Zu den wichtigsten Attributen des `HTLRoom` Modells zählen u.a.:

* `room_number`: Dieses Attribut bildet den Barcode eines Raumes.
* `barcode_override`: Wenn dieses Attribut gesetzt ist, überschreibt es den durch das Attribut `room_number` gebildeten Barcode.
* `internal_room_number`: Dieses Attribut repräsentiert die schulinterne Raumnummer und ist somit von den Daten aus dem \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System unabhängig.
* `is_in_sap`: Der Wert dieses \emph{Boolean}\index{Boolean: Ein Wert, der nur "Wahr" oder "Falsch" sein kann}-Attributs ist `Wahr`, wenn der `HTLRoom`-Datensatz einen aus dem  \emph{SAP ERP} \index{SAP ERP: Enterprise-Resource-Planning Software der Firma SAP. Damit können Unternehmen mehrere Bereiche wie beispielsweise Inventardaten oder Kundenbeziehungen zentral verwalten} System vorhandenen Raum repräsentiert.
* `children`: Mit dieser Beziehung können einem übergeordneten `HTLRoom` Objekt mehrere `HTLRoom` Objekte untergeordnet werden. Anwendungsfall für dieses Attribut ist die Definition von Schränken oder Serverracks, die je einem übergeordneten Raum zugeteilt sind, selbst aber eigenständige Räume repräsentieren.
* `type`: Dieses Attribut spezifiziert die Art eines `HTLRoom` Objekts. So kann ein `HTLRoom` Objekt einen ganzen Raum oder auch nur einen Kasten in einem übergeordneten Raum repräsentieren.
* `item`: Dieses Attribut kann gesetzt werden, um ein `HTLRoom` Objekt durch ein `HTLItem` Objekt zu repräsentieren. Anwendungsbeisipel ist ein Schrank, der sowohl als `HTLRoom` Objekt als auch als `HTLItem` Objekt definiert ist. Sind die beiden Objekte durch das `item` Attribut verbunden, ist der Barcode des `HTLRoom` Objekts automatisch jener des `HTLItem` Objekts.

### Einzigartigkeit von HTLRoom Objekten

Bezüglich der Einzigertigkeit von `HTLRoom` Objekten gelten ähnliche Bestimmungen wie zu jener von `HTLItem` Objekten.

Die Attribute `room_number`, `main_inv` und `location` sind gemeinsam einzigartig. Leere Werte sind von dieser Regel ausgeschlossen. Gleichzeitig darf der Wert des `barcode_override` Attribut nicht mit dem Wert des `room_number` Attributes eines anderen `HTLRoom` Objekts übereinstimmen und vice versa.
Beide Bedingungen müssen wie im Falle des `HTLItem` Modells durch Überschreiben der `validate_unique()`[^validate-unique] Methode \cite{django-doku-models-instances} implementiert werden.

### Subräume

Wie im Abschnitt ["Die wichtigsten Attribute"](#die-wichtigsten-attribute-1) festgehalten, können einem `HTLRoom` Objekt mehrere `HTLRoom` Objekte untergeordnet werden. Diese untergeordneten Räume werden "Subräume" genannt. Bei "Subräumen" handelt es sich beispielsweise um einen Kasten, der als eigenständiger Raum in einem ihm übergeordneten Raum steht. Logisch betrachtet ist der Kasten auch nur ein Raum, in dem sich Gegenstände befinden. Ob der Raum ein Klassenraum oder ein Kasten in einem Klassenraum ist, hat keine logischen Auswirkungen auf seine Eigenschaften als "Standort von Gegenständen".

Um eine valide Hierarchie beizubehalten, muss diese Beziehung bei jedem Speicherprozess eines `HTLRoom` Objekts überprüft werden. Das geschieht durch die Methode `clean_children()`, die beim Speichern durch die graphische Administrationsoberfläche automatisch aufgerufen wird. Bei Speicherforgängen, die nicht direkt durch die Administrationsoberfläche initiiert werden [^clean_children_manually], muss `clean_children()` manuell aufgerufen werden.

[^clean_children_manually]: etwa das automatisierte Speichern beim Datenimport

## Das HTLItemType Modell

Das `HTLItemType` Modell repräsentiert Kategorien von Gegenständen. Das Modell besteht aus 2 Eigenschaften. Die Eigenschaft `item_type` beschreibt ein `HTLItemType` kurz, die Eigenschaft `description` bietet Platz für Kommentare. 

Durch das Setzen eines `HTLItemType` Objekts für ein `HTLItem` Objekt durch seine Eigenschaft `item_type` werden dem `HTLItem` Objekt alle \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt ünabhängig definierbar sind.} des `HTLItemType` Objekts zugewiesen. Anwendungsbeispiel ist das Setzen eines \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt ünabhängig definierbar sind.} namens "Anzahl Ports" für den `HTLItemType` namens "Switch". Jedes `HTLItem` Objekt mit dem `item_type` "Switch" hat nun ein \emph{Custom-Field} namens "Anzahl Ports".

Das für \emph{Custom-Fields}\index{Custom-Fields: Benutzerdefinierte Eigenschaften eines Objektes in der Datenbank, die für jedes Objekt ünabhängig definierbar sind.} erforderliche Mixin (siehe Abschnitt ["Designgrundlagen"](#designgrundlagen)) `WithCustomFieldsMixin` bietet die Funktion der `custom_fields_inheritance`. Die Funktion ermöglicht das Erben von allen \emph{Custom-Field} Werten eines bestimmten Objekts an ein anderes. Dieser Funktion macht sich das `HTLItem` Modell zunutze. Um beim Speichern automatisch vom `HTLItemType` Objekt unabhängige \emph{Custom-Field} Werte zu erstellen, die sofort vom Benutzer bearbeitet werden können, muss der Speicherlogik eine Funktion hinzugefügt werden. Dazu wird eine `@receiver` Methode genutzt, die automatisch bei jedem Speichervorgang eines `HTLItemType` oder `HTLItem` Objekts aufgerufen wird:

```python
@receiver(post_save, sender=HTLItemType)
def populate_htlitem_custom_field_values(sender, instance, **kwargs):
    populate_inheritants_custom_field_values(instance)
    
@receiver(post_save, sender=HTLItem)
def populate_htlitemtype_custom_field_values(sender, instance, **kwargs):
    populate_with_parents_custom_field_values(instance)
```

Die beiden angeführten Methoden werden je beim Aufkommen eines `post_save` Signals \cite{django-doku-signals} ausgeführt, dessen `sender` ein `HTLItemType` oder `HTLItem` ist. Die Methoden rufen jeweils eine weitere Methode auf, welche die \emph{Custom-Fields} entsprechent aggregiert. 