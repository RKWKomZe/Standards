# Branches

## 1. Branches in Projekten
Die Projekte selbst sollten sehr "schlank" gehalten sein.

Dies bedeutet: **Es sollte angestrebt werden, so viel Code wie möglich in eigenständige Packages auszulagern, die dann über das Projekt-Package eingebunden werden.**

Wir unterscheiden in den Projekten drei zentrale Branches:
```
master    production     staging     development
  |           |             |             |
  |           |             |             ├─ ggf. feature/[WHAT_SO_EVER]
  |           |             |             ├─ ggf. bugfix/[WHAT_SO_EVER]
  |           |             |             ├─ [...]
```


### 1.1 master
Der Master-Branch ist der Haupt-Branch, von dem der Development-, Staging- und Production- Branch initial abgezweigt werden.

Liegt einer der nachfolgenden genannten Branches noch nicht im Remote-Repository wird dieser vom Master-Branch abgezweigt.

>**ACHTUNG: Auf dem Master-Branch erfolgen in der Regel keine direkten Commits**


### 1.2. production
Der Production-Branch wird für das Deployment auf eine entsprechende Production-Umgebung verwendet und spiegelt immer den letzten Stand auf der Live-Umgebung wider.

>**ACHTUNG: Auf diesem Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. Größere Anpassungen erfolgen ausschließlich über den Umweg des Development-Branches.**
>
> [Siehe auch: Workflow](3_Workflow.md)


### 1.3. staging
Der Staging-Branch wird für das Deployment auf eine entsprechende Staging-Umgebung verwendet und spiegelt immer den letzten Stand auf der Staging-Umgebung wider.

> **ACHTUNG: Auf diesem Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. Größere Anpassungen erfolgen ausschließlich über den Umweg des Development-Branches.**
>
> [Siehe auch: Workflow](3_Workflow.md)


### 1.4. development 
Der Development-Branch fungiert als Basis für die lokalen Entwicklungsumgebungen. 
Alles was hier commited wird, muss für alle lokalen Entwicklungsumgebungen funktionieren, d.h. **es handelt sich hier zwingend um eine lauffähige Version.** 

> **ACHTUNG: Kleinere Änderungen dürfen auf diesem Branch direkt commited werden. 
Für größere Änderungen ist ein entsprechender Sub-Branch abzuzweigen.**
>
> [Siehe auch: Workflow](3_Workflow.md)


## 2. Branches in Sub-Packages / Extensions
Wir unterscheiden hier zwei zentrale Branches:

### 2.1. master
Der Master-Branch ist der Haupt-Branch. Er enthält den letzten Stand der letzten `stable`-Version des Packages.

> **ACHTUNG: Auf dem Master-Branch erfolgen im Regelfall keine direkten Commits.**

**Hotfixes stellen eine Ausnahme von dieser Regel dar.** Wichtige Fixes, welche die Stabilität des Packages gefährden oder ein Sicherheitsrisiko darstellen, können direkt auf dem Master-Branch durchgeführt und commited.
* [Siehe auch: Workflow](3_Workflow.md)

### 2.2. development
**Der Development-Branch ist die Basis für alle Weiterentwicklungen des Packages.** 
Er enthält **IMMER** einen lauffähigen Stand des Packages, der aber nicht zwingend eine stabile Version sein muss.

> **ACHTUNG: Für jede größere Weiterentwicklung des Packages wird ein eigenständiger Sub-Branch vom Development-Branch abgezweigt.**
>
> [Siehe auch: Workflow](3_Workflow.md)


## 3. Sub-Branches
Feature- und Bugfix-Branches werden **AUSSCHLIEßLICH** vom Development-Branch abgezweigt und auch nur in diesen zurück-gemergt:
Ein Sub-Branch dient dazu, ein komplexeres oder kritisches Unterprojekt (ggf. auch mit einem externen Dienstleister) abzuwickeln. 


**Sub-Branches haben eine begrenzte Lebensdauer: Sie werden obsolet, sobald sie in den Development-Branch zurück-gemerkt werden und können dann gelöscht werden.
Zur Sicherheit gilt, dass Sub-Branches auch nachdem sie zurückgemergt wurden, noch einen Monat aufbewahrt werden. Danach werden sie aus dem Repository entfernt.**

Dabei unterscheiden wir zwei Typen:

#### 3.1 feature/[WHAT_SO_EVER]
Feature-Branches beinhalten eine Weiterentwicklung bzw. einen Change-Request.

Die Benennung der Branches erfolgt nach folgendem Muster: 
```
feature/[WHAT_SO_EVER]
```
Wobei `[WHAT_SO_EVER]` durch einen sinnvollen und selbsterklärenden Projektnamen zu ersetzten ist. Die Projektnamen sind immer in lowerCamelCase zu halten.
**Auf Bezüge zu Ticketnummern wird im Namen verzichtet.**

**Beispiel:**
```
feature/pageRightsOnIssueGeneration
```

#### 3.2 bugfix/[WHAT_SO_EVER]
Bugfix-Branches beheben ein bestehendes Problem.

Die Benennung der Branches erfolgt nach folgendem Muster: 
```
bugfix/[WHAT_SO_EVER]
```
Wobei `[WHAT_SO_EVER]` durch einen sinnvollen und selbsterklärenden Projektnamen zu ersetzten ist. Die Projektnamen sind immer in lowerCamelCase zu halten.
**Auf Bezüge zu Ticketnummern wird im Namen verzichtet.**

**Beispiel:**
```
bugfix/wrongPageRightsOnIssueGeneration
```

---
[Weiterlesen: Workflow](3_Workflow.md)