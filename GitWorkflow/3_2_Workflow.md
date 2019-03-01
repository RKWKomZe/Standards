# Workflow innerhalb von Sub-Packages / Extensions
Da es hier nur zwei Branches gibt, ist der Workflow vergleichsweise simpel.

Der Entwicklungszyklus besteht hier im Kern aus zwei Schritten:
* Entwicklung & Abnahme
* Abschluss

## 1 Entwicklung
Der Entwicklungsworkflow entspricht im Kerm dem Entwicklungsworkflow für das Projekt-Repository.

> [Siehe auch: Workflow innerhalb von Projekt-Repositories](3_1_Workflow.md)

Neben der technischen Abnahme erfolgt hier allerdings auch die Abnahme durch den "Kunden", bevor ein Merge in den Development-Branch erfolgt.

## 2 Abschluss
### 2.1 Allgemeine Hinweise
* Auf dem Master- Branch erfolgen **KEINE** direkten Commits. 
* Werden Änderungen nötig, müssen diese über den Development-Branch eingespielt werden, d.h. der Entwicklungszyklus beginnt zwangsweise erneut.
* Größere Anpassungen erfolgen auch dort wiederum ausschließlich über den Umweg entsprechender Sub-Branches.

### 2.2 Beispiel-Workflow
#### 2.2.1 Szenario 
* [Fortsetzung von oben]
* Nachdem nun alle Funktionen des Features `Function` integriert sind, sollen die Änderungen nun fester Bestandteil des Subpackages / der Extension werden.

#### 2.2.2 Ablauf 
1.) Der Development-Branch, welcher den Sub-Branch von oben bereits enthält, wird in den Master-Branch gemergt.
Darüber hinaus wird ein entsprechender Tag auf dem Master-Branch gesetzt.  

**Wichtig: Bei Extensions muss immer auch die ext_emconf.php entsprechend angepasst werden!**
> [Siehe auch: Tags](4_Tags.md)


```
development
  ├─────────── feature/function
  │                  │
  │                  X commit
  X commit           X commit
  |                  │
  X merge ───────────┘
  |                                    ┆
  │                                  master
  │                                    │  
  ├─────────────────────────────────── X merge
                                       │
  ┆                                    X tag
 
```

2.) Hiermit ist der Prozess bereits abgeschlossen. Der entsprechende Feature-Branch ist nun obsolet und kann gelöscht werden.

**Werden weitere Features oder Bugfixes an dem gerade integrierten Feature benötigt, so startet damit der Entwicklungsyzyklus erneut. Dies bedeutet auch, dass ggf. neue Sub-Branches angelegt werden müssen.**


---
[Weiterlesen: Tags](4_Tags.md)
