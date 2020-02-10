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

Xamarin ist ebenfalls ein cross-platform Framework, das jedoch in C# geschrieben wird und älter (und damit bewährter) als Flutter ist. Weiters macht Xamarin von  der proprietären .NET-Platform Gebrauch. Infolgedessen haben alle Xamarin-Apps Zugriff auf ein umfassendes Repertoire von .NET-Libraries \cite{xamarin-details}. Da Xamarin und .NET beide zu Microsoft gehören, ist eine leichtere Azure-Integration oftmals ein Argument, das von offzielen Quellen verwendet wird. Xamarin wird - anders als die restlichen Optionen - bevorzugterweise in Visual Studio entwickelt \cite{xamarin-vs}.

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

MVVM steht für Model–view–viewmodel\cite{mvvm-wiki}. Wie man am Namen bereits erkennt, gilt es zwischen drei Komponenten/Ebenen zu unterscheiden \cite{mvvm-article}. 

#### Model 
Model beschreibt die Ebene der Daten und wird daher oftmals auch als Datenzugriffsschicht bezeichnet. Diese Ebene beinhaltet so viel Anwendungslogik wie möglich.

#### View
View beschreibt die graphische Ebene und umfasst daher das GUI. Diese Ebene soll so wenig Logik wie möglich beinhalten. 

#### ViewModel
Das ViewModel dient als Bindeglied zwischen dem Model und der View. Die Logik der View wird in diese Ebene hinaufverschoben.  

# MVVM in Android

Mit der Einführung der Architecture Components hat Google Android-Entwicklern eine Vielzahl an Libraries zur Verfügung gestellt, um MVVM auch unter Android implementieren zu können. Die konkrete Implementierung unter Android ist in der Abbildung zu finden.

![MVVM in Android nach Google \cite{mvvm}](josip-pics\mvvm.png)

In dem vorliegenden Fall ist unser Fragment die `View`, das Repository das `Model` und das `ViewModel` ist in Android namensgleich.

## Das Repository im Detail

Wie in der Grafik veranschaulicht, ist das Repository dafür zuständig, Daten vom Server anzufordern. Im vorliegenden Fall besteht kein Grund eine Datenbank am Client zu führen. Damit fällt dieser Aspekt der Grafik - der in violetter Farbe gehalten ist - für die vorliegende Diplomarbeit weg. Die Aufgabe des Repositories ist also immer die angefragten Daten zu beschaffen. 

### JsonRequest

Die Kommunikation zwischen dem Client und der App findet ausschließlich im JSON-Format statt. JSON ist ein text-basiertes kompaktes Datenaustauschformat. Android bietet Entwicklern eine Out-of-the-box Netzwerklibrary namens `Volley` an, mithilfe derer man unter anderem JSON-Anfragen verarbeiten kann \cite{volley}. Da diese für die vorliegenden Zwecke nicht komplett geeignet war, hat das Diplomarbeitsteam die gegebene Library durch den Einsatz von Vererbung und einer Wrapper-Klasse modifiziert. Die Library wurde in folgenden Punkten angepasst:

 * Im Falle eines Fehlers wird die Anfrage wiederholt (Ausnahme: Zeitüberschreitungsfehler). Die maximale Anzahl an Wiederholungen ist limitiert. 
 * Die maximale Zeit Timeout-Dauer wurde erhöht.
 * Leere Antworten werden vom Projektteam als valide Antwort behandelt und können ohne Fehler verarbeitet werden.
 * Im Header der Anfrage wird der Content-Type der Anfrage auf JSON festgelegt.
 * Im Header wird das zur Authentikation notwendige API-Token mitgeschickt. Die Authentifizierung ist über einen Parameter deaktivierbar. 
 * Im Header wird die Systemsprache des Clients als standardisierter ISO-639-Code mitgesendet. Der Server passt seine Antwort auf die verwendete Sprache an \cite{ISO-639}. Die Bezeichnung der Felder, die dem Benuter auf Gegenstandsbasis angezeigt werden, ist beispielsweise abhängig von der Systemsprache.
* Zum Zeitpunkt der Anfrage ist nicht bekannt, ob die Antwort als JSONArray oder als JSONObject erfolgen wird. Da das Backend anhängig von der Anfrage sowohl mit einem JSONArray oder einem JSONObject antworten kann, ist der Rückgabewert am Client immer ein String. Die Umwandlung erfolgt erst im Repository. 

Diese Unterscheidung zwischen JSONArray und JSONObject ist notwendig, da Android zwei Möglichkeiten anbietet, JSON als Objekt zu speichern:

Direkt als JSONArray:
```java
JSONArray jsonArray = new JSONArray(payload);
```
Direkt als JSONObject:
```java
JSONObject jsonObject = new JSONObject(payload);
```
Beide Optionen haben einen Konstruktor, der einen String akzeptiert (im obrigen Beispiel ist dies der String `payload`). Im folgenden Beispiel stelle man sich vor, dass das Backend folgende Antworten sendet:


Dies ist ein JSONArray:
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


Dies ist ein JSONObject:
```json
{
  "id": 1,
  "item": "PC"
}
```

Wenn das Backend ein JSONArray sendet und man versucht jenes als JSONObject zu speichern, kommt es zu einem Fehler:
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



* `Context context`: Ist eine Schnittstelle die globale Information über die App-Umgebung zur Verfügung stellt \cite{context} und von Android zur Verfügung gestellt hat. Jede UI-Komponente (z.B. Textfelder, Buttons, Fragments, etc.) verfüguen über einen Context. Ein besonderer Context ist der globale Application-Context. Dieser ist einzigartig und besteht ist eine Singleton.  
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
Ein Interace mit einer einzigen abstrakten Methode ist als funktionales Interface zu bezeichnen \cite{lambda}. Da Android Studio Java 8 Language Features unterstützt, verwendet die App aussschließlich Lambda-Ausdrücke \cite{java8}. Lambda-Ausdrücke sind im Wesentlichen dazu da, funktionale Interfaces in vereinfachter Schreibweise verwenden zu können. Da es nur eine einzige abstrakte Methode gibt, kann die Schreibweise simplifiziert werden, da klar ist von welcher Methode die Rede ist. Damit fallen - wie im obigen Beispiel zu sehen - redudante Informationen wie Rückgabetyp und Methodenkörper vollständig weg. Das obige Beispiel würde ohne Lambda-Ausdrücke wie folgt ausschauen:

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
Lambdas sind eine Option, um `Callbacks` in Java zu implementieren. Ein Callback ("Rückruffunktion") ist eine Methode die einer anderen Methdoe als Parameter übergeben werden kann. Die soeben erwähnten funktionale Interfaces sind typische Callbacks \cite{Callbacks}.
Es gibt zwei Arten von Callbacks:

* Synchrone Callbacks: Die Ausführung der übergebenen Methode erfolgt sofort
* Asynchrone Callbacks: Die Ausführung der übergebenen Methode erfolgt später

In diesem Fall wird die Methode `handleSuccess` aufgerufen, sobald der Client die Antwort erhalten hat. Damit handelt es sich um ein asynchrones Callback.

### Retrofit 

`Retrofit` ist eine weitere Netzwerk-Library, die das Projektteam eingesetzt hat. Retrofit wurde nur zum Senden von Dateien eingesetzt, weil dies mit Volley nur erschwert möglich ist. Das Projektteam die von Retrofit zur Verfügung gestellten Libraries nicht wesentlich modifiziert.
Da die Anhangfunktion das einzige Einsatzgebiet von Retrofit darstellt, nimmt Volley in der vorliegenden Diplomarbeit die weitaus wichtigere Rolle ein. Volley kann grundsätzlich alles was Retrofit kann und vice versa. Aufgrunddessen dass Volley jedoch mehr Möglichkeiten zur Zuschneidung auf einen spezifischen Use-Case hat, hat sich das Projektteam für Volley entschieden. Ein Vorteil der jedoch damit verloren geht, ist das automatische Parsing von JSON-Antworten, dass zwar in Retrofit implementiert ist, jedoch nicht in Volley. \cite{retrofit-vs-volley}


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
Dies führt zu einem besseren Überblick als bei Volley, da man pro API-Endpunkt des Backends ein Interface hat. Damit wird sofort ersichtlich, mit welchen Backend-Endpunkten ein Repository kommuniziert. 



## Das ViewModel und Fragment im Detail

Das ViewModel ist eine Klasse, die dafür ausgelegt ist, UI-bezogene Daten in lifecycle-aware speichert. Ein Fragment durchlebt im Laufe seines Daseins eine Vielzahl an Zuständen - man spricht von einem Lifecycle. Wenn der Benutzer zum Beispiel sein Gerät rotiert, führt dies dazu, dass der Zustand des Fragments geändert wird und das Fragments alte Zustände wieder durchleben muss - dies führt zu einer Zerstörung des aktuellen UIs des Fragments sowie sämtlicher Referenzen, die das Fragment besitzt. Eine Gerätrotierung gehört zur Kategorie der `Configuration Changes` \cite{viewmodel}. Folgende Probleme können dadurch auftreten:

* Die App stürzt App. Wenn eine Methode ausgeführt wird, die eine Referenz auf ein zerstörtes Objekt hat, kann dies zum Absturz der gesamten App führen.
* Memory Leaks entstehen, da Referenzen auf zerstörte Objekte vom Gargabe Collector nicht freigegeben werden können. In Android wird die Minimierung des Speicherbedarfs der App einzig und allein vom Garbage Collector übernommen. Falls dieser Objekte nicht freigeben kann, führt dies dazu, dass die App immer mehr und mehr Arbeitsspeicher benötigt. Je nach Größe des Memory Leaks kann dies zu kleineren Lags bis zu einem Absturz der App führen.
* Nach einem Configuration Change gehen die aktuellen Daten verloren und der Benutzer muss das Problem selbst lösen.


Wie in der Abbildung zu sehen ist, stellt ein ViewModel eine Lösung für diese Probleme dar. Ein ViewModel überlebt Configuration Changes und kann dem UI damit stets die aktuellen Daten zur Verfügung stellen. 

![Zustände einer Activity im Vergleich zu den Zuständen eines ViewModels \cite{viewmodel}](josip-pics\viewmodel-lifecycle.png)

Folgende Aspekte hat man dennoch zu beachten \cite{viewmodel-antipatterns}:

* ViewModel sollte bei einem Configuration Changes keinen neuen Request starten, da es bereits über die Daten verfügt. Dies lässt sich mit einer If-Anweisung beheben.
* Referenzen zu Objekten, die an einen Lifecycle gebunden sind, ist ein absolutes NO-GO. Objekte mit Lifecycle haben ein klares Schicksal - wenn ihr Host vernichtet wird, müssen sie ebenfalls vernichtet werden. Folgendes Szenario: Ein ViewModel hat eine `TextView`-Variable. Dreht der Benutzer sein Gerät wird das aktuelle Fragment inklusive TextView vernichtet. Das ViewModel überlebt den Configuration Change und hat nun eine Referenz auf eine invalide TextView. Dies ist ein Memory Leak.
* ViewModel überleben ein Beenden des Prozesses nicht. Wenn das BS aktuell wenig Ressourcen zur Verfügung hat, kann es sein, dass Prozesse beendet werden. Falls man diesen Sonderfall behandeln will, ist dies mit Extra-Aufwand verbunden \cite{viewmodel-process-death}.
* ViewModels sollen nicht zu "God-ViewModels" werden. Das SoC-Prinzip ist anzuwenden.


Wie gelangen die Daten ins UI, wenn das ViewModel keine Referenzen darauf haben kann? Die Antwort lautet `LiveData`. 


### LiveData

LiveData ist eine observierbare Container-Klasse. Observierbar heißt, dass bei Änderungen des Objektes ein Callback aufgerufen wird. LiveData ist ebenfalls lifecycle-aware. Daher wird LiveData immer nur aktive Komponenten mit Daten versorgen. Eine TextView, die bereits zerstört wurde,erhält dementsprechend auch keine Updates mehr. 

Dieses (angepasste) offizielle Beispiel veranschaulicht die Funktionsweise sehr gut \cite{livedata}. Im Beispiel soll ein Benutzername angezeigt werden:

```java
public class NameViewModel extends ViewModel {

// LiveData-Objekt das einen String beinhaltet
private MutableLiveData<String> currentName;

    public LiveData<String> getCurrentName() {
        if (currentName == null) {
            currentName = new MutableLiveData<String>();
        }
        return currentName;
    }

// Rest des ViewModels...
}
```

Das ViewModel stellt LiveData bereit. Im Realfall stammt LiveData ursprünglich aus dem Repository. 

```java
public class NameFragment extends Fragment {
 @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        ...

        // Mit dieser Anweisung wird ein ViewModel erstellt
        model = new ViewModelProvider(this).get(NameViewModel.class);

        model.getCurrentName().observe(getViewLifecycleOwner(), currentName -> {
                // Diese Methode wird bei Änderugen aufgerufen, currentName ist ein String.
                // nameTextView ist ein TextFeld, dass den aktuellen Benutzernamen anzeigt.
                // Mit .setText kann der Anzeigename geändert werden.
                nameTextView.setText(newName);
        });
}
```
Hier kommt wieder die vorher angesprochene Lambda-Syntax zum Einsatz. Die Methode wird einmal beim erstmaligen Registrieren aufgerufen und wird danach bei jeder weiteren Änderung aufgerufen. Im Callback arbeitet man direkt mit dem eigentlichem Datentypen - in diesem Fall mit einem String -, da LiveData nur ein Container-Objekt ist. Im Fragment befindet sich damit relativ wenig Logik. Das Fragment hört nur auf eventuelle Änderungen und aktualisiert das UI in Abhängigkeit von den Änderungen. Das LiveData-Objekt ist an `getViewLifeycleOwner()` gebunden. Wenn der LifecycleOwner inaktiv wird, werden keine Änderungen mehr entsandt. Man könnte auch `this` als Argument übergeben (`this` wäre in diesem Fall das Fragment, das ebenfalls über einen Lifecycle verfügt). `getViewLifeycleOwner()` hat jedoch den Vorteil, dass der Observer automatisch entfernt wird, sobald der LifecycleOwner zerstört wird. 

STATES....



# Konkrete MVVM-Implementierung

Nun ist klar wie eine Netzwerkanfrage durchgeführt wird. Das Repository übernimmt das Verarbeiten der Antwort. Im vorliegenden Anwendungsfall hat ein Fragment immmer mindestens einen Datensatz, der für das Fragment namensgebend ist. Beispielsweise ist das `RoomsFragment` dafür verantwortlich, die Raumliste anzuzeigen. Das `RoomsRepository` ist dafür verantwortlich, die Raumliste vom Backend anzufordern und in Java-Objekte umzuwandeln. Da dies auf alle Fragments zutrifft, 