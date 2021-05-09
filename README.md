# KSwe SoSe 2021 - Aufgabe 4 - 2

Ziel dieser Aufgabe ist es, ein Softwareprojekt mit dem Build Automation Tool Gradle zu erstellen. Dabei sollen die
grundlegenden Konfigurationsparameter in einem Gradle Build-File kennengelernt sowie der Build-Prozess durch eigene
Tasks angepasst werden.

## Beschreibung
Es soll eine einfach Java-Anwendung mit Gradle gebaut werden. Der Build-Ablauf soll so angepasst werden, dass eine
ausführbare JAR-Datei in einem selbst zu wählenden Ordner erstellt wird. Außerdem ist ein eigener Task zu erstellen,
der einen erfolgreichen Build in eine Log-Datei schreibt.

## Vorgehensweise
**1. Projekt initialisieren**  
Klont dieses GitHub-Projekt auf euren lokalen Rechner. Das Projekt ist mit einem _build.gradle_ File für Java-Projekte
vorkonfiguriert. Außerdem enthält das Projekt bereits einen Gradle-Wrapper, der direkt nutzbar ist, sodass es nicht
notwendig ist, Gradle herunterzuladen und zu installieren. Importiert das Projekt in eure IDE.

**2. Gradle Build durchführen**  
Führt einen Gradle-Build des Projekts aus, indem ihr im Root-Verzeichnis eures Projekts `.\gradlew build` in einer
Kommandozeile ausführt. Nach erfolgreich durchgeführtem Build findet ihr im _./build/classes_-Ordner die kompilierten
Klassen sowie unter _./build/libs_ generierte Artefakte. Hier befindet sich auch eine ausführbare JAR-Datei.

**3. Gradle Build anpassen**  
**3.1 Umbenennen des Artefakts**  
Die beim Build generierte JAR-Datei soll einen frei wählbaren Namen erhalten. Legt diesen Namen als _Extra Property_ in
eurem Build Skript fest. Innerhalb der vorhandenen _jar_ Konfiguration kann der Name des Artefakts über das Property
_archiveFileName_ gesetzt werden. Nutzt hierfür das zuvor definierte _Extra Property_.  
**3.2 Kopieren der JAR-Datei**  
Passt den Gradle Build so an, dass die ausführbare JAR-Datei aus dem Ordner _./build/libs_ in einen Ordner eurer
Wahl kopiert wird. Verwendet hierzu den build-in Task Copy (Dokumentation unter: https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html).
Der zu erstellende Task soll vom Typ Copy sein und den _build_ Task als Abhängigkeit besitzen. Die Task Definition könnte somit
wie folgt aussehen:
```groovy
task buildAndCopyJar(type: Copy, dependsOn: build) {
    ...
}
```
Mit `.\gradlew buildAndCopyJar` wird der Task aufgerufen, sodass zunächst alle Schritte einschließlich _build_-Task ausgeführt
werden und anschließend der von euch erstellte Task zum Kopieren des Artefakts.
## Zusatzaufgabe
Erstellt einen eigenen Task mit der Bezeichnung _notify_, der nach erfolgreichem Build automatisch eine entsprechende Zeile
in eine Log-Datei schreibt. Die Zeile könnte etwa wie folgt aussehen: _Build successfull: Sun May 09 13:18:36 CEST 2021_

Macht euch zunutze, dass in einer `build.gradle` auch valides Java statt Groovy verwendet werden kann. Implemtiert daher
im `build.gradle` einfach eine Klasse `Logger` mit einem Kosntruktor, der den Pfad der Datei, in die geschrieben werden soll
entgegen nimmt und damit ein `File` Objekt erstellt. Implementiert in dieser Klasse außerdem eine Methode `public void log(String content)`
in der ihr einen beliebigen Inhalt in eine Zeile der Datei schreibt. Falls ihr nicht mehr direkt parat habt, wie man in Java
in Dateien schreibt, könnt ihr euch an eine der unter dem folgenden Link beschriebenen Optionen orientieren: https://www.baeldung.com/java-write-to-file

Nachdem ihr die Klasse implementiert habt, definiert einen eigenen Task `logBuild`. In diesem sollt ihr eure implementierte
`Logger` Klasse nutzen, um die o.a. Zeile zu loggen. 

Definiert abschließend in der `build.gradle` mit `build.finalizedBy(logBuild)`, dass der _build_ Task stets vom Task _logBuild_
beendet wird. Jedesmal wenn `.\gradlew build` sollte dann eine entsprechende Zeile in euere Log-File geschrieben werden. 

## Hands-On
Gradle wird vor allem als Build-Tool für die Entwicklung von Android Apps verwendet. Android Studio etwa nutzt Gradle zur
Verwaltung des Build-Prozess. Mit Hilfe von Gradle lässt sich der Build-Prozess für Android Apps den eigenen Anforderungen
entsprechend anpassen (siehe: https://developer.android.com/studio/build).Viele Open-Source Projekte für Android werden außerdem
bei GitHub gehostet. Diese lassen sich "from Source" bauen, anpassen oder weiterentwickeln sowie auf dem eigenen Smartphone testweise ausführen.
### Vanilla Music
https://github.com/vanilla-music/vanilla  
Vanilla Music ist ein GPLv3 lizensierter Musikplayer für Android der für eigene Zwecke weiterentwickelt werden kann.

Allerdings gibt es aber wohl einige Derivate, die über den Google Play Store betrieben werden, jedoch gegen die GPL Lizenz verstoßen und
daher in einer [HALL-OF-SHAME](https://github.com/vanilla-music/vanilla/blob/master/HALL-OF-SHAME.md) gelistet werden.

### LeafPic
https://github.com/HoraApps/LeafPic  
LeafPic ist eine einfach gehaltene Gallery App, die unter die GPLv3 Lizenz gestellt ist. Der Scope dieses Projekts liegt darin ein
eine einfache Open Source Gallery ohne Werbung anzubieten. Unter Wahrung der Lizenzbedingungen, können auch hier Weiterentwicklungen
vorgenommen werden.
