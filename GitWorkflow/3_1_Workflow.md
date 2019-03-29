# Workflow innerhalb von Projekt-Repositories
Der Entwicklungszyklus besteht hier im Kern aus drei Schritten:
* Entwicklung & technische Abnahme
* Staging & Abnahme (durch "Kunden")
* Abschluss & Going-Live

**WICHTIG: Änderungen an der `composer.json` und `composer.lock` werden, bei korrekter Konfiguration, über die `.gitattributes` und einen entsprechenden merge-driver NICHT automatisch zwischen den Branches zusammengeführt. Dies bedeutet, dass Updates in diesen Dateien pro Branch manuell getätigt werden müssen.**

## 1. Entwicklung & technische Abnahme
### 1.1 Allgemeine Hinweise
* **Ein Merge in den Development-Branch ist erst erlaubt, wenn eine explizite technische Freigabe vorliegt.** 
* Kleinere Änderungen dürfen auf dem Development-Branch direkt commited werden. Für größere Änderungen ist ein entsprechender Sub-Branch abzuzweigen.
* Der Development-Banch muss immer eine lauffähige Version für die weitere Entwicklung enthalten.
* Sub-Branches werden **ausschließlich** vom Development-Branch abgezweigt und, nach Abschluss, auch **nur** in diesen gemergt. **Ein Merge des Sub-Branches in den Staging-, Production- oder Master- Branch ist NICHT zulässig.**
* Es besteht aber die Möglichkeit, Sub-Branches probeweise auf die Staging-Umgebung zu deployen.

### 1.2 Beispiel-Workflow
#### 1.2.1 Szenario
* Es wird ein neues Feature namens `Function` gestartet und soll nun umgesetzt werden.

#### 1.2.2 Ablauf
1.) Vom Development-Branch wird ein Sub-Branch `feature/function` abgezweigt.
```
development
  ├───────────── feature/function
```

2.) Die Entwickler arbeiten auf dem neuen Sub-Branch und committen regelmäßig ihre Änderungen. Währenddessen wird auch auf dem Development-Branch weiterentwickelt.
```
development
  ├───────────── feature/function
  │                  |
  │                  X commit
  X commit           X commit
```

3.) Die Entwicklung auf dem Sub-Branch ist nun abgeschlossen und getestet.
 **Nach erfolgter Freigabe** werden die Änderungen in den Development-Branch gemergt.

```
development
  ├───────────── feature/function
  │                  |
  │                  X commit
  X commit           X commit
  │                  |
  X merge ───────────┘
```
4.) Der bisherige Sub-Branch ist damit obsolet und kann gelöscht werden. **Er bleibt daher auch bei künftigen Entwicklungen ab jetzt unverändert.**

> **WICHTIG: Auf dem Sub-Branch werden folglich nach dem Merge keine Änderungen mehr vorgenommen!!!** 

## 2 Staging & Abnahme
### 2.1 Allgemeine Hinweise
* **Ein Merge in den Staging-Branch ist erst erlaubt, wenn eine explizite technische und gestalterische Freigabe vorliegt.** 
* Auf dem Staging- Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. 
* Größere Anpassungen erfolgen ausschließlich über den Umweg des Development-Branches mit entsprechenden Sub-Branches.

### 2.2 Beispiel-Workflow
#### 2.2.1 Szenario 
* [Fortsetzung von oben]
* Nachdem nun alle Funktionen des Features `Function` integriert sind, sollen die Änderungen auf der Staging- Umgebung bereitgestellt werden.
* Nach der Bereitstellung zeigt sich aber, dass noch weitere Funktionen benötigt werden.

#### 2.2.2 Ablauf 
1.) Der Development-Branch, welcher den Sub-Branch von oben bereits enthält, wird **nach erfolgter Freigabe** in den Staging-Branch gemergt.
```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  └─────────────────────────────────── X merge

```

2.) Der aktuelle Stand wird auf die Staging-Umgebung deployt. 
Ein kleiner Bug wird vor dem Deployment direkt auf dem Staging-Branch noch behoben. 
```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment
```

3.) Danach wird nun der Staging-Branch in den  Development-Branch zurückgemergt, um diesen wieder auf den neusten Stand zu bringen. 
Hiermit wäre der Staging-Prozess nun eigentlich abgeschlossen. 

```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment
                                       │
  X merge ─────────────────────────────┘
```

4.) Nun erfolgt die Abnahme durch den "Kunden". Hier fallen nun aber weitere Funktionswünsche auf, die vor dem Going-Live implementiert werden sollen.
Wie sich zeigt, handelt es sich um größere Änderungswünsche, **daher beginnt nun ein weiterer Entwicklungszyklus**.

In diesem Kontext wird ein neuer Sub-Branch `feature/functionTwo` vom Development-Branch abgezweigt. Damit beginnt der Ablauf im Kern wie oben beschrieben erneut.

**Der bisherige Sub-Branch `feature/function` bleibt dabei stets unverändert. Er ist obsolet und kann gelöscht werden.**
```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment
                                       │
  X merge ─────────────────────────────┤
  │                                    ┆  
  
   [BEGINN ZWEITE ENTWICKLUNGSSCHLEIFE] 
 
  │                                     
  ├─────────── feature/functionTwo     ┆ 
  │                  │                  
  │                  X commit          ┆
  X commit           X commit           
  |                  │                 ┆
  X merge ───────────┘                  
  │                                    ┆
  │                                       
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment           
```

## 3  Abschluss / Going-Live
### 3.1 Allgemeine Hinweise
* **Ein Merge in den Production-Branch ist erst erlaubt, wenn eine explizite technische, gestalterische und inhaltliche Freigabe vorliegt.** 
* Auf dem Production- Branch erfolgen in der Regel nur kleinere Änderungen als direkte Commits. 
* Größere Anpassungen erfolgen hier schon per Definition nicht mehr bzw. würden dafür sorgen, dass der gesamte Entwicklungszyklus von vorne beginnt.
* Auf dem Master- Branch erfolgen **NIEMALS** direkte Commits. 
* Werden dort kleinere Änderungen nötig, müssen diese über den Production-Branch eingespielt werden - inkl. eines neuen Tags.

### 3.2 Beispiel-Workflow
#### 3.2.1 Szenario 
* [Fortsetzung von oben]
* Nachdem nun alle Funktionen der beiden Features `Function` und `FunctionTwo `integriert sind, sollen die Änderungen nun live gehen.

#### 3.2.2 Ablauf 
1.) **Nach erfolgter Freigabe** wird der Staging-Branch, welcher alle Features von oben bereits enthält,  in den Production-Branch gemergt. 
 Ein kleiner Bug wird vor dem Deployment direkt auf dem Production-Branch behoben. Anschließend wird der Branch deployed.
```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment
                                       │
  X merge ─────────────────────────────┤
  │                                    ┆  
  │                                     
  ├─────────── feature/functionTwo     ┆ 
  │                  │                  
  │                  X commit          ┆
  X commit           X commit           
  |                  │                 ┆
  X merge ───────────┘                  
  │                                    ┆
  │                                       
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │                              ┆
  ┆                                    X deployment              production          
                                       │                              │
  ┆                                    ├───────────────────────────── X merge
                                                                      │
  ┆                                    ┆                              X commit
                                                                      │
  ┆                                    ┆                              X deployment
```

2.)  Damit der Staging- und Development-Branch wieder auf dem neusten Stand sind, wird der Production-Branch dort hinein gemergt.

```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment
                                       │
  X merge ─────────────────────────────┤
  │                                    ┆  
  │                                     
  ├─────────── feature/functionTwo     ┆ 
  │                  │                  
  │                  X commit          ┆
  X commit           X commit           
  |                  │                 ┆
  X merge ───────────┘                  
  │                                    ┆
  │                                       
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │                              ┆
  ┆                                    X deployment              production          
                                       │                              │
  ┆                                    ├───────────────────────────── X merge
                                                                      │
  ┆                                    ┆                              X commit
                                                                      │
  ┆                                    ┆                              X deployment
                                                                      │
  ┆                                    X merge ───────────────────────┤
                                                                      │      
  X merge ────────────────────────────────────────────────────────────┘
```

3.) Zuletzt wird der Production-Branch in den Master-Branch gemergt. Dort wird ein entsprechender Tag gesetzt. Damit ist der Going-Live-Prozess abgeschlossen.
 
```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                 staging
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │
  ┆                                    X deployment
                                       │
  X merge ─────────────────────────────┤
  │                                    ┆  
  │                                     
  ├─────────── feature/functionTwo     ┆ 
  │                  │                  
  │                  X commit          ┆
  X commit           X commit           
  |                  │                 ┆
  X merge ───────────┘                  
  │                                    ┆
  │                                       
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X commit
                                       │                              ┆
  ┆                                    X deployment              production          
                                       │                              │
  ┆                                    ├───────────────────────────── X merge
                                                                      │
  ┆                                    ┆                              X commit
                                                                      │
  ┆                                    ┆                              X deployment
                                                                      │
  ┆                                    X merge ───────────────────────┤
                                                                      │                       ┆
  X merge ────────────────────────────────────────────────────────────┤                     master
                                                                      │                       │          
                                                                      └────────────────────── X merge
                                                                                              │
                                                                                              X tag
```

---
[Weiterlesen: Workflow innerhalb von Sub-Packages / Extensions](3_2_Workflow.md)
