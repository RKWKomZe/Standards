# Commits

## Allgemeines
* Grundsätzlich sollte nach jedem Entwicklungsschritt, der erfolgreich im lokalen System getestet worden ist, ein
Commit in das lokale Repository erfolgen. 
* Dabei ist darauf zu achten, dass jeder Commit einen Kommentar enthält, der klar erkennen lässt, was getan wurde

## Richtig kommentieren
Wie auch im Quellcode selbst, dienen die Kommentare zu einem Commit dazu, die Änderungen am Code besser
nachvollziehen zu können. Daher werden einige Richtlinien zum Verfassen der Commit-Kommentare festgelegt:

* Kommentare werden grundsätzlich auf Englisch verfasst.
* Jeder erledigte Schritt innerhalb eines Commits erhält eine eigene Zeile mit einer kurzen Beschreibung
* Beschreibingen sollen möglichst kurz, aber trotzdem prägnant gehalten werden. Am besten eignet sich eine
stichwortartige Erklärung.
* Jede Kommentarezeile enthält vorangestellt einen Prefix.

## Prefixes
Die Prefixes sollen dazu dienen, die Art der Änderung am Quellcode zu kategorisieren.
Wir verwenden folgende Prefixes (=Kategorien) für die Kommentare in Commits:
* `FIX`: Behebung eines Bugs
* `FEATURE`: Implementeirung eines neuen Features
* `CHANGE`: Veränderung an bestehenden Funktionen, veränderte Texte, usw.; **nur, wenn kein Feature oder Bug**
* `ADD`: Hinzufügen von Dateien
* `REMOVE`: Entfernen von Dateien oder Funktionen
* `UPDATE`: Upate von z.B. Libraries oder Versionen via composer
* `OTHER`: Restkategorie, wenn keine der anderen zutrifft.

Die Prefixes werden in UPPERCASE und in eckigen Klammern vor Kommentarzeilen in Commits gestellt.
Sie werden vom Kommentar mit einem Leerzeichen getrennt.

## Beispiele
```
[UPDATE] new version of rkw/rkw-basics
[FIX] fixed wrong description in introduction-template
[ADD] implemented a CSS file for introduction-template 
```
