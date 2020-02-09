\chapter{Einführung in die App-Architektur}

Wie im vorherigen Kapitel angesprochen, ist das Ziel der Diplomarbeit, eine App zu entwickeln, mit der man in der Lage ist, eine Inventur durchzuführen. Doch wieso eine App und wieso überhaupt Android? Um diese Frage zu klären, muss man zwischen zwei Begriffen unterscheiden \cite{native-vs-web}:

* Native App
* Web-App

Nativ vs. Web
=============================

Unter einer nativen App versteht man eine App, die für ein bestimmtes Betriebssystem geschrieben wurde \cite{native-definition}. 
Die Definition ist allerdings nicht ganz eindeutig, da Frameworks wie Flutter und Xamarin nativer Funktionalität sehr nahekommen, obwohl sie mehrere verschiedene Betriebssysteme unterstützten. 
Eine Web-App hingegen basiert auf HTML und wird per Browser aufgerufen. Sie stellt nichts anderes als eine für mobile Geräte optimierte Website da. 

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
Eine gemeinsame Codebasis wird oftmals unter dem Begriff \emph{cross-platform} zusammengefasst und bedeutet, dass man eine mit Flutter entwickelte native App sowohl mit Android-Geräten als auch mit IOS-Geräten verwenden kann. 
Flutter ist eine relativ neue Plattform – das erste stabile Release wurde erst im Dezember 2018 veröffentlicht \cite{flutter-stable}. Außerdem verwendet Flutter die Programmiersprache \emph{Dart}, die Java ähnelt. 
Diese Umstände sind ein Segen und Fluch zugleich. Flutter wird in Zukunft sicherlich weiterhin an Popularität zulegen, allerdings ist die Anzahl an verfügbarer Dokumentation für das junge Flutter im Vergleich zu den anderen Optionen immer noch weitaus geringer. 

Xamarin ist ebenfalls ein cross-platform Framework, das jedoch in C# geschrieben wird und älter (und damit bewährter) als Flutter ist. Weiters macht Xamarin von  der proprietären .NET-Platform Gebrauch. Infolgedessen haben alle Xamarin-Apps Zugriff auf ein umfassendes Repertoire von .NET-Libraries \cite{xamarin-details}. Da Xamarin und .NET beide zu Microsoft gehören, ist eine leichtere Azure-Integration oftmals ein Argument, das von offzielen Quellen verwendet wird. Xamarin wird - anders als die restlichen Optionen - bevorzugterweise in Visual Studio entwickelt \cite{xamarin-vs}.

Native IOS wird nur der Vollständigkeit halber aufgelistet, stellte allerdings zu keinem Zeitpunkt eine wirkliche Alternative da, weil IOS-Geräte einige Eigenschaften besitzen, die für eine Inventur nicht optimal sind (z.B. die Akkukapazität). Außerdem haben in etwa nur 20% aller Geräte \cite{ios-market-share} IOS als Betriebssystem und die Entwicklung einer IOS-App wird durch strenge Voraussetzungen äußerst unattraktiv gemacht. So kann man beispielsweise nur auf einem Apple-Gerät IOS-Apps entwickeln.

### Begründung: Natives Android (Java)

Die Entscheidung ist schlussendlich auf natives Android (Java) gefallen. Es mag zwar vielleicht nicht die innovativste Entscheidung sein, stellt aber aus folgenden Gründen die bewährteste und risikoloseste Option da:

* Natives Android ist eine allbekannte und weit etablierte Lösung. Die Wahrscheinlichkeit, dass die Unterstützung durch Google eingestellt wird, ist also äußerst gering.
* Die App wird in den nächsten Jahren immer noch am Stand der Technik sein.
* Natives Android hat mit großem Abstand die umfassendste Dokumentation.
* An der Schule wird Java unterrichtet. Das macht somit eventuelle Modifikationen nach Projektabschluss durch andere Schüler viel einfacher möglich.
* Dadurch, dass Kotlin erst seit 2019 \cite{kotlin-preference} offiziell die von Google bevorzugte Sprache ist, sind die meisten Tutorials immer noch in Java.
* Sehr viele Unternehmen haben viele aktive Java-Entwickler. Dadurch wird die App attraktiver, da die Unternehmensmitarbeiter (von z.B. allegro) keine neue Sprache lernen müssen, um Anpassungen durchzuführen.
* Das Projektteam hat im Rahmen eines Praktikums bereits Erfahrungen mit nativem Java gesammelt.

Aus den Projektzielen hat sich in Absprache mit den Betreuern ergeben, dass die App nicht auf jedem "Steinzeitgerät" zu funktionieren hat.
Das minimale API-Level der App ist daher 21 - auch bekannt als Android 5.0 'Lollipop'. 
Android 4.0 hat sehr viele nützliche Libraries hervorgebracht. So zum Beispiel die \emph{Mobile Vision API} von Google, dank derer man in der Lage ist, Barcodes in akzeptabler Zeit mit der Kamera des Geräts zu scannen. Die Wahl ist auf 5.0 gefallen, da somit ein Puffer zur Verfügung steht und in etwa 90% aller Android-Geräte ohnehin auf 5.0 oder einer neueren Version laufen \cite{android-versions-market-share}. 



# Einführung zu nativem Java

Um eine Basis für die folgenden Kapitel zu schaffen, werden hier die Basics der Android-Entwicklung mit nativem Java näher beschrieben. Das Layout einer App wird in XML Files gespeichert, während das wirkliche Programmieren mit Java erfolgt.


## Single-Activity-App

Als Einstiegspunkt in eine App dient eine sogenannte \emph{Activity}. Eine Activity ist eine normale Java-Klasse, der durch Vererbung UI-Funktionen verleiht werden. 

Bis vor kurzem war es üblich, dass eine App mehrere Activities hat. Das wird bei den Benutzern dadurch bemerkbar, dass die App z.B. bei einem Tastendruck ein weiteres Fenster öffnet, das das bisherige überdeckt. Das neue Fenster ist eine eigene Activity. Google hat sich nun offiziell für sogenannte Single-Activities ausgesprochen \cite{single-activity}. Das heißt, dass es nur eine Activity und mehrere \emph{Fragments} gibt. Ein Fragment ist eine Teilmenge des UIs bzw. einer Activity. Anstatt jetzt beim Tastendruck eine neue Activity zu starten, wird einfach das aktuelle Fragment ausgetauscht. Dadurch, dass keine neuen Fenster geöffnet werden, ist die User Expierence (UX) um ein Vielfaches besser – die Performanz leidet nur minimal darunter. Die vorliegende App ist aus diesen Gründen ebenfalls eine Single-Activity-App.

## Separation of Concerns

In Android ist es eine äußerst schlechte Idee, sämtliche Logik in einer Activity oder einem Fragment zu implementieren.
Das softwaretechnische Prinzip \emph{Separation of Concerns (SoP)} hat unter Android einen besonderen Stellenwert. Dieses Prinzip beschreibt im Wesentlichen, dass eine Klasse nur einer Aufgabe dienen sollte. Falls eine Klasse mehrere Aufgaben erfüllt, so muss diese auf mehrere logische Komponenten aufgeteilt werden.
Beispiel: Eine Activity hat immer die Verantwortung, die Kommunikation zwischen UI und Benutzer abzuwickeln. Bad Practice wäre es, wenn jene Activity ebenfalls dafür verantwortlich ist, Daten von einem Server abzurufen.
Das Prinzip verfolgt das Ziel, die \emph{God Activity Architecture (GAA)} möglichst zu vermeiden \cite{god-activities}. 
Eine God-Activity ist unter Android eine Activity, die die komplette Business-Logic beinhaltet und SoP in jeglicher Hinsicht widerspricht.
God-Activities gilt es dringlichst zu vermeiden, da sie folgende Nachteile mit sich bringen:

 * Refactoring wird kompliziert
 * Wartung und Dokumentation werden äußerst schwierig
 * Automatisiertes Testing (z.B. Unit-Testing) wird nahezu unmöglich gemacht
 * Größere Bug-Anfälligkeit
 * Im Bezug auf Android gibt es oftmals massive Probleme mit dem \emph{Lifecycle} einer Activity - da eine Activity und ihre Daten schnell vernichtet werden können (z.B. wenn der Benutzer sein Gerät rotiert und das Gerät den Bildschirmmodus wechselt)
 
God-Activities sind ein typisches Beispiel für Spaghetticode. Es bedarf also einer wohlüberlegten und strukturierten Architektur, um diese Probleme zu unterbinden. 
Im nächsten Kapitel wird dementsprechend die Architektur der App im Detail erklärt.
 
