# Workflow
Im Nachfolgenden wird der GIT-Workflow inklusive des Setzen von Tags in Projekten (Haupt-Package) und Sub-Packages beschrieben.

## 1. Tags
### 1.1 Allgemeines
Tags werden prinzipiell nach dem folgenden Muster gesetzt:
```
v[MAJOR].[MINOR].[PATCH][STABILITY]
```

* **Hauptversionsnummer (major release)**: 
Die Hauptversionsnummer zeigt meist äußerst signifikante Änderungen an (z.B. Relaunch). 
Ein Sprung in der Hauptversionsnummer folgt daher sehr selten.

* **Nebenversionsnummer (minor release)**:
Die Nebenversionsnummer bezeichnet die funktionale Erweiterung (z.B. Hinzufügen einer neuen Extension).
 
* **Revisionsnummer (patch level)**: 
Die Revisionsnummer bezeichnet im Wesentlichen Bugfixes oder kleinere Änderungen.

### 1.2 Tags auf den Haupt-Branches in den Projekt-Repositories
Development-, Staging- und Production-Branch bezeichnen wir als Haupt-Branches.
Die drei Haupt-Branches sind zudem hierarchisch zu sehen und zwar in dieser Reihenfolge:
1. Development
2. Staging
3. Production.

Es gelten folgende Basis-Regeln:
* Tags werden nur auf den Haupt-brances gesetzt
* In Project-Branches werden KEINE Tags gesetzt
* Die Versionsnummern orientieren sich dabei individuell am jeweiligen Projekt (und nicht etwa an der TYPO3-Version)


#### 1.2.1 Commits in den Hauptbranches
Jeder Commit innerhalb eines Haupt-Brances erhöht die Revisionsnummer um eins.

#### 1.2.2 Merges zwischen den Hauptbranches
Hier werden Tags nur dann gesetzt, wenn es sich um einen Merge von unten nach oben (Bottom-Up-Merge) handelt.
Ein Merge des Development-Branches in den Staging-Branch hätte daher einen Tag zur Folge, NICHT aber ein Merge 
des Staging-Brances in den Development-Branch.

Bei jedem Bottom-Up-Merge wird die Nebenversionsnummer um eins erhöht.

#### 1.2.3 Stability-Angaben
Für jeden Branch ist nur eine bestimmte Angabe für [STABILITY] zulässig.
Dadurch ist sichergestellt, dass das jeweilige Package auch über Composer unter Wahrung der Stabilitätskriterien 
referenziert werden kann. 
* Development-Branch: nur `alpha`
* Staging-Branch: nur `beta`
* Production-Branch: nur `stable`



## 2. Workflow innerhalb eines Projekt-Repositories
###  2.1 Vom Projectbranch zum Development-Branch
#### 2.1.1 Allgemeine Hinweise
* Kleinere Änderungen dürfen auf diesem Branch direkt commited werden. Für größere Änderungen ist ein entsprechender Project-Branch abzuzweigen.
* Ein Project-Branch dient dazu, ein komplexeres oder kritisches Unterprojekt (ggf. mit einem externen Dienstleister) abzuwickeln. Dabei handelt es sich per se um Entwicklungen im Dev- oder Alpha-Status, die auch gravierende Fehler enthalten können.
* Der Development-Banch hingegen muss immer eine lauffähige Version für die weitere Entwicklung enthalten.
* Project-Branches werden **ausschließlich** vom Development-Branch abgezweigt und, nach Abschluss, auch **nur** in diesen gemergt. **Ein Merge des Project-Branches in den Staging- oder Production-Branch ist NICHT zulässig.**
* Es besteht aber die Möglichkeit, Project-Branches probeweise auf die Staging-Umgebung zu deployen.

#### 2.1.2 Beispiel-Workflow, Teil 1
##### 2.1.2.1 Szenario
* Es wird ein neues Projekt namens `Test` gestartet und soll nun umgesetzt werden.

##### 2.1.2.2 Ablauf
1.) Vom Development-Branch wird ein Project-Branch `project/test` abgezweigt.
```
development
  ├───────────── project/test
  ```

2.) Die Entwickler arbeiten auf dem neuen Project-Branch und committen regelmäßig ihre Änderungen. Währenddessen wird auch auf dem Development-Branch weiterentwickelt.
```
development
  ├───────────── project/test
  |                  |
  |                  X commit
  X commit & tag     X commit
```

3.) Die Entwicklung auf dem Project-Branch ist nun abgeschlossen und getestet.
 Nach erfolgter **interner Qualitätssicherung** werden die Änderungen in den Development-Branch gemergt.
 Es wird ein entsprechender Tag gesetzt. 
```
development
  ├───────────── project/test
  |                  |
  |                  X commit
  X commit & tag     X commit
  |                  |
  X merge & tag ─────┘
  |
```
4.) Da der Project-Branch jetzt obsolet ist, kann er gelöscht werden.

### 2.2 Vom Development-Branch auf den Staging-Branch
#### 2.2.1 Allgemeine Hinweise
* Auf dem Staging- Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. 
* Größere Anpassungen erfolgen ausschließlich über den Umweg eines entsprechenden Project-Branches.

#### 2.2.2 Beispiel-Workflow, Teil 2, Version 2
##### 2.2.2.1 Szenario
* Nachdem nun alle Funktionen des Projektes `Test` integriert sind, sollen die Änderungen auf der Staging- Umgebung bereitgestellt werden.
* Nach der Bereitstellung zeigt sich aber, dass noch weitere Funktionen benötigt werden.

##### 2.2.2.2 Ablauf 
1.) Der Development-Branch wird in den Staging-Branch gemergt. Dabei wird ein entsprechender Tag gesetzt
```
staging            development
  |                   |
  X merge & tag───────┘
```

2.) Der aktuelle Stand wird auf die Staging-Umgebung deployt. Ein kleiner Bug wird vor dem Deployment noch behoben. 
Nach dem Deployment fallen aber weitere Funktionswünsche auf, die vor dem Going-Live implementiert werden sollen.
```
staging            development
  |                   |
  X merge & tag───────┘
  |
  X commit & tag
  |
  X deployment
```

3.) Wie sich zeigt, handelt es sich um größere Änderungswünsche. Hierfür wird nun der Staging-Branch in den 
Development-Branch zurückgemergt, um diesen auf den neusten Stand zu bringen. Anschließend wird ein neuer Proj
ect-Branch vom Development-Branch abgezweigt und wie oben beschrieben weiterverfahren.
```
staging            development
  |                   |
  X merge & tag───────┘
  |                   ┆
  X commit & tag      
  |                   
  X deployment        
  |                   ┆
  ├───────────────────X merge & tag
```
```
