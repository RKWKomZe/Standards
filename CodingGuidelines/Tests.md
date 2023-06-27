# Testing

## Relevante Arten von Tests
Wir unterscheiden im Wesentlichen drei zentrale Arten von Tests. 

![Test-Pyramide, Quelle: https://www.softwaretestinghelp.com/the-difference-between-unit-integration-and-functional-testing/](img/tests.png)

### Unit-Tests 

Hierbei werden einzelne Elemente einer Anwendung isoliert (ohne Interaktion mit Abhängigkeiten) getestet, um zu evaluieren, ob der Code korrekt funktioniert.

Im Regelfall werden hier einzelne Methoden isoliert voneinander getestet. Unit-Tests sind die Basis für Test-Driven-Development.

Insbesondere dann, wenn eine Methode jedoch wesentlich auf das Zusammenspiel mit Einträgen in der Datenbank angewiesen ist, kann es aber sinnvoll sein, die Ebene des Unit-Tests für diese Methode zu vernachlässigen und direkt in den (Unit-)Integration-Test überzugehen. Denkbar ist auf Basis dieses Verständnisses auch, (Unit-)Integration-Tests bevorzugt zu behandeln, da mit ihrer Hilfe auch Features der Software klarer formuliert und entsprechende Testszenarien skizziert werden können. Wenn sich aus den (Unit-)Integration-Tests ergibt, dass etwa eine komplexere Funktion als isoliertes Unit korrekt ausgeführt sein muss, so implementiert man zuerst diesen Unit-Test, bevor man sich wieder dem (Unit-)Integration-Test zuwendet. 

### (Unit-)Integration-Tests

Hier wird geprüft, ob verschiedene Einheiten (Units) in Kombination als Gruppe einwandfrei miteinander funktionieren.

Im Mittelpunkt steht hier also das Zusammenspiel von einzelnen Komponenten. 

In diesem Sinne sind Integration-Tests bereits abstrakter  als Unit-Tests, können sich aber durchaus auf eine konkrete Methode beziehen, die ihrerseits andere Methoden aufruft.

An dieser Stelle gilt es, ebenso wie bei den Unit-Tests, darauf hinzuweisen, dass die Formulierung eines (Unit-)Integration-Tests als Featureanforderung durchaus der erste Schritt im Rahmen des Test Driven Development sein kann und darf. 

![Integration Test failed](http://www.lops.io/assets/img/post/automated-testing-functional/meme-unit-tests-passing-no-integration-tests.jpg)

### Functional-Tests

Bei Functional-Tests steht das Funktionieren des gesamten Systems / Moduls im Vordergrund.

Dies ist die abstrakteste der drei hier genannten Test-Formen. 

Im Regelfall werden hier technische Anforderungen in Form von Test-Szenarien definiert, deren Erfüllung geprüft wird. Diese Form des Tests setzt daher im Kern keine Kenntnisse über die konkrete Funktionsweise des Codes voraus, sondern lediglich das Wissen darüber, was der Code am Ende machen soll. Das bedeutet, dass diese Tests das Verhalten und die Erwartung eines echten Nutzers simulieren sollen.

Diese Tests führen stellvertretend für den Nutzer eine Anfrage an das System durch und prüfen die Antwort auf die gesetzten Erwartungen. Diese Anfrage kann entweder als HTTP-Request im Rahmen des genutzten Frameworks selbst erfolgen oder wie im Falle von zum Beispiel [Laravel Dusk](https://laravel.com/docs/5.8/dusk) auch als echte Anfrage via Browser.

Da diese Art der Testung als oberste Abstraktionsebene der Formulierung eines Features am nächsten kommt, kann dies auch Ausgangspunkt des Test Driven Development sein. 

Die Testung über einen echten Browser wie im Falle von Laravel Dusk oder Selenium, ... bietet zudem die Möglichkeit, das Interface auch dann zu testen, wenn es in wesentlichen Elementen oder vollständig per Javascript erst im Browser selbst gerendert wird.

## Namenskonvention für Test-Methoden
Um die Benennung der Test-Methoden stringent und übersichtlich zu gestalten, werden folgende allgemeine Regeln definiert:

* Die Notation des Test-Methoden-Namens erfolgt pinzipiell in lowerCamelCase. Dies entspricht dem Standard im TYPO3-Core, Symfony, ZendFramework und in großen Teilen von Laravel.

* Um die Lesbarkeit zu erhöhen, wird statt des Präfixes 'test' im Methodennamen ausschließlich die @test-Annotation im Dokumentationsblock der Methode verwendet
```
    /**
     * @test
     */
```

* Sofern sich Tests auf konkrete Einheiten einer Anwendung (=Methoden) beziehen, wird der Name der zu testenden Methode dem Test-Methoden-Namen vorangestellt. Dies trifft im Regelfall auf Unit-Tests, häufig auf (Unit-)Integration-Tests, sollte jedoch nie auf Functional-Tests zutreffen. 

* Verweisen Tests nicht auf konkrete Units, wird stattdessen das Keyword "it" vorangestellt.

* Generell orientieren sich die Test-Methoden-Namen an der Logik des Behavior-Driven-Development, d.h. die Test-Methoden-Namen beschreiben, was die Methode (unter den gegebenen Bedingungen) tut. Eine gute Orientierung bietet hier Gherkin (https://cucumber.io/docs/gherkin/reference/).

    * Wesentlich ist, dass der Test-Methoden-Namen das zu erwartende Verhalten beschreibt und ggf. die dazu notwendige Bedingung enthält.

    * Die Beschreibung des erwarteten Verhaltens sollte mithilfe von Verben erfolgen und hinreichend konkret sein.  

### Positive Beispiele
_Unit-Test:_
```
clearPageCacheDoesNotConvertPageIdsIfNoneAreSpecified()
```

### Negative Beispiele
_Unit-Test:_
```
clearPageCacheWorksCorrectly()
```

## Aufbau eines Tests
* Innerhalb der Tests ist sich von der Struktur her an Gherkin anzulehnen. Jeder Test enthält daher mindestens drei Teile, die auch entsprechend als Szenario im Test beschrieben werden:

    * **Given**: definiert die Ausgangsbedingungen für den Test. Dies können entsprechende Vor-Konfigurationen oder aber auch Datensätze in der Datenbank sein. Jeder Test kann mehrere Given-Statements enthalten. Der Zweck von Given-Statements ist es, den bekannten Zustand des Systems zu beschreiben und diesen Ausgangszustand herzustellen, bevor der Benutzer (oder das externe System) mit der Interaktion mit dem System beginnt (in den When-Statements).
    
    * **When**: definiert die eigentliche Aktion, die durchgeführt wird. Je nach Test-Art ist dies abstrakter oder weniger abstrakt. Jeder Test sollte nur EIN When-Statement enthalten.
    
    * **Then**: definiert das erwartete Resultat. Jeder Test kann mehrere Then-Statements enthalten. Hierin finden sich auch die Assertions, d. h. die eigentlichen Prüfungen, ob die erhaltenen Werte den erwarteten entsprechen.
    
* Für Functional-Tests gilt, dass deren Szenarios so abstrakt formuliert werden sollten, dass keine Kenntnis der tatsächlichen Implementierung enthalten bzw. vonnöten ist.

* Naturgemäß sind Unit- und Integration-Tests weniger abstrakt und folglich auch in der Beschreibung der Szenarien technischer als Functional-Test.
 
_Beispiel, Integration-Test, mittlere Abstraktion:_
```
/**
 * @test
 * @throws \RKW\RkwMailer\Service\Exception\MailServiceException
 * @throws \TYPO3\CMS\Extbase\Persistence\Exception\IllegalObjectTypeException
 * @throws \TYPO3\CMS\Extbase\Persistence\Exception\UnknownObjectException
 */
public function setQueueMailDoesNotAllowNonPersistentQueueMail()
{
    /**
    * Scenario:
    *
    * Given MailService is initiated
    * When a non-persistent queueMail-object is to be set
    * Then an error is thrown
    */
    static::expectException(\RKW\RkwMailer\Service\Exception\MailServiceQueueMailException::class);

    /** @var \RKW\RkwMailer\Domain\Model\QueueMail $queueMail */
    $queueMail = GeneralUtility::makeInstance(\RKW\RkwMailer\Domain\Model\QueueMail::class);
    $this->subject->setQueueMail($queueMail);
}
```    

## Aufbau und Struktur
* Jeder Test-File hält zwingend auch die an anderer Stelle dieser Guidelines definierte Struktur ein, d.h. weder die Lizenzhinweise noch die PHPdocs dürfen fehlen.
* Die Test-Methoden werden gruppiert und zwar so, dass zusammenhängende Tests von den anderen Tests durch einen Linien-Kommentar abgegrenzt werden
```
    [TEST-BLOCK 1]
    
    }

    #==========================================================================

     /**
     * @test
     */
    public function 

    [TEST-BLOCK 2]
```
* Jeder Test enthält eine setUp()- und tearDown()-Methode, auch wenn sie nur die parent-Methode aufruft. Beide sind von den anderen Methoden ebenfalls durch einen Linien-Kommentar abzutrennen
* Sofern ein Test auf Fixtures angewiesen ist, werden diese nicht global im SetupUp definiert, sondern für die Erhaltung der Übersichtlichkeit im jeweiligen Test geladen.
* Die XML-Dateien für die Datenbank-Daten sollten nicht nach Tabellennamen, sondern nach Tests aufgeteilt sein, sodass alle Datenbank-Daten (über verschiedene Tabellen) für einen Test (oder einer Gruppe von Tests, die die gleichen Daten verwenden) in einer Datei liegen. Dies verhindert auch versehentliche Überschneidungen in Datensätzen und erhöht die Übersichtlichkeit.
* Die Benennung der Dateien ist weitgehend frei, sofern eine Zuordung des Fixture-Files zum Test in irgendeiner Weise möglich bleibt.
* Der Pfad zu den Fixture-Files wird in der Test-Klasse als Konstante definiert und ausschließlich verwendet:
```
    /**
     * @const
     */
    const FIXTURE_PATH = __DIR__ . '/CalculationViewHelperTest/Fixtures';
```
* Die im nachfolgenden Beispiel gezeigte Ordner-Struktur hat sich bewährt 

### Beispiel: TYPO3 Integration-Test
#### Ordnerstruktur
```
Tests
    Integration
        Orders  (analog zu "Classes")
            OrderManagerTest (= Entspricht Test-Klasse)
                Database (= XML-Files für die Tests)
                    Check10.xml (= enthält Datenban-Einträge für den ersten Test) 
                    Check20.xml ...
                    [...]
                    Global.xml (= enthält allgemeine Datenbank-Einträge, die in SetUp benötigt werden, z.B. die Rootpage)
                Frontend
                    Configuration
                        Rootpage.typoscript (= Rootpage-Setup)
                    Templates (= enthält Templates für das Frontend
            OrderManagerTest.php (= Test-File)
```

#### Inhalt: Test/Integration/Orders/Database/Global.xml
```
<?xml version="1.0" encoding="utf-8"?>
<dataset>
    <pages>
        <uid>1</uid>
        <pid>0</pid>
        <title>Rootpage</title>
        <tx_rkwbasics_teaser_text />
        <tx_rkwbasics_information />
        <doktype>1</doktype>
        <perms_everybody>15</perms_everybody>
    </pages>
</dataset>
```

#### Inhalt: Test/Integration/Orders/Database/Test10.xml
```
<?xml version="1.0" encoding="utf-8"?>
<dataset>
    <tx_rkwshop_domain_model_product>
        <uid>1</uid>
        <pid>1</pid>
        <title>Leitfaden deluxe</title>
        <subtitle>Mit Unterüberschriften kann man alles länger machen</subtitle>
        <admin_email>test3@test.de</admin_email>
        <stock>1</stock>
        <backend_user />
    </tx_rkwshop_domain_model_product>
    <tx_rkwshop_domain_model_stock>
        <uid>1</uid>
        <pid>1</pid>
        <product>1</product>
        <amount>100</amount>
        <comment>Auf- und Unterlage</comment>
    </tx_rkwshop_domain_model_stock>
</dataset>
```
#### Eigentlicher Test aus Test/Integration/Orders/OrderManagerTest.php
```
/**
     * @test
     * @throws \RKW\RkwShop\Exception
     * @throws \RKW\RkwRegistration\Exception
     * @throws \TYPO3\CMS\Extbase\Persistence\Exception\IllegalObjectTypeException
     * @throws \TYPO3\CMS\Extbase\Persistence\Exception\UnknownObjectException
     * @throws \TYPO3\CMS\Extbase\Persistence\Exception\InvalidQueryException
     * @throws \TYPO3\CMS\Extbase\SignalSlot\Exception\InvalidSlotException
     * @throws \TYPO3\CMS\Extbase\SignalSlot\Exception\InvalidSlotReturnException
     * @throws \TYPO3\CMS\Extbase\Configuration\Exception\InvalidConfigurationTypeException
     * @throws \TYPO3\CMS\Core\Type\Exception\InvalidEnumerationValueException
     * @throws \Exception
     */
    public function createOrderCreatesRegistrationIfUserIsNotLoggedIn ()
    {

        /**
         * Scenario:
         *
         * Given I'm not logged in
         * Given I accept the Terms & Conditions
         * Given I accept the Privacy-Terms
         * Given I enter a valid shippingAddress
         * Given an product is ordered with amount greater than zero
         * When I place an order
         * Then the order is saved as registration
         */
        $this->importDataSet(self::FIXTURE_PATH . '/Database/Test10.xml');

        /** @var \RKW\RkwShop\Domain\Model\Order $order */
        $order = GeneralUtility::makeInstance(Order::class);
        $order->setEmail('email@rkw.de');

        /** @var \RKW\RkwShop\Domain\Model\ShippingAddress $shippingAddress */
        $shippingAddress = GeneralUtility::makeInstance(ShippingAddress::class);
        $shippingAddress->setAddress('Emmenthaler Allee 15');
        $shippingAddress->setZip('12345');
        $shippingAddress->setCity('Gauda');
        $order->setShippingAddress($shippingAddress);

        /** @var \RKW\RkwShop\Domain\Model\Product $product */
        $product = $this->productRepository->findByUid(1);

        /** @var \RKW\RkwShop\Domain\Model\OrderItem $orderItem */
        $orderItem = GeneralUtility::makeInstance(OrderItem::class);
        $orderItem->setProduct($product);
        $orderItem->setAmount(10);
        $order->addOrderItem($orderItem);

        /** @var \TYPO3\CMS\Extbase\Mvc\Request $request */
        $request = $this->objectManager->get(Request::class);

        static::assertEquals(
            'orderManager.message.createdOptIn',
            $this->subject->createOrder($order, $request, null, true, true)
        );

        /** @var \RKW\RkwRegistration\Domain\Model\Registration $registration */
        $registration = $this->registrationRepository->findByUid(1);
        static::assertInstanceOf('RKW\RkwRegistration\Domain\Model\Registration', $registration);
        static::assertEquals(1, $registration->getUser());
        static::assertEquals('rkwShop', $registration->getCategory());

    }

```


## Links
* https://codeutopia.net/blog/2015/04/11/what-are-unit-testing-integration-testing-and-functional-testing/
* http://www.lops.io/automated-testing-functional/
* https://www.softwaretestinghelp.com/the-difference-between-unit-integration-and-functional-testing/
* https://dannorth.net/introducing-bdd/
* https://cucumber.io/docs/gherkin/reference/
