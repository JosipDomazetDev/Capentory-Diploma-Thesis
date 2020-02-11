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
 
\chapter{MVVM}

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


\chapter{Konkrete MVVM-Implementierung}

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

Hier greift das softwaretechnische Prinzip `Do not repeat yourself (DRY)` \cite{driy}. Anstatt `Boilerplate-Code` für jeden einzelnen Screen kopieren zu müssen, hat das Projektteam diese sich wiederholende Logik abstrahiert. Boilerplate-Code sind Code-Abschnitte, die sich immer wieder wiederholen \cite{boiler}. Wiederholende Logik sollte immer in eine Superklasse abstrahiert werden. Das Projektteam hat demnach drei abstrakte Klassen definiert, die die Menge an Boilerplate-Code signifikant reduzieren:

* `NetworkRepository`
* `NetworkViewModel`
* `NetworkFragment`

Alle Komponenten von Screens, die Netzwerkanforderungen durchführen, erben von diesen drei Klassen. Damit hat das Projektteam folgende Vorteile aggregiert: 

* Abstrahierte Fehlerbehandlung
* Abstrahiertes Refreshverhalten
* Abstrahierte Ladeanzeige (durch eine `ProgressBar`)
* Abstrahierter Netzwerkzugriff

Der Refactor, der dies realisierte, war zwar zeitintensiv, hat sich jedoch mittlerweile mehr als rentiert. Das Hinzufügen von neuen Screens benötigt nur mehr einen Bruchteil des ursprünglichen Codes. Infolgedessen wird das Erweitern der App um neue Features enorm erleichtert. 


# NetworkRepository

TODO

# NetworkViewModel

TODO

# NetworkFragment

TODO