\chapter{Einführung in die App-Architektur}

Wie im vorherigen Kapitel angesprochen, ist das Ziel der Diplomarbeit, eine App zu entwickeln, mit der man in der Lage ist, eine Inventur durchzuführen. Doch wieso eine App und wieso überhaupt Android? Um diese Frage zu klären, muss man zwischen zwei Begriffen unterscheiden \cite{native-vs-web}:

* Native App
* Web-App

Nativ vs. Web
=============================

Unter einer nativen App versteht man eine App, die für ein bestimmtes Betriebssystem geschrieben wurde \cite{native-definition}. 
Die Definition ist allerdings nicht ganz eindeutig, da Frameworks wie Flutter und Xamarin nativer Funktionalität sehr nahekommen, obwohl sie mehrere verschiedene Betriebssysteme unterstützten. 
Eine Web-App hingegen basiert auf HTML und wird per Browser aufgerufen. Sie stellt nichts anderes als eine für mobile Geräte optimierte Website dar. 

Am Markt zeichnet sich in letzter Zeit ein klarer Trend ab – native Apps sterben allmählich aus \cite{native-trend} und werden durch mobile Webseiten ersetzt. Das ist dadurch erklärbar, dass mobile Webseiten immer funktionsreicher werden. Mittlerweile haben Web-Apps nur noch geringfügig weniger Möglichkeiten als native Apps. Aus wirtschaftlicher Betrachtungsweise amortisieren sich Web-Apps de facto um einiges schneller und sind auch dementsprechend lukrativer. 


# Begründung: Native App

Das Projektteam hat sich dennoch für eine native App entschieden. Um diese Entscheidung nachvollziehen zu können, ist ein tieferer Einblick in den gegebenen Use-Case erforderlich.  

Das Ziel ist es nicht, möglichst viele Downloads im Play Store zu erzielen oder etwaige Marketingmaßnahmen zu setzen. 
Es soll stattdessen mit den gegebenen Ressourcen eine Inventurlösung entwickelt werden, die die bestmögliche Lösung für unsere Schule darstellt. 
Eine native App wird eine Web-App immer hinsichtlich Qualität und User Experience klar übertreffen. 
Im vorliegenden Fall wäre es sicherlich möglich, eine Inventur mittels Web-App durchzuführen, allerdings würde diese vor allem in den Bereichen Performanz und Verlässlichkeit Mängel aufweisen.
Diese zwei Bereiche stellen genau die zwei Problembereiche dar, die es mit der vorliegenden Gesamtlösung bestmöglich zu optimieren gilt. 
Des Weiteren bieten sich native Apps ebenfalls für komplexe Projekte an, da Web-Apps aktuell noch nicht in der Lage sind, komplexe Aufgabenstellungen mit vergleichbar geringem Aufwand zu inkorporieren \cite{complex-article}. 
Die vorliegende Arbeit ist dabei bereits allein aufgrund der in Betracht zu ziehenden Sonderfälle als komplexe Aufgabenstellung einzustufen.


Unter Berücksichtigung dieser Gesichtspunkte wurde also der Entschluss gefasst, eine native Applikation zu entwickeln, da diese ein insgesamt besseres Produkt darstellen wird.
Es sei gesagt, dass es auch Hybride Apps gibt. Diese sind jedoch einer nativen App in denselben Aspekten wie eine Web-App klar unterlegen.

## Auswahl der nativen Technologie

Folgende nativen Alternativen waren zu vergleichen:


* Flutter \cite{flutter}
* Xamarin \cite{xamarin}
* Native IOS
* Native Android (Java/Kotlin)


Flutter ist ein von Google entwickeltes Framework, dass eine gemeinsame Codebasis für Android und IOS anbietet.
Eine gemeinsame Codebasis wird oftmals unter dem Begriff `cross-platform` zusammengefasst und bedeutet, dass man eine mit Flutter entwickelte native App sowohl mit Android-Geräten als auch mit IOS-Geräten verwenden kann. 
Flutter ist eine relativ neue Plattform – das erste stabile Release wurde erst im Dezember 2018 veröffentlicht \cite{flutter-stable}. Außerdem verwendet Flutter die Programmiersprache `Dart`, die Java ähnelt. 
Diese Umstände sind ein Segen und Fluch zugleich. Flutter wird in Zukunft sicherlich weiterhin an Popularität zulegen, allerdings ist die Anzahl an verfügbarer Dokumentation für das junge Flutter im Vergleich zu den anderen Optionen immer noch weitaus geringer. 

Xamarin ist ebenfalls ein cross-platform Framework, das jedoch in C# geschrieben wird und älter (und damit bewährter) als Flutter ist. Weiters macht Xamarin von der proprietären .NET-Platform Gebrauch. Infolgedessen haben alle Xamarin-Apps Zugriff auf ein umfassendes Repertoire von .NET-Libraries \cite{xamarin-details}. Da Xamarin und .NET beide zu Microsoft gehören, ist eine leichtere Azure-Integration oftmals ein Argument, das von offzielen Quellen verwendet wird. Xamarin wird - anders als die restlichen Optionen - bevorzugterweise in Visual Studio entwickelt \cite{xamarin-vs}.

Native IOS wird nur der Vollständigkeit halber aufgelistet, stellte allerdings zu keinem Zeitpunkt eine wirkliche Alternative dar, weil IOS-Geräte einige Eigenschaften besitzen, die für eine Inventur nicht optimal sind (z.B. die Akkukapazität). Außerdem haben in etwa nur 20% aller Geräte \cite{ios-market-share} IOS als Betriebssystem und die Entwicklung einer IOS-App wird durch strenge Voraussetzungen äußerst unattraktiv gemacht. So kann man beispielsweise nur auf einem Apple-Gerät IOS-Apps entwickeln.

### Begründung: Natives Android (Java)

Die Entscheidung ist schlussendlich auf natives Android (Java) gefallen. Es mag zwar vielleicht nicht die innovativste Entscheidung sein, stellt aber aus folgenden Gründen die bewährteste und risikoloseste Option dar:

* Natives Android ist eine allbekannte und weit etablierte Lösung. Die Wahrscheinlichkeit, dass die Unterstützung durch Google eingestellt wird, ist also äußerst gering.
* Die App wird in den nächsten Jahren immer noch am Stand der Technik sein.
* Natives Android hat mit großem Abstand die umfassendste Dokumentation.
* An der Schule wird Java unterrichtet. Das macht somit eventuelle Modifikationen nach Projektabschluss durch andere Schüler viel einfacher möglich.
* Dadurch, dass Kotlin erst seit 2019 \cite{kotlin-preference} offiziell die von Google bevorzugte Sprache ist, sind die meisten Tutorials immer noch in Java.
* Sehr viele Unternehmen haben viele aktive Java-Entwickler. Dadurch wird die App attraktiver, da die Unternehmensmitarbeiter (von z.B. allegro) keine neue Sprache lernen müssen, um Anpassungen durchzuführen.
* Das Projektteam hat im Rahmen eines Praktikums bereits Erfahrungen mit nativem Java gesammelt.

Aus den Projektzielen hat sich in Absprache mit den Betreuern ergeben, dass die App nicht auf jedem "Steinzeitgerät" zu funktionieren hat.
Das minimale API-Level der App ist daher 21 - auch bekannt als Android 5.0 'Lollipop'. 
Android 4.0 hat sehr viele nützliche Libraries hervorgebracht. So zum Beispiel die `Mobile Vision API` von Google, dank derer man in der Lage ist, Barcodes in akzeptabler Zeit mit der Kamera des Geräts zu scannen. Die Wahl ist auf 5.0 gefallen, da somit ein Puffer zur Verfügung steht und in etwa 90% aller Android-Geräte ohnehin auf 5.0 oder einer neueren Version laufen \cite{android-versions-market-share}. 



# Einführung zu nativem Java

Um eine Basis für die folgenden Kapitel zu schaffen, werden hier die Basics der Android-Entwicklung mit nativem Java näher beschrieben. Das Layout einer App wird in XML Files gespeichert, während das wirkliche Programmieren mit Java erfolgt.


## Single-Activity-App

Als Einstiegspunkt in eine App dient eine sogenannte `Activity`. Eine Activity ist eine normale Java-Klasse, der durch Vererbung UI-Funktionen verleiht werden. 

Bis vor kurzem war es üblich, dass eine App mehrere Activities hat. Das wird bei den Benutzern dadurch bemerkbar, dass die App z.B. bei einem Tastendruck ein weiteres Fenster öffnet, das das bisherige überdeckt. Das neue Fenster ist eine eigene Activity. Google hat sich nun offiziell für sogenannte Single-Activities ausgesprochen \cite{single-activity}. Das heißt, dass es nur eine Activity und mehrere `Fragments` gibt. Ein Fragment ist eine Teilmenge des UIs bzw. einer Activity. Anstatt jetzt beim Tastendruck eine neue Activity zu starten, wird einfach das aktuelle Fragment ausgetauscht. Dadurch, dass keine neuen Fenster geöffnet werden, ist die User Expierence (UX) um ein Vielfaches besser – die Performanz leidet nur minimal darunter. Die vorliegende App ist aus diesen Gründen ebenfalls eine Single-Activity-App.

## Separation of Concerns

In Android ist es eine äußerst schlechte Idee, sämtliche Logik in einer Activity oder einem Fragment zu implementieren.
Das softwaretechnische Prinzip `Separation of Concerns (SoC)` hat unter Android einen besonderen Stellenwert. Dieses Prinzip beschreibt im Wesentlichen, dass eine Klasse nur einer Aufgabe dienen sollte. Falls eine Klasse mehrere Aufgaben erfüllt, so muss diese auf mehrere logische Komponenten aufgeteilt werden.
Beispiel: Eine Activity hat immer die Verantwortung, die Kommunikation zwischen UI und Benutzer abzuwickeln. Bad Practice wäre es, wenn jene Activity ebenfalls dafür verantwortlich ist, Daten von einem Server abzurufen.
Das Prinzip verfolgt das Ziel, die `God Activity Architecture (GAA)` möglichst zu vermeiden \cite{god-activities}. 
Eine God-Activity ist unter Android eine Activity, die die komplette Business-Logic beinhaltet und SoP in jeglicher Hinsicht widerspricht.
God-Activities gilt es dringlichst zu vermeiden, da sie folgende Nachteile mit sich bringen:

 * Refactoring wird kompliziert
 * Wartung und Dokumentation werden äußerst schwierig
 * Automatisiertes Testing (z.B. Unit-Testing) wird nahezu unmöglich gemacht
 * Größere Bug-Anfälligkeit
 * Im Bezug auf Android gibt es oftmals massive Probleme mit dem `Lifecycle` einer Activity - da eine Activity und ihre Daten schnell vernichtet werden können (z.B. wenn der Benutzer sein Gerät rotiert und das Gerät den Bildschirmmodus wechselt)
 
God-Activities sind ein typisches Beispiel für Spaghetticode. Es bedarf also einer wohlüberlegten und strukturierten Architektur, um diese Probleme zu unterbinden. 
Im nächsten Kapitel wird dementsprechend die Architektur der App im Detail erklärt.
 
\chapter{MVVM - Die App-Architektur}

Als Reaktion auf eine Vielzahl von Apps, die Probleme mit God-Activites aufwiesen, hat Google Libraries veröffentlicht, die klar auf eine MVVM-Architektur abzielen \cite{mvvm}. Daher fiel die Wahl der App-Architektur auf MVVM.

# Designgrundlagen von MVVM

MVVM steht für Model–view–viewmodel \cite{mvvm-wiki}. Wie man am Namen bereits erkennt, gilt es zwischen drei Komponenten/Ebenen zu unterscheiden \cite{mvvm-article}. 

#### Model 
Model beschreibt die Ebene der Daten und wird daher oftmals auch als Datenzugriffsschicht bezeichnet. Diese Ebene beinhaltet so viel Anwendungslogik wie möglich.

#### View
View beschreibt die graphische Ebene und umfasst daher das GUI. Diese Ebene soll so wenig Logik wie möglich beinhalten. 

#### ViewModel
Das ViewModel dient als Bindeglied zwischen dem Model und der View. Die Logik der View wird in diese Ebene hinaufverschoben.  

# MVVM in Android

Mit der Einführung der Architecture Components hat Google Android-Entwicklern eine Vielzahl an Libraries zur Verfügung gestellt, um MVVM leichter in Android implementieren zu können \cite{mvvm-architecture-components}. Die konkrete Implementierung in Android ist in der Abbildung ersichtlich.

![MVVM in Android nach Google \cite{mvvm}](josip-pics\mvvm.png)

In dem vorliegenden Fall ist unser `Fragment` die `View`, das `Repository` das `Model` und das `ViewModel` ist in Android namensgleich.

## Das Repository im Detail

Wie in der Grafik veranschaulicht, ist das Repository dafür zuständig, Daten vom Server anzufordern. Im vorliegenden Fall besteht kein Grund, eine Datenbank am Client zu führen. Damit fällt dieser Aspekt der Grafik - der in violetter Farbe gehalten ist - für die vorliegende Diplomarbeit weg. Die Aufgabe des Repositories ist es also immer Daten vom Server anzufordern. 

### JsonRequest

Die Kommunikation zwischen dem Client und der App findet ausschließlich im `JSON`-Format statt. JSON ist ein text-basiertes und kompaktes Datenaustauschformat. Android bietet Entwicklern eine Out-of-the-box Netzwerklibrary namens `Volley` an, mithilfe derer man unter anderem JSON-Anfragen verarbeiten kann \cite{volley}. Da diese für die vorliegenden Zwecke nicht komplett geeignet war, hat das Diplomarbeitsteam die gegebene Library durch den Einsatz von Vererbung und einer Wrapper-Klasse modifiziert. Die Library wurde in folgenden Punkten angepasst:

 * Im Falle eines Fehlers wird die Anfrage wiederholt (Ausnahme: Zeitüberschreitungsfehler). Die maximale Anzahl an Wiederholungen ist limitiert. 
 * Die maximale Timeout-Dauer wurde erhöht.
 * Leere Antworten werden von der App als valide Antwort behandelt und können ohne Fehler verarbeitet werden.
 * Im Header der Anfrage wird der Content-Type der Anfrage auf JSON festgelegt.
 * Im Header wird das zur Authentikation notwendige API-Token mitgeschickt. Die Authentifizierung ist über einen Parameter deaktivierbar. 
 * Im Header wird die Systemsprache des Clients als standardisierter ISO-639-Code mitgesendet. Der Server passt seine Antwort auf die verwendete Sprache an \cite{ISO-639}. Die Bezeichnung der Felder, die dem Benutzer auf Gegenstandsbasis angezeigt werden, ist beispielsweise abhängig von der Systemsprache.
* Zum Zeitpunkt der Anfrage ist nicht bekannt, ob die Antwort als JSONArray oder als JSONObject erfolgen wird. Da das Backend abhängig von der Anfrage sowohl mit einem JSONArray als auch einem JSONObject antworten kann, ist der Rückgabewert am Client immer ein String. Die Umwandlung erfolgt erst im Repository. 

Diese Unterscheidung zwischen JSONArray und JSONObject ist notwendig, da Android zwei Möglichkeiten anbietet, JSON als Objekt zu speichern:

Direkt als JSONArray:
```java
JSONArray jsonArray = new JSONArray(payload);
```
Direkt als JSONObject:
```java
JSONObject jsonObject = new JSONObject(payload);
```
Beide Optionen haben einen Konstruktor, der einen String akzeptiert (im obigen Beispiel ist dies der String `payload`). Das Backend könnte - natürlich in Abhängigkeit von der ursprünglichen Anfrage - folgende (vereinfachte) Antworten senden:


Ein JSONArray (wenn die Anfrage beispielsweise eine Gegenstandsliste verlangt):
```json
[
  {
    "id": 1,
    "item": "PC"
  },
  {
    "id": 2,
    "item": "Schrank"
  }
]
```


Ein JSONObject (wenn die Anfrage beispielsweise einen spezifischen Gegenstand verlangt):
```json
{
  "id": 1,
  "item": "PC"
}
```

Wenn das Backend ein JSONArray sendet und man versucht, jenes als JSONObject zu speichern, kommt es zu einem Fehler:
```java
// Dieser Code entspricht keiner korrekten Java-Syntax! 
// Antwort vom Backend
String payload =   "[
                        {
                            "id": 1,
                            "item": "PC"
                        },
                        {
                            "id": 2,
                            "item": "Schrank"
                        }
                    ]";

// Die Umwandlung eines JSONArray zu einem JSONObjects 
// wirft eine Exception.
JSONObject jsonObject = new JSONObject(payload);
```

Aus diesem Grund kann die Umwandlung der Backend-Antwort erst im Repository erfolgen. Dies führt zu keinen Perfomance-Problemen, da die vorgefertigte Android Library den String zwar zu einem früheren Zeitpunkt aber auf dieselbe Weise umwandeln würde. Die Umwandlung eines Strings
zu einem JSONArray oder einem JSONObject entspricht nur dem Bruchteil der Zeitdauer, die die Backend-Antwort benötigt, um am Client anzukommen. Selbst bei größeren Strings bemerkt der Benutzer daher keinerlei Unterschied. 

### JsonRequest - Beispiel

Eine Anfrage wird nie direkt, sondern immer über einen Wrapper ausgeführt. Der Konstruktor ist wie folgt aufgebaut:



* `Context context`: Ist eine Schnittstelle, die globale Information über die App-Umgebung zur Verfügung stellt \cite{context} und von Android zur Verfügung gestellt wird. Jede UI-Komponente (z.B. Textfelder, Buttons, Fragments, etc.) verfügt über einen Context. Ein besonderer Context ist der globale Application-Context. Dieser ist einzigartig und ist ein `Singleton`.  Ein Singleton bedeutet, dass von einer Klasse nur ein (globales) Objekt besteht \cite{singleton}. 
* `int method`: Ist die HTTP-Methode. Die App verwendet `GET`, `OPTIONS` und `POST`.
* `String url`: Ist die Seite, die eine JSON-Antwort liefern soll.
* `@Nullable String requestBody`: Eventuelle Parameter, die an den Server gesendet werden sollen. Dieser Paramter ist für einen `POST`-Request wichtig.
* `NetworkSuccessHandler successHandler`: Funktionales Interface 
* `NetworkErrorHandler errorHandler`: Funktionales Interface 

Ein Request kann wie folgt ausschauen:

```java
RobustJsonRequestExecutioner robustJsonRequestExecutioner =
 new RobustJsonRequestExecutioner(context, Request.Method.GET,
  "https://www.beispiel.org/", null, 
  payload -> {
         // TODO: Antwort verarbeiten
     },
  error -> {
         // TODO: Fehler verarbeiten
     });
     
 robustJsonRequestExecutioner.launchRequest();
```

Wie man sehen kann, sind die letzten beiden Parameter funktionale Interfaces, die dazu dienen, Methoden als Parameter übergeben zu können.
Ein Interface mit einer einzigen abstrakten Methode ist als funktionales Interface zu bezeichnen \cite{lambda}. Da Android Studio Java 8 Language Features unterstützt, verwendet die App mehrheitlich Lambda-Ausdrücke \cite{java8}. Lambda-Ausdrücke sind im Wesentlichen dazu da, funktionale Interfaces in vereinfachter Schreibweise verwenden zu können. Da es nur eine einzige abstrakte Methode gibt, kann die Schreibweise simplifiziert werden, weil klar ist von welcher Methode die Rede ist. Damit fallen - wie im obigen Beispiel zu sehen - redundante Informationen wie Rückgabetyp und Methodenkörper vollständig weg. Das obige Beispiel würde ohne Lambda-Ausdrücke wie folgt ausschauen:

```java
RobustJsonRequestExecutioner robustJsonRequestExecutioner = 
 new RobustJsonRequestExecutioner(context, Request.Method.GET, 
 "https://www.beispiel.org/", null,
     new NetworkSuccessHandler() {
         @Override
         public void handleSuccess(String payload) {
             // TODO: Antwort verarbeiten
         }
     },
     new NetworkErrorHandler() {
         @Override
         public void handleError(Exception error) {
             // TODO: Fehler verarbeiten
         }
     });

robustJsonRequestExecutioner.launchRequest();
```
Lambdas sind eine Option, um `Callbacks` in Java zu implementieren. Ein Callback ("Rückruffunktion") ist eine Methode, die einer anderen Methode als Parameter übergeben werden kann. Die soeben erwähnten funktionalen Interfaces sind typische Callbacks \cite{Callbacks}.
Es gibt zwei Arten von Callbacks:

* Synchrone Callbacks: Die Ausführung der übergebenen Methode erfolgt sofort
* Asynchrone Callbacks: Die Ausführung der übergebenen Methode erfolgt später

In diesem Fall wird die Methode `handleSuccess` aufgerufen, sobald der Client die Antwort erhalten hat. Damit handelt es sich um ein asynchrones Callback.

### Retrofit 

`Retrofit` ist eine weitere Netzwerk-Library (bzw. Libraries), die das Projektteam eingesetzt hat. Retrofit wurde nur zum Senden von Dateien eingesetzt, weil dies mit Volley nur erschwert möglich ist. Das Projektteam hat die von Retrofit zur Verfügung gestellten Libraries nicht wesentlich modifiziert.
Da die Anhangfunktion das einzige Einsatzgebiet von Retrofit darstellt, nimmt Volley in der vorliegenden Diplomarbeit die weitaus wichtigere Rolle ein. Volley kann grundsätzlich alles was Retrofit kann und vice versa. Aufgrund dessen, dass Volley jedoch mehr Möglichkeiten zur Zuschneidung auf einen spezifischen Use-Case hat, hat sich das Projektteam für Volley entschieden. Ein Vorteil der jedoch damit verloren geht, ist das automatische Parsing von JSON-Antworten, das zwar in Retrofit implementiert ist, jedoch nicht in Volley \cite{retrofit-vs-volley}.


Das Projektteam hat bei dem einzigen Einsatz von Retrofit auf das automatische Parsing verzichtet und stattdessen die bereits bekannte Callback-Logik verwendet. 
```java
Call<String> call = prepareCall(args);
call.enqueue(new Callback<String>() {
    @Override
    public void onResponse(Call<String> call, Response<String> response) {
        // TODO: Antwort verarbeiten
    }
    @Override
    public void onFailure(Call<String> call, Throwable t) {
        // TODO: Fehler verarbeiten
    }
});
```

In Retrofit werden API-Endpunkte über Interfaces definiert:

```java
public interface AttachmentAPI {
    @Multipart
    @POST(SerializerEntry.attachmentUrl)
    Call<String> addFile(@Header("authorization") String auth,
                         @Part MultipartBody.Part file,
                         @Part("description") String description);
}
```
Dies führt zu einem besseren Überblick als bei Volley, da man pro API-Endpunkt des Backends ein Interface hat. Damit ist sofort ersichtlich, mit welchen Backend-Endpunkten ein Repository kommuniziert. 



## Das ViewModel und das Fragment im Detail

Das ViewModel ist eine Klasse, die dafür ausgelegt ist, UI-bezogene Daten lifecycle-aware zu speichern. Ein Fragment durchlebt im Laufe seines Daseins eine Vielzahl an Zuständen/Phasen - man spricht von einem `Lifecycle`. Wenn der Benutzer zum Beispiel sein Gerät rotiert, führt dies dazu, dass das Fragment \emph{zerstört} wird und das Fragment durch erneutes Durchleben alter Zustände wiederaufgebaut wird - dies führt zu einer Zerstörung des aktuellen UIs des Fragments sowie sämtlicher Referenzen, die das Fragment besitzt. Eine Gerätrotierung gehört zur Kategorie der `Configuration Changes` \cite{viewmodel}. Der Grund hierfür liegt darin, dass Android das aktuelle Layout ändert, da beispielsweise andere Layouts (XML-Files) für den Landscape-Modus zur Verfügung stehen \cite{configuration-changes}. In der Literatur wird der Begriff \emph{zerstören} verwendet, da dabei das Callback `onDestroy` in einer Activity aufgerufen wird.  

Folgende Probleme können dadurch auftreten:

* Die App stürzt App. Wenn eine Methode ausgeführt wird, die eine Referenz auf ein zerstörtes Objekt hat, kann dies zum Absturz der gesamten App führen.
* Memory Leaks entstehen, da Referenzen auf zerstörte Objekte vom Gargabe Collector nicht freigegeben werden können. In Android wird die Minimierung des Speicherbedarfs der App einzig und allein vom Garbage Collector übernommen. Falls dieser Objekte nicht freigeben kann, führt dies dazu, dass die App immer mehr und mehr Arbeitsspeicher benötigt. Je nach Größe des Memory Leaks kann dies zu kleineren Lags bis zu einem Absturz der App führen.
* Nach einem Configuration Change gehen die aktuellen Daten verloren und der Benutzer muss das Problem selbst lösen.
* Wenn die aktuellen Daten verloren gehen, verhält sich eine App oftmals unvorhersehbar. 

### ViewModel als Lösung

![Zustände einer Activity im Vergleich zu den Zuständen eines ViewModels, Fragments haben einen ähnlichen Lifecycle \cite{fragment-lifecycle} \cite{viewmodel}](josip-pics\viewmodel-lifecycle.png)

Bei genauerer Betrachtung der Grafik wird ersichtlich, welche Phasen eine Activity bei einer Gerätrotierung durchlebt:

* Activity wird zerstört:
    * `onPause`
    * `onStop`
    * `onDestroy`
* Activity wird wieder aufgebaut:
    * `onCreate`
    * `onStart`
    * `onResume`


Wie in der Abbildung zu sehen ist, stellt ein ViewModel eine Lösung für diese Probleme dar. Ein ViewModel ist von einem Configuration Changes nicht betroffen und kann dem UI damit stets die aktuellen Daten zur Verfügung stellen. Der gegebene Sachverhalt trifft genauso auf Fragments zu. Diese haben einen leicht veränderten Lifecycle, sind allerdings genauso von Configuration Changes betroffen wie Activities.

Anmerkung: Man kann das Zerstören & Wiederaufbauen von Activities/Fragments manuell blockieren. Dies ist jedoch kein Ersatz für eine wohlüberlegte App-Architektur und führt in den meisten Fällen zu unerwünschten Nebenwirkungen, da man sich nun auch manuell um das Wechseln der Konfiguration (Layouts etc.) kümmern muss und dies weitaus komplizierter ist, als auf ViewModels zu setzen \cite{lifecycle-blocking}.


Folgende Aspekte hat man bei der Verwendung eines ViewModels zu beachten \cite{viewmodel-antipatterns}:

* ViewModel sollte bei einem Configuration Changes keinen neuen Request starten, da es bereits über die Daten verfügt. Dies lässt sich mit einer If-Anweisung beheben.
* Referenzen zu Objekten, die an einen Lifecycle gebunden sind, sind ein absolutes NO-GO. Objekte mit Lifecycle haben ein klares Schicksal - wenn ihr Host vernichtet wird, müssen sie ebenfalls vernichtet werden. Folgendes Szenario: Ein ViewModel hat eine `TextView`-Variable. Dreht der Benutzer sein Gerät wird das aktuelle Fragment inklusive TextView vernichtet. Das ViewModel überlebt den Configuration Change und hat nun eine Referenz auf eine invalide TextView. Dies ist ein Memory Leak.
* ViewModel überleben ein Beenden des Prozesses nicht. Wenn das BS aktuell wenig Ressourcen zur Verfügung hat, kann es sein, dass Prozesse beendet werden. Falls man diesen Sonderfall behandeln will, ist dies mit Extra-Aufwand verbunden \cite{viewmodel-process-death}.
* ViewModels sollen nicht zu "God-ViewModels" werden. Das SoC-Prinzip ist anzuwenden.


Wie gelangen die Daten ins UI, wenn das ViewModel keine Referenzen darauf haben darf? Die Antwort lautet `LiveData`. 


### LiveData

LiveData ist eine observierbare Container-Klasse. Observierbar heißt, dass bei Änderungen des enkapsulierten Objektes ein Callback aufgerufen wird. LiveData ist ebenfalls lifecycle-aware. Daher wird LiveData immer nur aktive Komponenten mit Daten versorgen. Eine TextView, die bereits zerstört wurde, erhält dementsprechend auch keine Updates mehr. 

Dieses (angepasste) offizielle Beispiel veranschaulicht die Funktionsweise sehr gut \cite{livedata}. Im Beispiel soll ein Benutzername angezeigt werden:

```java
public class NameViewModel extends ViewModel {

    // LiveData-Objekt, das einen String beinhaltet
    private MutableLiveData<String> currentName;

    public LiveData<String> getCurrentName() {
        if (currentName == null) {
            currentName = new MutableLiveData<>();
        }
        // Benutzernamen bekannt geben
        currentName.postValue("Max Mustermann");

        return currentName;
    }

    // Rest des ViewModels...
}
```
Der Unterschied zwischen `MutableLiveData` und `LiveData` besteht darin, dass letzteres nicht veränderbar ist. Mit der `postValue`-Methode kann einer MutableLiveData-Instanz, die ja als Container-Objekt dient, ein neuer Wert zugewiesen werden. Dadurch werden etwaige Callbacks aufgerufen (siehe nächster Code-Ausschnitt). Man sollte bei öffentlichen Methoden immer nur LiveData als Rückgabewert verwenden, damit auf der Ebene der View keine Modifikationen der Daten des ViewModels vorgenommen werden können. Im Realfall stammt LiveData ursprünglich aus dem Repository. 

```java
public class NameFragment extends Fragment {
 @Override
    public void onViewCreated(@NonNull View view, 
    @Nullable Bundle savedInstanceState) {
        ...

        // Mit dieser Anweisung wird ein ViewModel erstellt
        model = new ViewModelProvider(this).get(NameViewModel.class);

        model.getCurrentName().observe(getViewLifecycleOwner(),
         currentName -> {
             // Diese Methode wird bei Änderungen aufgerufen.
             // CurrentName ist ein String.
             // nameTextView ist ein TextFeld, 
             // das den aktuellen Benutzernamen anzeigt.
             // Mit .setText(String) kann der angezeigte Text 
             // geändert werden.
             nameTextView.setText(currentName);
         });
}
```
Hier kommt wieder die vorher angesprochene Lambda-Syntax zum Einsatz. Die Methode wird einmal beim erstmaligen Registrieren aufgerufen und wird danach bei jeder weiteren Änderung aufgerufen. Im Callback arbeitet man direkt mit dem eigentlichem Datentypen - in diesem Fall mit einem String -, da LiveData nur ein Container-Objekt ist. Im Fragment befindet sich damit relativ wenig Logik. Das Fragment hört nur auf eventuelle Änderungen und aktualisiert das UI in Abhängigkeit von den Änderungen. Das LiveData-Objekt ist an `getViewLifeycleOwner()` gebunden. Wenn der LifecycleOwner inaktiv wird, werden keine Änderungen mehr entsandt. Man könnte auch `this` als Argument übergeben (`this` wäre in diesem Fall das Fragment, das ebenfalls über einen Lifecycle verfügt). `getViewLifeycleOwner()` hat jedoch den Vorteil, dass der Observer automatisch entfernt wird, sobald der LifecycleOwner zerstört wird. 


### Angepasste LiveData-Klasse

Für den vorliegenden Usecase reichen jedoch die Nutzdaten allein nicht. Es sind weitere Informationen über den Status der Nutzdaten erforderlich. 
Beispiel: Wenn eine Anfrage zehn Sekunden benötigt, um am Client anzukommen, muss dem Benutzer signalisiert werden, dass er auf das Backend zu warten hat. Das wird mit einer `ProgressBar` realisiert. Man könnte jetzt im ViewModel `LiveData<Boolean> isFetching` verwenden und im Fragment dieses LiveData-Objekt observieren. Falls der aktuelle Wert `true` ist, wird die ProgressBar angezeigt. Falls der Wert auf `false` geändert wird, werden stattdessen die nun zur Verfügung stehenden Nutzdaten angezeigt. Bei mehreren Netzwerkanfragen wird dies bald unübersichtlich, da mehrere LiveData-Objekte vonnöten sind, die aus logischer Perspektive zu einem bereits bestehenden LiveData-Objekt gehören - den Nutzdaten. Daher hat das Diplomarbeitsteam - wie von Google \cite{google-wrapper} und von einem StackOverflow-Thread \cite{so-wrapper} empfohlen - die Nutzdaten in einer Wrapper-Klasse enkapsuliert.


```java
public class StatusAwareData<T> {

    // Status der Nutzdaten
    @NonNull
    private State status;

    // Nutzdaten
    @Nullable
    private T data;

    // Eventueller Fehler
    @Nullable
    private Throwable error;

    ...

    @NonNull
    public State getStatus() {
        return status;
    }

    @Nullable
    public T getData() {
        return data;
    }

    @Nullable
    public Throwable getError() {
        return error;
    }

    // Enum, das die legalen Status definiert
    public enum State {
        INITIALIZED,
        SUCCESS,
        ERROR,
        FETCHING
    }
}
```

Diese Wrapper-Klasse speichert Nutzdaten eines generischen Typen. Der Status wird durch eine Finite-state machine in Form eines `Enums` implementiert.  In Android sollte man Enums vermeiden, da diese um ein Vielfaches mehr Arbeitsspeicher und persistenten Speicher benötigen als ihre Alternativen. Als Alternative kann man auf Konstanten zurückgreifen. Dieser Code-Ausschnitt stellt das einzige Enum des gesamten Projektes dar. \cite{avoid-enums}

* `INITIALIZED`: Dieser Status bedeutet, dass das Objekt soeben erstellt wurde. Wird nur bei der erstmaligen Instanziierung verwendet.
* `SUCCESS`: Dieser Status bedeutet, dass die Nutzdaten `data` bereit sind.
* `ERROR`: Dieser Status bedeutet, dass eine Netzwerkanfrage fehlgeschlagen ist (Ob am Client oder am Server spielt keine Rolle). In diesem Fall ist die `error`-Variable gesetzt.
* `FETCHING`: Dieser Status bedeutet, dass auf eine Netzwerkanfrage gewartet wird.

Um diese Wrapper-Klasse elegant benutzen zu können, hat das Projektteam `MutableLiveData` durch Vererbung angepasst:

```java
public class StatusAwareLiveData<T> 
    extends MutableLiveData<StatusAwareData<T>> {

    public void postFetching() {
        // Instanziiert StatusAwareData-Objekt mit FECHTING als 
        // aktuellen Status.
        // Setzt das StatusAwareData-Objekt anschließend 
        // als Wert der LiveData-Instanz.
        postValue(new StatusAwareData<T>().fetching());
    }

    public void postError(Exception exception) {
        // Instanziiert StatusAwareData-Objekt mit ERROR als
        // aktuellen Status.
        // Mit der übergebenenen Exception wird die error-Variable
        // initialisiert.
        // Setzt das StatusAwareData-Objekt anschließend 
        // als Wert der LiveData-Instanz.
        postValue(new StatusAwareData<T>().error(exception));
    }

    public void postSuccess(T data) {
        // Instanziiert StatusAwareData-Objekt mit SUCCESS als
        // aktuellen Status.
        // Mit dem übergebenenen Objekt wird die data-Variable
        // initialisiert.
        // Setzt das StatusAwareData-Objekt anschließend 
        // als Wert der LiveData-Instanz.
        postValue(new StatusAwareData<T>().success(data));
    }

}
```

 Damit entfällt der Bedarf selbst neue StatusAwareData-Objekte zu instanziieren, da diese bereits über die `postFetching`-, `postError`- und `postSuccess`-Methoden - mit korrektem Status - instanziiert werden. Infolgedessen ist StatusAwareData abstrahiert und im ViewModel genügt es, mit den modifizierten LiveData-Instanzen zu arbeiten. Damit ändert sich das vorherige ["Beispiel"](livedata) wie folgt:


```java

// Im ViewModel:

// StatusAwareLiveData-Objekt, das einen String mit Status beinhaltet
private StatusAwareLiveData<String> currentName;

public LiveData<StatusAwareData<String>> getCurrentName() {
    if (currentName == null) {
        currentName = new StatusAwareLiveData<>();
    }
    // Benutzernamen bekannt geben, hier wird eine
    // erfolgreiche Netzwerkanfrage simuliert.
    currentName.postSuccess("Max Mustermann");
       
    return currentName;
}


// Im Fragment:

model.getCurrentName().observe(getViewLifecycleOwner(), 
 statusAwareData -> {
      switch (statusAwareData.getStatus()) {
          case SUCCESS:
              // TODO: Nutzdaten anzeigen
              nameTextView.setText(statusAwareData.getData());
              break;
          case ERROR:
              // TODO: Fehlermeldung anzeigen
              ...
              break;
          case FETCHING:
              // TODO: Ladebalken anzeigen
              ...
              break;
      }
   });
```


## Konkrete MVVM-Implementierung

Im vorliegenden Anwendungsfall hat ein Fragment immmer mindestens einen Datensatz, der für das Fragment namensgebend ist. 
Der Room-Screen (also die Anzeige mit einer DropDown zur Raumauswahl) setzt sich beispielsweise aus folgenden Komponenten zusammen: 

* Der `RoomsFragment`-Klasse 
* Der `fragment_rooms.xml`-Datei, die das UI-Layout definiert
* Der `RoomsViewModel`-Klasse
* Der `RoomsRepository`-Klasse

Das `RoomsRepository` ist dafür verantwortlich, die Raumliste vom Backend anzufordern und in Java-Objekte umzuwandeln.
Das `RoomsViewModel` ist dafür verantwortlich, dem Fragment LiveData-Objekte zur Verfügung zu stellen. 
Das `RoomsFragment` ist dafür verantwortlich, dem Benutzer die Räume anzuzeigen, indem es LiveData observiert. Alternativ zeigt es Fehlermeldungen bzw. einen Ladebalken an.

Bei genauerer Betrachtung wird klar, dass fast jeder Screen dieselbe Aufgabe hat:

* Das Repository fordert Daten vom Backend an und wandelt die JSON-Antwort um.
* Das ViewModel abstrahiert Logik und stellt der View LiveData zur Verfügung. 
* Das Fragment zeigt entweder Nutzdaten, eine Fehlermeldung oder einen Ladebalken an.

Hier greift das softwaretechnische Prinzip `Do not repeat yourself (DRY)` \cite{dry}. Anstatt `Boilerplate-Code` für jeden einzelnen Screen kopieren zu müssen, hat das Projektteam diese sich wiederholende Logik abstrahiert. Boilerplate-Code sind Code-Abschnitte, die sich immer wieder wiederholen \cite{boiler}. Wiederholende Logik sollte immer in eine Superklasse abstrahiert werden. Das Projektteam hat demnach drei abstrakte Klassen definiert, die die Menge an Boilerplate-Code signifikant reduzieren:

* `NetworkRepository`
* `NetworkViewModel`
* `NetworkFragment`

Alle Komponenten von Screens, die Netzwerkanforderungen durchführen, erben von diesen drei Klassen. Damit hat das Projektteam folgende Vorteile aggregiert: 

* Abstrahierte Fehlerbehandlung
* Abstrahiertes Refreshverhalten
* Abstrahierte Ladeanzeige (durch eine `ProgressBar`)
* Abstrahierter Netzwerkzugriff

Der Refactor, der dies realisierte, war zwar zeitintensiv, hat sich jedoch mittlerweile mehr als rentiert. Das Hinzufügen von neuen Screens benötigt nur mehr einen Bruchteil des ursprünglichen Codes. Infolgedessen wird das Erweitern der App um neue Features enorm erleichtert. 


\chapter{Das Scannen}

Das Scannen ist ein vitaler Aspekt dieser Diplomarbeit. Gegenstände an unserer Organisation sind mit Barcodes ausgestattet. Um die Produktivität maximal zu steigern, muss die App in der Lage sein, diese Barcodes zu erfassen. Wir bieten folgende Varianten zum Scannen an:

* Zebra-Scan
* Kamerascan
* Manuelle Eingabe (für den Fall, dass ein Scan fehlschlägt)

# Der Zebra-Scan

Der Sponsor dieser Diplomarbeit - Zebra - hat dem Diplomarbeitsteam einen TC56 ("Touch Computer") zur Verfügung gestellt. Dieser verfügt über einige Eigenschaften, die für eine Inventur vom Vorteil sind \cite{zebra-tc56}:

* Ein Akku mit über 4000 mAh ermöglicht mehrstündige Inventuren.
* Viel Arbeitsspeicher und ein leistungsstarker Prozessor ermöglichen ein flüssiges App-Verhalten.
* Dank robuster Bauart hält das Gerät auch physisch anspruchsvollere Phasen einer Inventur aus.
* Ein in das Smartphone integrierter Barcodescanner reduziert die Scanzeiten drastisch.

# Zebra-Scan: Funktionsweise

Die App kommuniziert nicht direkt mit dem Scanner. Stattdessen wickelt das Zebra-Gerät den Scan ab und sendet das Resultat als `Broadcast` aus \cite{broadcast}. Auf dem Zebra-Gerät läuft im Hintergrund immer die DataWege-Applikation. Dies ist eine App, die die Behandlung des tatsächlichen Scans abwickelt und das Ergebnis auf mehrere Arten aussendet \cite{datawedge}. Beispielsweise wird das Ergebnis an die Tastatur geschickt, aber eben auch als Broadcast an das Betriebssystem. Die App registriert sich beim Betriebssystem und hört auf den Broadcast, der den Barcode enthält und automatisch von DataWege entsandt wird. Broadcast werden durch eine String-ID unterschieden, die über DataWedge konfiguriert wird. Derartige Ansätze werden als Publish–subscribe-Model bezeichnet \cite{publish–subscribe}. 

Dies bietet folgende Vorteile:

* Die Hardware wird komplett abstrahiert. Die vorliegende App "sieht" den Scanner zu keinem Zeitpunkt.
* Durch die Abstrahierung des Scanners wird die eigene Codebasis kleiner und weniger kompliziert. 
* DataWedge wird von Zebra entwickelt. Damit hat man eine gewisse Sicherheit, dass der Scanner verlässlich funktioniert.

Bei weiterer Überlegung kommt man außerdem zur Erkenntnis, dass der Broadcast nicht von DataWege stammen muss. Wenn eine beliebige andere App, einen Broadcast mit derselben ID ausschickt, wird die vorliegende App dies als Scan werten. In weiterer Folge ist es zumindest theoretisch möglich, beispielsweise einen Bluetooth-Scanner mit einem regulären Android-Gerät zu verwenden und bei einem Resultat, einen Broadcast mit derselben ID auszuschicken. Die vorliegende App würde keinen Unterschied bemerken und somit auch mit dem Bluetooth-Scanner funktionieren. Dieses Einsatzgebiet wurde vom Diplomarbeitsteam jedoch nicht getestet.

## Zebra-Scan: Codeausschnitt

Broadcast können in Android durch `BroadcastReceiver` ausgelsen werden. Auch hier wurde das DRY-Prinzip angewandt. Jedes Fragment, das Scanergebnisse braucht, verwendet nahezu denselben BroadcastReceiver. Daher hat das Team einen eigenen BroadcastReceiver erstellt, der die gemeinsamen Eigenschaften zusammenführt. Der gesamte Code für den Zebra-Scan konnte damit relativ kompakt in einer Klasse eingebunden werden:

```java
public class ZebraBroadcastReceiver extends BroadcastReceiver {
    ...
    @Override
    public void onReceive(Context context, Intent intent) {
        String action = intent.getAction();

        // R.string.activity_intent_filter_action definiert 
        // die konfigurierte ID des Broadcasts
        if (action.equals(context.getResources()
           .getString(R.string.activity_intent_filter_action))) {
            // R.string.datawedge_intent_key_data 
            // definiert die ID des Scanergebnis selbst. 
            // Der Scan liefert auch andere Ergebnisse, 
            // wie z.B. das Barcodeformat. 
            // Relevant ist nur der Barcode.
            String barcode = intent.getStringExtra(
                context.getResources()
                .getString(R.string.datawedge_intent_key_data));

          
            // Das Scanergebnis wird nun per 
            // funktionalem Interface an die Fragments weitergegeben.
            scanListener.handleZebraScan(barcode);
        }
    }


    public static void registerZebraReceiver(
                            Context context, 
                            ZebraBroadcastReceiver zebraBroadcastReceiver, 
                            ErrorHandler errorHandler) {
        ...
        IntentFilter filter = new IntentFilter();
        filter.addCategory(Intent.CATEGORY_DEFAULT);
        // R.string.activity_intent_filter_action definiert 
        // die konfigurierte ID des Broadcasts
        filter.addAction(
            context.getResources()
            .getString(R.string.activity_intent_filter_action));
        // Hier wird der BroadcastReceiver mit der ID, 
        // auf die er zu hören hat, registriert.
        context.registerReceiver(zebraBroadcastReceiver, filter);
    }


    public static void unregisterZebraReceiver(
                    Context context, 
                    ZebraBroadcastReceiver zebraBroadcastReceiver) {
         // Falls ein BrodcastReceiver nicht mehr gebraucht wird
         // Sollte er immer abgemeldet werden.
        context.unregisterReceiver(zebraBroadcastReceiver);
    }
}
```




# Der Kamerascan

Für Geräte, die nicht dem Hause Zebra entstammen, bietet die App die Möglichkeit eines Kamerascans an. Im Hintergrund wird dafür die Google Mobile Vision API verwendet, die unter anderem auch Texterkennung oder Gesichtserkennung anbietet \cite{mobile-vision}. Hierbei wird ein Barcode mittels der Gerätekamera erfasst, ohne dass zuvor ein Bild gemacht werden muss. Dem Benutzer wird eine Preview angezeigt und die Kamera schließt sich, sobald ein Barcode erfasst wurde. Um die Performanz zu maximieren, hat das Diplomarbeitsteam folgende Optimierungen vorgenommen:

* Die API wurde um Blitzfunktionalitäten ergänzt. Dazu wurde eine OpenSource-Variante der Library modifiziert, da die offizielle proprietäre Version keinen Blitz unterstützt. Der Blitz ist über einen ToggleButton sofort deaktivierbar oder aktivierbar. Um einen Klick einzusparen, kann der Benutzer über die Einstellungen den Blitz gleich beim Start des Kamerascans aktivieren lassen. Für eine optimale Erkennung darf man den Blitz jedoch nicht direkt in Richtung des Barcode anvisieren, sondern sollte den Blitz entweder höher oder niedriger als den Barcode halten, um optische Reflexionen zu vermeiden. 
* Über die Einstellungen kann der Benutzer die Barcodeformate einschränken. Dies führt ebenfalls zur schnelleren Barcodeerkennung und vermeidet zudem noch das Auftreten von false positives. Wenn der Benutzer die Kamera nicht auf den gesamten Barcode hält, kann es unter Umständen dazu kommen, dass der abgeschnittene Barcode fälschlicherweise als anderes Format interpretiert wird und der Scan daher einen Barcode liefert, der nicht existiert. Offiziell wird in der vorliegenden Organisation nur das `Code_93`-Format eingesetzt.
* Interne Test haben ergeben, dass ein Aspect Ratio von 16:9 das Schnellste ist. Daher wird die Preview-Größe statisch auf 1920x1080 Pixel festgelegt. Die Preview verwendet jedoch tatsächlich die Größe, die am nähesten zu 1920x1080 ist und vom Gerät unterstützt wird.
* Falls ein Scan erfolgreich ist, wird ein Piepston abgespielt, der als akustisches Feedback fungiert.


## Kamerascan: Codeausschnitt

Für die Scanfeatures wird bezüglich der Single-Activity-App eine Ausnahme gemacht. Da diese Screens navigationstechnisch unabhängig sind und im Vollbildmodus gestartet werden, macht es mehr Sinn, sie als Activities anstatt als Fragments zu implementieren. Die Fragments, die einen Kamerascan verwenden, können ihn mit `startActivityForResult` aufrufen. Sie starten also eine neue Activity um ein Ergebnis zu erhalten:


```java
Intent intent = new Intent(getContext(), ScanBarcodeActivity.class);
// 0 ist der Request Code, der die Aktivität identifiziert. 
startActivityForResult(intent, 0);
```

Mit der Vision API erstellt man einen `BarcodeDetector` dem ein `Processor` zugewiesen wird. Falls ein Barcode erkannt wird, wird `receiveDetections` automatisch aufgerufen. Die App nimmt sich den ersten erkannten Barcode heraus, setzt ihn als Ergebnis dieser Aktivität und beendet die Aktivität.

```java
barcodeDetector.setProcessor(new Detector.Processor<Barcode>() {
        ...

     @Override
    public void receiveDetections(Detector.Detections<Barcode> detections) {
           ...
           // Barcode einlesen
           String barcode = barcodeSparseArray.valueAt(0).rawValue;

           //Barcode bekanntgeben
           Intent intent = new Intent();
           intent.putExtra("barcode", barcode);
           setResult(CommonStatusCodes.SUCCESS, intent);
           finish();
           ...
    }
```

Das Ergebnis kann dann wiederum im Fragment wie folgt abgefangen und weiter verarbeitet werden:

```java
@Override
public void onActivityResult(int requestCode, int resultCode, 
                             @Nullable Intent data) {
    if (requestCode == 0) {
        if (resultCode == CommonStatusCodes.SUCCESS) {
            ...    
            // Barcode einlesen
            String barcode = data.getStringExtra("barcode");
            // Anhand des Barcodes werden dann 
            // weitere Aktionen gesetzt
            launchItemDetailFragmentFromBarcode(barcode);
        }
        ...
}
```

# Die manuelle Eingabe

Gegenstände können durch Scannen, aber auch durch manuelles Klicken auf ihre GUI-Darstellung validiert werden. Es kann durchaus Sinn machen, auf einen Scan zu verzichten, wenn:

* der Scan langsam oder unmöglich ist (z.B. bei Barcodes in unerreichbarer Höhe oder beschädigten Barcodes).
* man einen Gegenstand auch ohne Barcode identifizieren kann.
* man mit einer manuellen Vorgehensweise schneller ist, als mit dem Kamerascan.

## Suche

Die Gegenstandsliste, die dem Benutzer angezeigt wird, ist durch eine Suchleiste (`SearchBar`) filterbar. Der Benutzer kann nach Barcode und Gegenstandsbeschreibung filtern. Dadurch ergibt sich auch die Möglichkeit, eine Voice-Tastatur - insofern das Gerät eine hat - einzusetzen.

## Textscan

Falls der Benutzer weder Voice noch Tastatur verwenden will, kann er den Textscan verwenden. Hierbei wird per Google Mobile Vision API der aufgedruckte Text auf einem Barcode - jedoch NICHT der Barcode selbst - gescannt \cite{text-scan}. Der Scan ist für Sprachen, die ein lateinisches Schriftsystem verwenden, optimiert. Dem Benutzer wird das aktuelle Scanergebnis laufend angezeigt. Da der Out-Of-The-Box-Scan für die vorliegenden Zwecke nicht unbedingt geeignet ist, wurden folgende Modifikationen vorgenommen (Der Textscan wurde als `TextScanActivity` realisiert):

* Der Scanner gibt grundsätzlich alles was er sieht wieder. Das Projektteam hat dies mit Regex-Ausdrücken kombiniert um sinnvolle Ergebnisse zu erlangen. Es gibt daher verschiedene Texterkennungsmodi:
    * Kein Filter. Hier wird keine Regex verwendet und der Benutzer sieht den ungefilterten Text, den der Scanner erkannt hat.
    * Alphanumerisch.
    * Alphabetisch. 
    * Numerisch (Barcodes).
    * IP-Adressen. 
Jeder Modus verwendet die Regex, die am besten für seine Kategorie geeignet ist. Die Library bietet nur geringfügige Möglichkeiten an, den Scan selbst zu beeinflussen. Man kann einen Fokus auf ein Scanergebnis setzen. Daher wird der Fokus auf ein Scanergebnis gesetzt, sobald dieses durch die aktuelle Regex ausgedrückt werden kann. Die API beschränkt sich allerdings nicht auf das Ergebnis, auf das der Fokus gesetzt wurde, sondern kann jederzeit den Fokus selbst wieder aufheben. Unabhängig davon werden alle Zeichen des Ergebnisses entfernt, die nicht durch die Regex erfasst wurden. 
* Die API wurde um Blitzfunktionalitäten ergänzt.

Die Erkennung von Barcodes (also der Numerische Modus) hat zusätzlich folgende Optimierungen erhalten:

* Gescannte Ergebnisse werden durch die Regex optimiert und gespeichert. Das Ergebnis, das zuerst dreimal vorkam, wird eingelockt. Das heißt, dass alle restlichen Scans ignoriert werden. 
* Zeichen die häufig vom Scanner verwechselt werden (z.B. wird die Ziffer "0" häufig als Buchstabe "O" erkannt), werden durch ihr numerisches Gegenstück ersetzt. 
* Längere Zeichenketten erhalten eine höhere Priorität und werden dem Benutzer vorzugsweise angezeigt. Damit werden abgeschnittene Ergebnisse benachteiligt. Falls ein kürzeres Ergebnis jedoch dreimal vorkommt, wird nicht mehr die längste Zeichenkette, sondern die häufigste bevorzugt und dem Benutzer angezeigt.
* Ergebnisse, die weniger als sieben Zeichen enthalten, werden nicht gespeichert und können damit auch nicht eingelockt werden.


Der Benutzer kann das aktuell angezeigte Ergebnis in seine Zwischenablage kopieren und anschließend die Suchleiste nutzen. Die Kommunikation zwischen den restlichen Screens und dem Textscan erfolgt analog zum Kamerascan, da der Textscan aus denselben Gründen als Activity implementiert wurde. 



\chapter{Die Inventurlogik auf der App}

# Die Modelle

Um die Antworten des Servers abzubilden, wurden mehrere Modell-Klassen erstellt. Deren Konstruktur hat ein JSONObject als Parameter. Die Werte der einzelnen Attribute werden aus diesem JSONObject ausgelesen. Folgende Modell-Klassen wurden erstellt.

* `SerializerEntry`: Stelt eine Datenbanksicht dar.
* `Stocktaking`: Stellt eine Inventur dar. 
* `Room`: Stellt einen Raum dar.
* `MergedItem`: Stellt einen Gegenstand dar.
* `MergedItemField`: Stellt ein dynamisches Feld dar. 
* `Attachment`: Stellt einen Anhang dar.  

## Grundsätze

Eine Modell-Klasse verwaltet etwaige Statusinformationen immer selbst. So weiß beispielsweise nur ein Gegenstand selbst, dass er ursprünglich aus einem anderen Raum stammt. Dies verbessert die Lesbarkeit und Wartbarkeit des Codes massiv, da diese Informationen abstrahiert sind und nicht mehrmals an verschiendenen Stellen im Quellcode schlummern. 

# Die Fragments

Dieses Kapitel basiert auf den Erklärungen der vorhergehenden Artikel. Eine Inventur wird auf der App durch folgenden Screens (Fragments) abgewickelt, die gleichzeitig als Phasen verstanden werden können:

* `StocktakingFragment`
* `RoomsFragment`
* `ViewPagerFragment`
    * `MergedItemsFragment`
    * `ValidatedMergedItemsFragment`
* `DetailedItemFragment`
* `AttachmentsFragment`

#### StocktakingFragment
ist das Fragment, in dem der Benutzer die aktuelle Inventur auswählt. Die Inventur kann nur vom Administrator am Server angelegt werden. Außerdem wählt der Benutzer hier die Datenbanksicht ("Serializer") aus. Die App kommuniziert ausschließlich mittels REST API mit dem Server. Diese Schnittstelle kann verschiedene Quellen haben. Für eine Inventur an unserer Schule ist die "HTL"-Datenbanksicht auszuwählen. Durch das Bestätigen eines langegezogenen blauen Buttons gelangt man immer zur nächsten Inventurphase. In diesem Fall gelangt man zum `RoomsFragment`.

#### RoomsFragment
ist das Fragment, in dem der Benutzer den aktuellen Raum über eine DropDown auswählt. Anstatt die DropDown zu verwenden, kann er alternativ auch die Suchleiste verwenden. Als zusätzliche Alternative hat der Benutzer die Möglichkeit den Barcode eines Raumes zu scannen. Nach der Auswahl eines Raumes gelangt er zum `ViewPagerFragment`. 

#### ViewPagerFragment
ist das Fragment, das als Wrapper für das `MergedItemsFragment` und das `ValidatedMergedItemsFragment` dient. Die einzige Aufgabe dieses Fragments ist es, die zwei vorher genannten Fragments als Tabs anzuzeigen. Dies wurde mit der neuen ViewPager2-Library realisiert \cite{viewpager2}. Die Tabs kommunizieren über ein geteiltes ViewModel miteinander.

#### MergedItemsFragment & ValidatedMergedItemsFragment

sind die Fragments, die die Gegenstandsliste eines Raumes verwalten und sie dem Benutzer anzeigen. Die Gegenstände werden einzeln validiert. Durch das Scannen des Barcodes eines Gegenstands (beziehungsweise das Klicken auf seine GUI-Repräsentation) gelangt er zum `DetailedItemFragment`. `ValidatedMergedItemsFragment` erfüllt nur den Zweck, dem Benutzer bereits validierte Gegenstände anzuzeigen und ihm die Möglichkeit zu geben, validierte Gegenstände zurück zu den nicht-validierten Gegenständen in `MergedItemsFragment` zu verschieben. Daher trägt der Tab für das  `MergedItemsFragment` die Beschriftung "TODO", währenddessen der Tab für das  `ValidatedMergedItemsFragment` die Beschriftung "DONE" trägt.

Beide Fragments verwenden eine `RecyclerView`, um dem Benutzer die Gegenstände anzuzeigen \cite{recyclerview}. Eine RecyclerView generiert pro Eintrag ein Layout, hält aber nur die aktuell angezeigten Einträge inkl. Layout im RAM.   

####  DetailedItemFragment
ist das Fragment, das zur Validierung eines einzelnen Gegenstands dient. Der Benutzer hat hier die Möglichkeit, etwaige Eigenschaften des Gegenstandes (beispielsweise den Anzeigenamen) zu ändern. Ein Formular, dass die Felder eines Gegenstandes beinhaltet, wird einmal angefordert, und anschließend für die gesamte Lebensdauer der App gespeichert. Anhand dieses Formulars wird dann eine GUI-Repräsentation dynamisch erstellt. Das Formular kann `ExtraFields` beinhalten. Das sind Felder, die als nicht essentiel angesehen werden und infolgedessen standardmäßig eingeklappt sind. Dazu gehören auch benutzerdefinierte Felder - sognannte `CustomFields`. 

Dadurch braucht man für die gesamte Validierung eines Raumes - insofern keine Sonderfälle auftreten - keine Verbindung zum Server. Der Benutzer kann die Liste an einer Lokalität mit einer guten Verbindung anfordern, den Raum mit schlechter Verbindung betreten und validieren. Anschließend kann er den Raum verlassen und seine Validierungen an den Server senden. Damit wird der Bedarf an Netzwerkanfragen in Räumen mit schlechter Netzwerkverbindung minimiert. Der Benutzer kann zusätzlich zur Validierung auch Anhänge für einen Gegenstand definieren, dazu landet er beim `AttachmentsFragment`.

Folgende GUI-Komponenten wurden statisch implementiert, da sie Felder repräsentieren, die unabhängig von der ausgewählten Datenbanksicht immer vorhanden sind und daher nicht dynamisch sind:

* Ein read-only Textfeld für die Gegendstandsbeschreibung
* Ein read-only Textfeld für den Barcode
* Eine Checkbox "Erst später entscheiden"
* Eine DropDown für Subrooms


#### AttachmentsFragment
ist das Fragment, das die Anhänge eines Gegenstandes verwaltet. Der Benutzer sieht hier die bereits vorhandenen Anhänge mit Beschriftungen und kann weitere Anhänge hinzufügen. Bilder werden direkt angezeigt. Andere Dateien werden hingegen als Hyperlink dargestellt. Der Benutzer kann diese per Browser runterladen. Zum Hochladen eigener Anhänge greift die App auf den Standard-Dateibrowser des Systems zurück. Das Outsourcen auf Webbrowser und Dateibrowser bietet den massiven Vorteil, dass man sich die Entwicklung eigener Download-Manager bzw. File-Manager erspart und auf Apps setzen kann, die von namenhaften Herstellern entwickelt werden. Fast jedes System hat bereits beide Komponenten vorinstalliert, daher treten bezüglich der Verfügbarkeit keine Probleme auf. 

Beim dem Hochladen von Bildern komprimiert die App jene zuvor. Die Qualität des Bildes ist einstellbar:

* 100 % (keine Kompression)
* 95 %
* 85 %
* 75 %

Zur Kompression wird das WEBP-Format verwendet, das dem mittlerweile veralteten JPG-Standard überlegen ist \cite{webp}. Der Server speichert den gesendeten Anhang nur einmal (Dateien werden anhand von Hashes unterschieden). Wenn ein Benutzer allen PCs in einem EDV-Saal dasselbe Bild zuweist, wird es nur einmal am Server hinterlegt. Die Anzahl der Anhänge ist nicht limitiert.

# Validierungslogik

`MergedItemsFragment` & `DetailedItemFragment` sind die Fragments, die den Großteil einer Inventur ausmachen. Der Benutzer scannt alle SAP-Barcodes, die sich in einem Raum befinden. Im Idealfall entspricht diese Menge exakt der Menge der Gegenstände, die dem Benutzer im MergedItemsFragment angezeigt wird. Im Normalfall wird dies durch etwaige Sonderfälle jedoch nicht gegeben sein. 

Nach dem Scannen eines Gegenstandes werden die Felder des Gegenstandes dem Benutzer im DetailItemFragment angezeigt. In diesem Fragment hat der Benutzer zwei Buttons, mit denen er den Gegenstand validieren kann:

* Grüner Button: Mit diesem Button wird signalisiert, dass sich der Gegenstand im Raum befindet und der Gegenstand wird mitsamt etwaigen Änderungen an seinen Attributen/Feldern übernommen. Dazu wird ein `ValidationEntry` erstellt. Alternativ kann der Benutzer das Klicken dieses Buttons mit dem Schütteln des Gerätes ersetzen. Die Schüttel-Sensibilität ist über die Einstellungen konfigurierbar (und auch deaktivierbar).
* Roter Button: Mit diesem Button wird signalisiert, dass sich der Gegenstand nicht im Raum befindet. Dieser Button wird im Normalfall nie betätigt werden, da ein Gegenstand, der sich nicht in diesem Raum befindet, nicht gescannt werden kann und daher dieses Fenster nie geöffnet werden wird. Der Button hat trotzdem einen Sinn, da der Benutzer damit die "TODO"-Liste über das GUI verkleinern kann, um sich einen besseren Überblick zu verschaffen. 

## Der ValidationEntry

Das MergedItemsFragment (bzw. das MergedItemsViewModel) verfügt über eine HashMap (`Map<MergedItem, List<ValidationEntry>>`), die alle ValidationEntries beinhaltet. Ein ValidationEntry beinhaltet sämtliche Informationen, die der Server benötigt, um die Datensätze eines Gengenstandes entsprechend anzupassen.
Einem Gegenstand ist eine Liste an ValidationEntries zugeordnet, da Subitems eigene ValidtionEntries bekommen können (siehe ["Sonderfälle"](#sonderfuxe4lle)). 

Ein ValidationEntry beinhaltet immer den Primary Key eines Gegenstandes und die Felder, die sich geändert haben. Ein ValidationEntry hat daher eine Liste an Feldern `List<Field>`. Wenn sich der Wert eines Feldes geändert hat, wird diese Liste um einen Eintrag erweitert. Da die Felder wie erwähnt dynamisch sind, wurden Java Generics eingesetzt \cite{java-generics}, um diese abbilden zu können. `Field` ist eine innere Klasse in `ValidationEntry`:

```java
public static class Field<T> {
    private String fieldName;
    private T fieldValue;
    ...
```

Ein Feld besteht also immer aus einem Feldnamen und einem generischen Feldwert. Selbiges gilt für ExtraFields.

### Sendeformat
 
Wenn ein Raum abgeschlossen ist, werden alle ValidationEntries in einer Liste vereint und anschließend in eine JSON-Darstellung transformiert. Dieses JSON wird dem Server gesandt und damit ist der aktuelle Raum abgeschlossen und der Benutzer kann sich den restlichen Räumen annehmen. Das POST-Format wird im Kapitel ["JSON-Schema"](json-schema) beschrieben.

## Quickscan

Der häufigste Fall einer Inventur wird der sein, dass ein Gegenstand im richtigen Raum ist und der Benutzer ohne weiteren Input auf den grünen Button drückt. Da dies einen unnötigen Overhead darstellt, wurde die App um den `QuickScan`-Modus erweitert. Hierbei wird sofort nach dem Scannen ein `ValidationEntry` erstellt, ohne dass zuvor das DetailedItemFragment geöffnet wird. Dieser Modus ist durch einen weiteren Button im MergedItemsFragment aktivierbar/deaktivierbar. Falls ein Sonderfall auftreten sollte, vibriert das Gerät zweimal und öffnet das  DetailedItemFragment. Damit wird gewährleistet, dass der Benutzer nicht irrtümlich mit dem Scannen weitermacht. Er muss diesen Sonderfall händisch validieren. Haptisches Feedback ist für Sonderfälle reserviert.

## Sonderfälle auf der App

Ein zentrales Thema der vorliegenden Diplomarbeit ist die Behandlung der Sonderfälle. 

### Subitems

Falls ein Gegenstand mehrmals vorhanden sein sollte, ist der `times_found_last`-Zähler in der Antwort des Servers größer als 1 und der Gegenstand gilt als Subitem. Dieser Counter wird dem Benutzer in folgender Form angezeigt: `[Anzahl aktuell gefunden] / [Anzahl zuletzt gefunden]`. 

Pro Subitem wird ein eigener ValidationEntry erstellt. An unserer Schule haben Subitems jedoch keinen Barcode sondern sind beispielsweise Teil eines Bundles, was dazu führt, dass Subitems auch in der Datenbank keine selbstständigen Gegenstände sind. CPs die auf Basis dieser ValidationEntries erstellt werden, werden dem echten "Parent"-Gegenstand zugeordnet und können wahlweise angewandt werden. Falls ein Gegenstand mehrmals gescannt wird, wird - nach Bestätigung durch den Benutzer - die Anzahl der aktuellen Funde erhöht und wiederum ein ValidationEntry erstellt, selbst wenn es sich bei dem Gegenstand aktuell nicht um ein Subitem handelt.

### Subrooms

Subrooms sind logische Räume in einem Raum. Subrooms werden dem Benutzer im MergedItemsFragment als einklappbare Teilmenge aller Gegenstände des Raumes angezeigt. Die Subroom-Zugehörigkeit kann auf Gegenstandsbasis über eine DropDown geändert werden. Die Subroom-Zugehörigkeit wird in einem ValidationEntry immer gesetzt, auch wenn sie sich nicht geändert hat. 

Die Gegenstandsliste des MergedItemsFragment beinhaltet in Wahrheit auch die Subrooms. Dies ist notwendig, da die `RecyclerView`, die dazu genutzt wird dem Benutzer die Gegenstände anzuzeigen, keine Möglichkeit bietet, eine Hierarchie bzw. Zwischenebenen darzustellen. Daher implementieren das Room-Model und das MergedItem-Model das Interface `RecyclerViewItem` und die RecyclerView erhält eine Liste an RecyclerViewItems - dies ist ein typisch polymorpher Ansatz. Abhängig vom Typen des aktuellen Listenelements baut die RecyclerView entweder ein Gegenstandslayout oder ein Raumlayout auf. 

### Unbekannte Gegenstände

Falls ein Gegenstand gescannt wird, der sich nicht in der aktuellen Gegenstandsliste befindet, muss der Server befragt werden. Es gibt zwei mögliche Antwortszenarien. Die ValidationEntries für diese Sonderfälle unterscheiden sich nicht von den bisherigen.

##### Neuer Gegenstand
Der Gegenstand befindet sich überhaupt nicht in der Datenbank. Im "DONE"-Tab haben solche Gegenstände eine blaue Hervorhebung.

##### Gegenstand aus anderem Raum
Der Gegenstand befindet in der Datenbank und stammt ursprünglich aus einem anderen Raum. Im "DONE"-Tab haben solche Gegenstände eine orange Hervorhebung.



