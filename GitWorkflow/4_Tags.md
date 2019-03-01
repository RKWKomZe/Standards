## Tags
Tags werden prinzipiell nach dem folgenden Muster gesetzt:
```
v[MAJOR].[MINOR].[PATCH]-[STABILITY]
```
Generell gilt, dass Tags **AUSCHLIEßLICH** auf dem Master-Branch gesetzt werden.
Der Master-Branch dient somit der Sammlung funktionsfähiger Versionen.


### 1. Versionsnummern
* **Hauptversionsnummer (major release)**: 
Die Hauptversionsnummer zeigt meist äußerst signifikante Änderungen an (z.B. Relaunch). 
Ein Sprung in der Hauptversionsnummer folgt daher sehr selten.

* **Nebenversionsnummer (minor release)**:
Die Nebenversionsnummer bezeichnet die funktionale Erweiterung (z.B. Hinzufügen einer neuen Extension).
 
* **Revisionsnummer (patch level)**: 
Die Revisionsnummer bezeichnet im Wesentlichen Bugfixes oder kleinere Änderungen.

### 2. Stability-Angaben
Jeder Tag wird durch eine Stability-Angabe ergänzt. 
Wir verwenden folgende Stability-Angaben:
* `alpha` = Code ist noch in der Entwicklungsphase und läuft unter Umständen noch nicht fehlerfrei
* `beta` = Code ist bereits getestet und lauffähig. Einzelne Fehler sind aber nicht auszuschließen
* `stable` = Code hat umfangreiche Tests durchlaufen und läuft stabil. 

### 3. Besonderheiten

#### 3.1 Im Projekt-Repository
Für die Versionsnummern gelten hier folgende Basis-Regeln:
* Tags werden nur auf dem Master-Branch gesetzt
* In Project-Branches werden folglich KEINE Tags gesetzt
* Die Versionsnummern orientieren sich dabei individuell am jeweiligen Projekt (und nicht etwa an der TYPO3-Version)


#### 3.2 In Sub-Packages / Extensions
Für die Versionsnummern gelten folgende Basis-Regeln:
* Tags werden nur auf dem Master-Branch gesetzt
* In Project-Branches werden folglich KEINE Tags gesetzt
* Die Versionsnummern orientieren sich dabei hinsichtlich der Haupt- und Nebenversionsnummer **IMMER** an der jewweiligen TYPO3 LTS- Version, für die die jeweilige Extension lauffähig ist. Dadurch ist immer erkennbar, für welche TYPO3-Version eine Extension vorgesehen ist.
* Die Revisionsnummer wird dann individuell hochgezählt

Beispiel:
```
v7.6.X für TYPO3 7.6 LTS
v8.7.X für TYPO3 8.7 LTS
```

**WICHTIG:** Bei der Entwicklung mit TYPO3 ist wesentlich, dass der jeweils gesetzte Tag unbedingt mit den korrespondierenden Angaben in der `ext_emconf.php` übereinstimmt.

Beispiel:
`ext_emconf.php` für Tag `v8.7.2-beta`
```
$EM_CONF[$_EXTKEY] = array(
   	[...]
   	'state' => 'beta',
    [...]
   	'version' => '8.7.2',
    [...]
   );
```

---
[Weiterlesen: Commits](5_Commits.md)
