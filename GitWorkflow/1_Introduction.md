# Einführung
Wir setzen ausschließlich GIT für die Versionierung ein.

Alle Projekte werden mit Hilfe von Composer zusammengestellt und verwaltet.
Jedes Projekt enthält daher eine `composer.json` und `composer.lock`, die entsprechende Packages referenziert.

## Repositories
* Alle projektbezogenen Repositories werden im internen Repository verwaltet. Hierfür ist die Bereitstellung eines entsprechendes Zugangs notwendig.
* Extensions (Sub-Packages), die unter OpenSource- Lizenz für uns erstellt worden sind, werden öffentlich zugänglich auf [GitHub](https://github.com/RKWKomZe/) zur Verfügung gestellt und mittels Packagist bereitgestellt.


## Deployment

Das Deployment für TYPO3-Projekte erfolgt in der Regel über die Extension `typo3/surf`. 
Das entsprechende Deployment-Script ist im Projekt-Repository enthalten. 
Entsprechende Credential-Files werden bei Bedarf bereitgestellt.

---
[Weiterlesen: Branches](2_Branches.md)