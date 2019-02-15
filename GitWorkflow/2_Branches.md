# Branches
## Branches in Projekten
Die Projekte selbst sollten sehr "schlank" gehalten sein.

Dies bedeutet: **Es sollte angestrebt werden, so viel Code wie möglich in eigenständige Packages auszulagern, die dann über das Projekt-Package eingebunden werden.**

Wir unterscheiden in den Projekten drei zentrale Branches:
### 1. master
Der Master-Branch ist der Haupt-Branch, von dem alle Branches abgezweigt werden.

Liegt einer der nachfolgenden genannten Branches noch nicht im Remote-Repository wird dieser vom Master-Branch abgezweigt.

>**ACHTUNG: Auf dem Master-Branch erfolgen in der Regel keine direkten Commits**

### 2. development 
Der Development-Branch fungiert als Basis für die lokalen Entwicklungsumgebungen. 
Alles was hier commited wird, muss für alle lokalen Entwicklungsumgebungen funktionieren, d.h. **es handelt sich hier zwingend um eine lauffähige Version.** 

> **ACHTUNG: Kleinere Änderungen dürfen auf diesem Branch direkt commited werden. 
Für größere Änderungen ist ein entsprechender Project-Branch abzuzweigen.**
>
> [Siehe auch: Workflow](3_Workflow.md)

 #### 2.1 project/[WHAT_SO_EVER]
Die meisten der größeren Entwicklungen dürften im Regelfall in den Repositories der Sub-Packages stattfinden.
Darüber hinaus besteht aber auch die Möglichkeit, für Unter-Projekte einen eigenständigen Branch abzuzweigen.

Gründe, die einen Project-Branch rechtfertigen sind u.a.:
* Es handelt sich um eine Weiterentwicklung mit Breaking-Changes
* Die geplanten Änderungen werden über einen längeren Zeitraum umgesetzt, währenddessen sollen aber andere Weiterentwicklungen oder Bugfixes basierend auf dem bisherigen Stand möglich bleiben.
* Sub-Packages mit Alpha-Status sollen getestet werden, ohne weitere Entwicklungen zu behindern
* Es werden mehrere Sub-Packages gleichzeitig modifiziert
* ...

Ein Project-Branch dient dazu, ein komplexeres oder kritisches Unterprojekt (ggf. mit einem externen Dienstleister) abzuwickeln. 
Dabei handelt es sich per se um Entwicklungen im Dev- oder Alpha-Status, die auch gravierende Fehler enthalten können.

Die Benennung der Branches erfolgt nach folgendem Muster: 
```
project/[WHAT_SO_EVER]
```
Wobei `[WHAT_SO_EVER]` durch einen sinnvollen und selbsterklärenden Projektnamen zu ersetzten ist. Die Projektnamen sind immer in lowerCamelCase zu halten.
**Auf Bezüge zu Ticketnummern wird im Namen verzichtet.**

**Beispiel:**
```
project/refactoringOrders
```

> **ACHTUNG: Project-Branches werden ausschließlich vom Development-Branch abgezweigt und, nach Abschluss, auch nur in diesen gemergt.
Ein Merge in den Staging- oder Production-Branch ist NICHT zulässig. Es besteht aber die Möglichkeit, Project-Branches probeweise auf die Staging-Umgebung zu deployen.**
>
> [Siehe auch: Workflow](3_Workflow.md)


### 3. staging
Der Staging-Branch wird für das Deployment auf eine entsprechende Staging-Umgebung verwendet und spiegelt immer den letzten Stand auf der Staging-Umgebung wider.

> **ACHTUNG: Auf diesem Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. Größere Anpassungen erfolgen ausschließlich über den Umweg eines entsprechenden Project-Branches.**
>
> [Siehe auch: Workflow](3_Workflow.md)

### 4. production
Der Production-Branch wird für das Deployment auf eine entsprechende Production-Umgebung verwendet und spiegelt immer den letzten Stand auf der Live-Umgebung wider.

>**ACHTUNG: Auf diesem Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. Größere Anpassungen erfolgen ausschließlich über den Umweg eines entsprechenden Project-Branches.**
>
> [Siehe auch: Workflow](3_Workflow.md)



## Branches in Sub-Packages
Wir unterscheiden hier zwei zentrale Branches:

### 1. master
Der Master-Branch ist der Haupt-Branch. Er enthält den letzten Stand der letzten `stable`-Version des Packages.

> **ACHTUNG: Auf dem Master-Branch erfolgen im Regelfall keine direkten Commits.**

**Hotfixes stellen eine Ausnahme von dieser Regel dar.** Wichtige Fixes, welche die Stabilität des Packages gefährden oder ein Sicherheitsrisiko darstellen, können direkt auf dem Master-Branch durchgeführt und commited.
* [Siehe auch: Workflow](3_Workflow.md)

### 2. development
**Der Development-Branch ist die Basis für alle Weiterentwicklungen des Packages.** 
Er enthält **IMMER** einen lauffähigen Stand des Packages, der aber nicht zwingend eine `stable`-Version sein muss.

> **ACHTUNG: Für jede größere Weiterentwicklung des Packages wird ein eigenständiger Entwicklungsbranch vom Development-Branch abgezweigt.
Dieser Zweig wird dann so lange fortgeführt, bis die Entwicklung an diesem Zweig abgeschlossen ist.**
>
> [Siehe auch: Workflow](3_Workflow.md)


Die vom Development-Branch abgeweigten Branches werden nach zwei Typen unterschieden:

#### 2.1 feature/[WHAT_SO_EVER]
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

#### 2.1 bugfix/[WHAT_SO_EVER]
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