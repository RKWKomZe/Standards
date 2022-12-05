# Sprachkonstrukte

## Bedingungen
Bedingungen bestehen aus den Schlüsselwörtern ``if``, ``elseif`` und ``else``.
``else if`` wird nicht verwendet.

Beispiel:
```
if ($this->processSubmission) {
    // Process submission here
} elseif ($this->internalError) {
    // Handle internal error
} else {
    // Something else here
}
```

Es wird empfohlen, die Bedingungen so zu gestalten, dass der kürzeste Codeblock zuerst ausgeführt wird.
Beispiel:
```
if (!$this->processSubmission) {
    // Generate error message, 2 lines
} else {
    // Process submission, 30 lines
}
```

Wenn die Bedingung lang ist, muss sie in mehrere Zeilen aufgeteilt werden. 
Die logischen Operatoren müssen vor der nächsten Bedingung stehen und auf der 
gleichen Ebene wie die erste Bedingung eingerückt werden. Die abschließende runde und die öffnende 
geschweifte Klammer nach der letzten Bedingung sollten in einer neuen Zeile stehen, die auf die gleiche 
Ebene wie die if-Bedingung eingerückt ist:

Beispiel:
```
if ($this->getSomeCondition($this->getSomeVariable())
    && $this->getAnotherCondition()
) {
    // Code follows here
}
```

Der ternäre Bedingungsoperator ? : darf nur verwendet werden, wenn er genau zwei mögliche Ergebnisse hat.
Beispiel:
```
$result = ($useComma ? ',' : '.');
```

## Case-Switch
``case``-Anweisungen werden mit einer zusätzlichen Einrückung (vier Leerzeichen) innerhalb der
``switch``-Anweisung eingerückt. Der Code innerhalb der ``case``-Anweisungen wird mit einer zusätzlichen
Einrückung weiter eingerückt. Die break-Anweisung wird an den Code angehängt. Pro ``case`` ist nur 
eine ``break``-Anweisung zulässig.

Die Standardanweisung muss die letzte in der ``switch``-Anweisung sein und darf keine ``break``-Anweisung enthalten.

Wenn ein ``case``-Block die Kontrolle an einen anderen ``case``-Block weitergeben muss, ohne dass ein
``break`` vorhanden ist, muss dies im Code kommentiert werden.

Beispiel:
```
switch ($useType) {
    case 'extended':
        $content .= $this->extendedUse();
        // Fall through
    case 'basic':
        $content .= $this->basicUse();
        break;
    default:
        $content .= $this->errorUse();
}
```

## Schleifen
Die folgenden Schleifen können verwendet werden:
* ``do``
* ``while``
* ``for``
* ``foreach``

Die Verwendung von`` each`` ist in Schleifen nicht erlaubt.
``for``-Schleifen dürfen nur Variablen enthalten (keine Funktionsaufrufe). Das Folgende ist korrekt:
```
$size = count($dataArray);
for ($element = 0; $element < $size; $element++) {
    // Process element here
}
```
Das Folgende ist nicht erlaubt:
```
for ($element = 0; $element < count($dataArray); $element++) {
    // Process element here
}
```
``do``- und ``while``-Schleifen müssen zusätzliche Klammern verwenden, wenn eine Zuweisung in der Schleife erfolgt:
```
while (($fields = $this->getFields())) {
    // Do something
}
```
Es gibt einen Sonderfall für foreach-Schleifen, wenn der Wert nicht innerhalb der Schleife verwendet wird. 
In diesem Fall wird die Dummy-Variable $_ (Unterstrich) verwendet:
```
foreach ($GLOBALS['TCA'] as $table => $_) {
    // Do something with $table
}
```
Dies geschieht aus Leistungsgründen, da es schneller ist, als array_keys() aufzurufen und das Ergebnis in einer Schleife zu verarbeiten.

## Zeichenketten
Alle Zeichenketten müssen in einfache Anführungszeichen gesetzt werden. 
Doppelte Anführungszeichen sind nur erlaubt, um das Zeichen für den Zeilenwechsel ("\n") zu erzeugen.

Operatoren zur Verkettung von Zeichenfolgen müssen von Leerzeichen umgeben sein. Beispiel:
```
$Inhalt = 'Hallo ' . 'Welt!';
```
Das Leerzeichen nach dem Verkettungsoperator darf jedoch nicht vorhanden sein, 
wenn der Operator die letzte Konstruktion in der Zeile ist.

Variablen dürfen nicht in Zeichenketten eingebettet werden. Korrekt:
```
$Inhalt = 'Hallo ' . $Benutzername;
```
Falsch:
```
$Inhalt = "Hallo $Benutzername";
```
Mehrzeilige String-Verkettungen sind erlaubt. 
Der Zeilenverkettungsoperator muss am Anfang der Zeile stehen. 
Zeilen ab der zweiten Zeile müssen relativ zur ersten Zeile eingerückt werden. 
Es wird empfohlen, die Zeilen eine Ebene nach dem Beginn der Zeichenkette auf der ersten Ebene einzurücken.
```
$content = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. '
                . 'Donec varius libero non nisi. Proin eros.';
```

## Boolesche Werte
Boolesche Werte müssen die Sprachkonstrukte von PHP verwenden und nicht explizite Integer-Werte wie ``0`` oder ``1``. 
Außerdem sollten sie in Kleinbuchstaben geschrieben werden, d.h. ``true`` und ``false``.

## NULL
Auch dieser spezielle Wert wird in Kleinbuchstaben geschrieben, d.h. ``null``.

## Arrays
Array-Deklarationen verwenden die kurze Array-Syntax [] anstelle des Schlüsselworts "array". Also
```
$a = [];
```
Array-Komponenten werden jeweils in einer eigenen Zeile deklariert. Solche Zeilen werden mit vier Leerzeichen mehr eingerückt als der Beginn der Deklaration. Die abschließende eckige Klammer steht auf der gleichen Einrückungsebene wie die Variable. Jede Zeile, die ein Array-Element enthält, endet mit einem Komma. Dieses kann nach Belieben des Entwicklers weggelassen werden, wenn es keine weiteren Elemente gibt. Beispiel:
```
$thisIsAnArray = [
    'foo' => 'bar',
    'baz' => [
        0 => 1
    ]
];
```
Verschachtelte Arrays folgen dem gleichen Muster. Diese Formatierung gilt auch für sehr kleine und einfache Array-Deklarationen, z.B. :
```
$a = [
    0 => 'b',
];
```