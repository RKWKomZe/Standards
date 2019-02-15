# Branches
## Branches in Projekten
Wir unterscheiden hier vier zentrale Branches:
### 1. Master
Der Master-Branch ist der Haupt-Branch, von dem alle Branches abgezweigt werden.

Liegt einer der nachfolgenden genannten Branches noch nicht im Remote-Repository wird dieser vom Master-Branch abgezweigt.

### 2. Development 
Der Development-Branch fungiert als Basis für die lokalen Entwicklungsumgebungen. 
Alles was hier commited wird, muss für alle lokalen Entwicklungsumgebungen funktionieren.

### 3. Staging
Der Staging-Branch wird für das Deployment auf eine entsprechende Staging-Umgebung verwendet und spiegelt immer den letzten Stand auf der Staging-Umgebung wider.

### 4. Production
Der Production-Branch wird für das Deployment auf eine entsprechende Production-Umgebung verwendet und spiegelt immer den letzten Stand auf der Live-Umgebung wider.

### Weitere Branches
Die meisten der Entwicklungen dürften im Regelfall in den Repositories der Sub-Packages stattfinden.
Darüber hinaus besteht aber auch im Projekt-Branch die Möglichkeit vom Development-Branch entsprechende Feature- oderBugfix- Branches abzuweigen.
Näheres hierzu siehe unten.


## Branches in Sub-Packages
Wir unterscheiden hier zwei zentrale Branches:

### 1. Master
