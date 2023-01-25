# Allgemeine Regeln

Es gelten die Coding Guidelines des jeweiligen Frameworks. 
Ferner gelten die weiterführenden Hinweise dieser Dokumentation

## Dateistruktur
Folgende Dateistruktur wird festgelegt:
1. Öffnender PHP-Tag (einschließlich strict_types-Deklaration)
2. Copyright-Hinweis
3. Namespace
4. Namespace-Importe
5. Klasseninformationsblock im phpDoc Format
6. PHP-Klasse
7. Optionaler Code
8. KEIN schließender PHP-Tag

## Lesbarkeit & Kommentare
Damit der Code gut lesbar ist, gehören einerseits Kommentare dazu, andererseits aber auch eine sinnvolle Gruppierung der Codezeilen durch Zeilenumbrüche und Einrückungen.
Ein Zuviel an Zeilenumbrüchen kann wiederum aber auch die Lesbarkeit des Codes erheblich beeinträchtigen.

Es gelten folgende Regeln:
* Keine Zeile wird länger als 130 Zeichen
* Kommentarzeilen sind maximal 80 Zeichen lang
* Einrückungen (mit 4 Leerzeichen)
* Kommentierung von wichtigen Passagen/Blöcken des Codes
* Kommentare werden mit ``//`` eingeleitet (kein ``#``!)
* Zwei Leerzeilen zwischen Methoden und zwischen property-Definitionen
* Codeblöcke werden innerhalb einer Methode sinnvoll durch Leerzeilen gruppiert
* Eine Leerzeile nach jeder Codezeile ist KEINE sinnvolle Gruppierung :-)

### Negatives Beispiel
```
public function findExpiredByFormIdentifier(string $identifier = ''): QueryResultInterface
{
    $query = $this->createQuery();
    
    $query->getQuerySettings()->setRespectStoragePage(false);

    $constraints = [];
    
    $constraints[] = $query->equals('identifier', $identifier);
    
    $constraints[] = $query->lessThanOrEqual('validUntil', time());

    $query->matching($query->logicalAnd($constraints));

    return $query->execute();
}
```
* Schwer lesbar
* Zusammenhänge erschließen sich nicht auf den ersten Blick
* Methode wird unnötig aufgebläht 

### Positives Beispiel
```
public function findExpiredByFormIdentifier(string $identifier = ''): QueryResultInterface
{
    $query = $this->createQuery();
    $query->getQuerySettings()->setRespectStoragePage(false);

    $constraints = [];
    $constraints[] = $query->equals('identifier', $identifier);
    $constraints[] = $query->lessThanOrEqual('validUntil', time());

    $query->matching($query->logicalAnd($constraints));
    return $query->execute();
}
```
* Leicht lesbar
* Code-Blöcke und Struktur klar als solche erkennbar


## Leerzeichen
Leerzeichen müssen hinzugefügt werden:
* Auf beiden Seiten von Zeichenketten, arithmetischen Operatoren, Zuweisungsoperatoren und anderen ähnlichen Operatoren (z. B. ``.``, ``=``, ``+``, ``-``, ``?``, ``:``, ``*``, usw.).
* Nach Kommas
* In einzeiligen Kommentaren nach dem Kommentarzeichen (doppelter Schrägstrich).
* Nach Sternchen in mehrzeiligen Kommentaren.
* Nach bedingten Schlüsselwörtern wie ``if (`` und ``switch (``.
* Vor bedingten Schlüsselwörtern, wenn das Schlüsselwort nicht das erste Zeichen ist, wie ``} elseif {``.

Leerzeichen dürfen nicht vorhanden sein:
* Nach einer öffnenden Klammer und vor einer schließenden Klammer. Zum Beispiel: ``explode( 'blah', 'someblah' )`` muss als ``explode('blah', 'someblah')`` geschrieben werden.


## Geschweifte Klammern
Die Verwendung von öffnenden und schließenden geschweiften Klammern ist in allen Fällen obligatorisch, 
in denen sie gemäß der PHP-Syntax verwendet werden können (außer bei case-Anweisungen).

Die öffnende geschweifte Klammer steht immer in der gleichen Zeile wie die vorangehende Konstruktion. 
Vor der öffnenden geschweiften Klammer muss ein Leerzeichen (nicht ein Tabulator!) stehen. 
Eine Ausnahme sind Klassen und Funktionen: Hier steht die öffnende geschweifte Klammer auf einer neuen 
Zeile mit der gleichen Einrückung wie die Zeile mit dem Klassen- oder Funktionsnamen. 
Auf die öffnende geschweifte Klammer folgt immer eine neue Zeile.

Die schließende geschweifte Klammer muss auf einer neuen Zeile beginnen und auf der gleichen Ebene wie 
das Konstrukt mit der öffnenden Klammer eingerückt sein.

### Negatives Beispiel
```
protected function getForm() {
    if ($this->extendedForm) { // generate extended form here
    } else {
        // generate simple form here
    }
}
```

### Positives Beispiel
```
protected function getForm()
{
    if ($this->extendedForm) {
        // generate extended form here
    } else {
        // generate simple form here
    }
}
```

## Type Declarations
Zu einem sauberen und gut testbaren PHP-Code gehören ordentlich angegebene Typen-Deklarationen - und zwar nicht nur für die Parameter, sondern auch für die Rückgabewerte einer Funktion.
Dies ist nicht zuletzt deshalb wichtig, weil klar gesetzte Typen-Deklarationen schon während der Entwicklungsphase dazu beitragen, falsche Übergabewerte oder falsch erwartete Rückgabewerte zu erkennen.
Es gilt:
* Abkürzung der Namespace-Pfade zu den Objekten ist möglich, solange der vollständige Pfad in den PHPDocs steht
* Klare Definition des Typs
* Vermeidung des ``mixed``-Types
* Der ``integer``-Typ wird immer mit ``int`` abgekürzt
* Der ``boolean``-Typ wird immer mit ``bool`` abgekürzt
* Für eine Erleichterung des Test-Driven-Developments: Vermeidung von Methoden ohne Rückgabewerte (``void``)
* Bei Methoden ohne Rückgabewert ist Angabe von ``void`` als Rückgabewert verpflichtend
* Angabe des Types immer auch für properties
```
protected float $longitude = 0.0;
```
* Setzen von korrekten Standardwerten für properties, die ihrer Typ-Deklaration entsprechen
* properties, die Objekte beinhalten und standardmäßig ``null`` sind, erhalten ebenfalls einen Standardwert, weil sonst die Prüfung scheitert (default ist  ``undefined``). Dabei kann auch hier das Fragezeichen helfen. Im Beispiel: FrontendUser oder null. Standardwert ist null.
```
public ?FrontendUser $frontendUser = null;
```NICHT mit Fragezeichen und null instanziert werden. Hier genügt:
```
public FrontendUser $frontendUser;
```
### Negatives Beispiel
```
/**
 * Sends an email to a Frontend-User
 *
 * @param object formRequest
 * @return mixed 
 * @throws \RKW\RkwMailer\Exception
 */
public function userMail($formRequest)
```
* Keine Type Declaration für den Parameter
* Rückgabewert bleibt unklar und ist nur über das Lesen der Methode bzw. der PHPDocs erschließbar
* Hohe Fehleranfälligkeit

### Positives Beispiel
```
/**
 * Sends an email to a Frontend-User
 *
 * @param \RKW\RkwForm\Domain\Model\StandardForm $formRequest
 * @return \RKW\RkwRegistration\Domain\Model\FrontendUser|null 
 * @throws \RKW\RkwMailer\Exception
 */
public function userMail(StandardForm $formRequest):? FrontendUser
```
* Klare Deklaration der Parameter
* Klare Deklaration des Rückgabewertes
* über ``:?`` wird auch der Fall abgefangen, dass null zurückgegeben wird
* Geringe Fehleranfälligkeit

## phpDoc
Die phpDocs dienen dazu, für andere Entwickler*innen zu dokumentieren, was die Methode an Parametern erwartet und an Rückgabewerten liefert.
Es ist daher auch eine Frage des Umgangs untereinander, die phpDocs sauber zu pflegen. 
Sie dienen dazu, auf einem Blick die relevanten Parameter und Rückgabewerte einer Methode sehen zu können. Daher gelten auch folgende Regeln:
* Reihenfolge der @-Angaben: 
  1. Parameter
  2. Rückgabewerte
  3. Exceptions
  4. alles weitere
* Keine Leerzeilen zwischen den @-Angaben
* Keine verkürzten Angaben des Namespace-Pfades von Objekten
* Bei Methoden: verpflichtende Angabe des Rückgabewertes. Ist dieser leer, dann ``@return void``
* Angabe der phpDocs auch für properties 
* Verzicht auf Angabe des property-Namens bei properties (Redundanz vermeiden)
* Wird innerhalb einer Methode eine Variable zugewiesen, so ist auch für diese eine phpDoc-Angabe an Ort und stelle und nach den hier definierten Regeln zu machen
``` 
/** @var \TYPO3\CMS\Core\Configuration\Loader\YamlFileLoader $yamlFileLoader */
$yamlFileLoader = GeneralUtility::makeInstance(YamlFileLoader::class);
```

### Negatives Beispiel
```
/**
* $companytypeRepository
*
* @var \RKW\RkwBasics\Domain\Repository\CompanyTypeRepository
* @inject
*/
protected $companytypeRepository = null;

/**
* @throws \Exception
* @param $fieldsets
*
* @return mixed
*/
protected function getFieldsLayout($fieldsets)
```
* Redundante Angabe des property-Namens
* Definition des Properties als null ist unnötig
* Keine Beschreibung der Methode
* ``@throws`` an falscher Stelle
* ``@throws`` zu allgemein - welcher Fehler wird genau geworfen?
* Parameter-Angabe gibt keinerlei Hinweis auf den Typ
* Leerzeichen zwischen ``@param`` und ``@return``
* Keinerlei Hinweise auf den zu erwartenden Rückgabewert

### Positives Beispiel
```
/**
* find expired form entries
* 
* @param string $identifier
* @return \TYPO3\CMS\Extbase\Persistence\QueryResultInterface|null
* @throws \TYPO3\CMS\Extbase\Persistence\Exception\InvalidQueryException
*/
public function findExpiredByFormIdentifier(string $identifier = ''):? QueryResultInterface
```
* Kurze Beschreibung gibt Infos zur Funktion
* @tags in richtiger Reihenfolge
* Keine unnötigen Leerzeilen zwischen den @tags
* Konkrete und absolute Angabe des Rückgabewertes
* Berücksichtigung des alternativen Rückgabewertes (``null``)
* Konkrete und absolute Angabe des Fehlerwertes

