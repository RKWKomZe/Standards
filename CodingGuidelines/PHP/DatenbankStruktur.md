# Datenbank-Struktur
## Allgemeines
Generell gilt die Regel, dass wir auf Datenbank-Ebene immer mit STRICT_TRANS_TABLES entwickeln.
Alle Tabellen-Definitionen in den Extensions / Plugins müssen daher mit diesem Standard kompatibel sein.

Das hat folgende Implikationen:
* Datenbankfelder, die eine Objekt-Referenz beinhalten, die auch NULL sein kann, dürfen nicht als NOT NULL definiert sein.
```
CREATE TABLE tx_feregister_domain_model_consent
(

	uid                    int(11) NOT NULL auto_increment,
	pid                    int(11) DEFAULT '0' NOT NULL,

	parent                 int(11) DEFAULT '0',
	child                  int(11) DEFAULT '0',

	frontend_user          int(11) DEFAULT '0',
	opt_in     			   int(11) DEFAULT '0',

	PRIMARY KEY (uid),
	KEY                    parent (pid),
	KEY                    opt_in (opt_in),
);

```
* Datenbankfelder, die als TEXT oder LONGTEXT definiert sind, dürfen nicht als NOT NULL definiert sein
```
CREATE TABLE tx_postmaster_domain_model_queuemail (

	uid int(11) NOT NULL auto_increment,
	pid int(11) DEFAULT '0' NOT NULL,

	body_text text,
	attachment_paths text,

	plaintext_template longtext,
	html_template longtext,
	calendar_template longtext,

	layout_paths text,
	partial_paths text,
	template_paths text,

	PRIMARY KEY (uid),
	KEY parent (pid),
);
```
## STRICT_TRANS_TABLES deaktivieren
Sollte man mit einem Setup arbeiten müssen, dass auch ältere Extensions enthält, kann temporär folgende Einstellung in der my.cnf unter /etc/mysql vorgenommen werden
(ACHTUNG: Variationen bei verschiedenen Versionen von MySQL!)
```
[mysqld]
# Default:  ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
sql_mode = "ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
```