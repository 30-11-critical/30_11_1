## Wie man Tests schreibt

Tests sind Rust-Funktionen, die überprüfen, ob der Nicht-Testcode wie erwartet funktioniert. Die Hauptteile von Testfunktionen führen normalerweise diese drei Aktionen aus:

1. Richten Sie alle erforderlichen Daten oder Zustände ein.
2. Führen Sie den Code aus, den Sie testen möchten.
3. Bestätigen Sie, dass die Ergebnisse Ihren Erwartungen entsprechen.

Schauen wir uns die Funktionen an, die Rust speziell zum Schreiben von Tests bietet, die diese Aktionen ausführen, darunter das Attribut `test` , einige Makros und das Attribut `should_panic` .

### Die Anatomie einer Testfunktion

Im einfachsten Fall ist ein Test in Rust eine Funktion, die mit dem Attribut `test` annotiert ist. Attribute sind Metadaten über Teile von Rust-Code; Ein Beispiel ist das Attribut `derive` , das wir in Kapitel 5 mit Strukturen verwendet haben. Um eine Funktion in eine Testfunktion umzuwandeln, fügen Sie `#[test]` in die Zeile vor `fn` ein. Wenn Sie Ihre Tests mit dem `cargo test` Befehl ausführen, erstellt Rust eine Test-Runner-Binärdatei, die die annotierten Funktionen ausführt und darüber berichtet, ob jede Testfunktion bestanden wird oder fehlschlägt.

Immer wenn wir mit Cargo ein neues Bibliotheksprojekt erstellen, wird automatisch ein Testmodul mit einer darin enthaltenen Testfunktion generiert. Dieses Modul gibt Ihnen eine Vorlage zum Schreiben Ihrer Tests, damit Sie nicht jedes Mal, wenn Sie ein neues Projekt starten, die genaue Struktur und Syntax nachschlagen müssen. Sie können beliebig viele zusätzliche Testfunktionen und Testmodule hinzufügen!

Wir werden einige Aspekte der Funktionsweise von Tests untersuchen, indem wir mit dem Vorlagentest experimentieren, bevor wir tatsächlich Code testen. Dann schreiben wir einige reale Tests, die einen von uns geschriebenen Code aufrufen und bestätigen, dass sein Verhalten korrekt ist.

Lassen Sie uns ein neues Bibliotheksprojekt namens `adder` erstellen, das zwei Zahlen addiert:

```console
$ cargo new adder --lib
     Created library `adder` project
$ cd adder
```

Der Inhalt der Datei *src/lib.rs* in Ihrer `adder` -Bibliothek sollte wie in Listing 11-1 aussehen.

<span class="filename">Dateiname: src/lib.rs</span>

<!-- manual-regeneration
cd listings/ch11-writing-automated-tests
rm -rf listing-11-01
cargo new --lib listing-11-01 --name adder
cd listing-11-01
cargo test
git co output.txt
cd ../../..
-->

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-01/src/lib.rs}}
```

<span class="caption">Listing 11-1: Das von <code>cargo new</code> automatisch generierte Testmodul und die Funktion</span>

Lassen Sie uns vorerst die oberen beiden Zeilen ignorieren und uns auf die Funktion konzentrieren. Beachten Sie die Annotation `#[test]` : Dieses Attribut gibt an, dass es sich um eine Testfunktion handelt, sodass der Testrunner weiß, dass er diese Funktion als Test behandeln muss. Wir haben möglicherweise auch Nicht-Testfunktionen im Testmodul, um allgemeine Szenarios einzurichten oder allgemeine Vorgänge auszuführen, daher müssen wir immer angeben, welche Funktionen `tests` sind.

Der Beispielfunktionskörper verwendet das `assert_eq!` -Makro, um zu behaupten, dass `result` , das das Ergebnis der Addition von 2 und 2 enthält, gleich 4 ist. Diese Behauptung dient als Beispiel für das Format eines typischen Tests. Lassen Sie es uns ausführen, um zu sehen, ob dieser Test bestanden wird.

Der Befehl `cargo test` führt alle Tests in unserem Projekt aus, wie in Listing 11-2 gezeigt.

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-01/output.txt}}
```

<span class="caption">Listing 11-2: Die Ausgabe des automatisch generierten Tests</span>

Cargo hat den Test zusammengestellt und durchgeführt. Wir sehen, dass die Zeile `running 1 test` . Die nächste Zeile zeigt den Namen der generierten Testfunktion namens `it_works` und dass das Ergebnis der Ausführung dieses Tests `ok` ist. Das zusammenfassende Gesamttestergebnis `test result: ok.` bedeutet, dass alle Tests bestanden wurden und der Teil, der `1 passed; 0 failed` bestanden summiert die Anzahl der Tests, die bestanden oder nicht bestanden wurden.

Es ist möglich, einen Test als ignoriert zu markieren, damit er in einer bestimmten Instanz nicht ausgeführt wird; Wir werden das im [Abschnitt „Ignorieren einiger Tests, sofern nicht ausdrücklich angefordert“] behandeln.<!-- ignorieren --> Abschnitt später in diesem Kapitel. Da wir das hier nicht getan haben, zeigt die Zusammenfassung `0 ignored` . Wir können auch ein Argument an den Befehl `cargo test` übergeben, um nur Tests auszuführen, deren Name mit einer Zeichenfolge übereinstimmt; Dies wird als Filtern bezeichnet und wir behandeln das im [Abschnitt „Ausführen einer Teilmenge von Tests nach Namen“.]<!-- ignorieren --> Sektion. Wir haben die ausgeführten Tests auch nicht gefiltert, sodass am Ende der Zusammenfassung `0 filtered out` .

Die Statistik „ `0 measured` “ ist für Benchmark-Tests, die die Leistung messen. Benchmark-Tests sind zum jetzigen Zeitpunkt nur in Nightly Rust verfügbar. Weitere Informationen finden Sie in [der Dokumentation zu Benchmark-Tests] .

Der nächste Teil der Testausgabe, beginnend bei `Doc-tests adder` ist für die Ergebnisse aller Dokumentationstests. Wir haben noch keine Dokumentationstests, aber Rust kann alle Codebeispiele kompilieren, die in unserer API-Dokumentation erscheinen. Diese Funktion hilft dabei, Ihre Dokumente und Ihren Code synchron zu halten! Wie man Dokumentationstests schreibt, besprechen wir in [„Dokumentationskommentare als Tests“] .<!-- ignorieren --> Abschnitt von Kapitel 14. Im Moment ignorieren wir die Ausgabe von `Doc-tests` .

Beginnen wir damit, den Test an unsere eigenen Bedürfnisse anzupassen. Ändern Sie zunächst den Namen der Funktion `it_works` in einen anderen Namen, z. B. `exploration` , wie folgt:

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-01-changing-test-name/src/lib.rs}}
```

Führen Sie dann `cargo test` erneut durch. Die Ausgabe zeigt jetzt `it_works` `exploration`

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-01-changing-test-name/output.txt}}
```

Jetzt fügen wir einen weiteren Test hinzu, aber diesmal machen wir einen Test, der fehlschlägt! Tests schlagen fehl, wenn etwas in der Testfunktion in Panik gerät. Jeder Test wird in einem neuen Thread ausgeführt, und wenn der Haupt-Thread feststellt, dass ein Test-Thread gestorben ist, wird der Test als fehlgeschlagen markiert. In Kapitel 9 haben wir darüber gesprochen, dass der einfachste Weg, in Panik zu geraten, darin besteht, die `panic!` Makro. Geben Sie den neuen Test als Funktion namens `another` , sodass Ihre Datei *src/lib.rs* wie in Listing 11.3 aussieht.

<span class="filename">Dateiname: src/lib.rs</span>

```rust,panics,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-03/src/lib.rs:here}}
```

<span class="caption">Listing 11-3: Hinzufügen eines zweiten Tests, der fehlschlagen wird, weil wir <code>panic!</code> Makro</span>

Führen Sie die Tests mit `cargo test` erneut durch. Die Ausgabe sollte wie in Listing 11.4 aussehen, was zeigt, dass unser `exploration` bestanden und `another` fehlgeschlagen ist.

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-03/output.txt}}
```

<span class="caption">Listing 11-4: Testergebnisse, wenn ein Test bestanden und ein Test fehlgeschlagen ist</span>

Statt `ok` zeigt die Zeile `test tests::another` `FAILED` . Zwischen den einzelnen Ergebnissen und der Zusammenfassung erscheinen zwei neue Abschnitte: Der erste zeigt den detaillierten Grund für jeden Testfehler an. In diesem Fall erhalten wir die Details, dass `another` fehlgeschlagen ist, weil er `panicked at 'Make this test fail'` in Zeile 10 in der Datei *src/lib.rs* in Panik geraten ist. Der nächste Abschnitt listet nur die Namen aller fehlgeschlagenen Tests auf, was nützlich ist, wenn es viele Tests und viele detaillierte fehlgeschlagene Testausgaben gibt. Wir können den Namen eines fehlgeschlagenen Tests verwenden, um nur diesen Test auszuführen, um ihn einfacher zu debuggen; Wir werden mehr über Möglichkeiten zum Ausführen von Tests im [Abschnitt „Kontrollieren, wie Tests ausgeführt werden“](ch11-02-running-tests.html#controlling-how-tests-are-run) sprechen.<!-- ignorieren --> Sektion.

Die zusammenfassende Zeile wird am Ende angezeigt: Insgesamt lautet unser Testergebnis `FAILED` . Wir hatten einen Test bestanden und einen Test nicht bestanden.

Nachdem Sie nun gesehen haben, wie die Testergebnisse in verschiedenen Szenarien aussehen, schauen wir uns einige andere Makros als `panic!` die in Tests nützlich sind.

### Überprüfen der Ergebnisse mit dem `assert!` Makro

Die `assert!` Makro, das von der Standardbibliothek bereitgestellt wird, ist nützlich, wenn Sie sicherstellen möchten, dass eine Bedingung in einem Test als `true` ausgewertet wird. Wir geben die `assert!` Makro ein Argument, das zu einem booleschen Wert ausgewertet wird. Wenn der Wert `true` ist, passiert nichts und der Test wird bestanden. Wenn der Wert `false` ist, wird die `assert!` Makro ruft `panic!` damit der Test fehlschlägt. Mit Hilfe des `assert!` Makro hilft uns zu überprüfen, ob unser Code so funktioniert, wie wir es beabsichtigen.

In Kapitel 5, Listing 5-15, haben wir eine `Rectangle` -Struktur und eine `can_hold` Methode verwendet, die hier in Listing 11-5 wiederholt werden. Lassen Sie uns diesen Code in die Datei *src/lib.rs einfügen* und dann einige Tests dafür schreiben, indem wir das `assert!` Makro.

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-05/src/lib.rs:here}}
```

<span class="caption">Listing 11-5: Verwendung der <code>Rectangle</code> -Struktur und ihrer <code>can_hold</code> Methode aus Kapitel 5</span>

Die Methode `can_hold` gibt einen booleschen Wert zurück, was bedeutet, dass dies ein perfekter Anwendungsfall für die `assert!` Makro. In Listing 11.6 schreiben wir einen Test, der die Methode `can_hold` , indem wir eine `Rectangle` -Instanz erstellen, die eine Breite von 8 und eine Höhe von 7 hat, und behaupten, dass sie eine andere `Rectangle` Instanz mit einer Breite von 5 und einer Höhe von enthalten kann 1.

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-06/src/lib.rs:here}}
```

<span class="caption">Listing 11-6: Ein Test für <code>can_hold</code> , der überprüft, ob ein größeres Rechteck tatsächlich ein kleineres Rechteck aufnehmen kann</span>

Beachten Sie, dass wir im `tests` eine neue Zeile hinzugefügt haben: `use super::*;` . Das `tests` ist ein reguläres Modul, das den üblichen Sichtbarkeitsregeln folgt, die wir in Kapitel 7 in [„Pfade zum Verweisen auf ein Element in der Modulstruktur“] behandelt haben.<!-- ignorieren --> Sektion. Da das `tests` ein inneres Modul ist, müssen wir den zu testenden Code im äußeren Modul in den Geltungsbereich des inneren Moduls bringen. Wir verwenden hier einen Glob, sodass alles, was wir im äußeren Modul definieren, für dieses `tests` verfügbar ist.

Wir haben unseren Test `larger_can_hold_smaller` genannt und wir haben die zwei `Rectangle` -Instanzen erstellt, die wir brauchen. Dann riefen wir die `assert!` Makro und übergab ihm das Ergebnis des Aufrufs von `larger.can_hold(&smaller)` . Dieser Ausdruck soll `true` zurückgeben, also sollte unser Test bestehen. Lass es uns herausfinden!

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-06/output.txt}}
```

Es geht vorbei! Lassen Sie uns einen weiteren Test hinzufügen, diesmal mit der Behauptung, dass ein kleineres Rechteck kein größeres Rechteck halten kann:

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-02-adding-another-rectangle-test/src/lib.rs:here}}
```

Da das korrekte Ergebnis der Funktion `can_hold` in diesem Fall `false` ist, müssen wir dieses Ergebnis negieren, bevor wir es an das `assert!` Makro. Als Ergebnis wird unser Test bestanden, wenn `can_hold` `false` zurückgibt:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-02-adding-another-rectangle-test/output.txt}}
```

Zwei Prüfungen, die bestehen! Sehen wir uns nun an, was mit unseren Testergebnissen passiert, wenn wir einen Fehler in unseren Code einführen. Wir ändern die Implementierung der Methode `can_hold` , indem wir beim Vergleich der Breiten das Größer-als-Zeichen durch ein Kleiner-als-Zeichen ersetzen:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-03-introducing-a-bug/src/lib.rs:here}}
```

Das Ausführen der Tests erzeugt nun Folgendes:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-03-introducing-a-bug/output.txt}}
```

Unsere Tests haben den Fehler entdeckt! Da `larger.width` 8 und `smaller.width` 5 ist, gibt der Vergleich der widths in `can_hold` nun `false` zurück: 8 ist nicht kleiner als 5.

### Testen der Gleichheit mit dem `assert_eq!` und `assert_ne!` Makros

Eine gängige Methode zum Überprüfen der Funktionalität besteht darin, auf Gleichheit zwischen dem Ergebnis des getesteten Codes und dem Wert zu testen, den Sie vom Code erwarten. Sie könnten dies mit dem `assert!` Makro und übergibt ihm einen Ausdruck mit dem Operator `==` . Dies ist jedoch ein so häufiger Test, dass die Standardbibliothek ein Paar Makros bereitstellt – `assert_eq!` und `assert_ne!` —um diesen Test bequemer durchzuführen. Diese Makros vergleichen zwei Argumente auf Gleichheit bzw. Ungleichheit. Sie geben auch die beiden Werte aus, wenn die Assertion fehlschlägt, was es einfacher macht zu erkennen, *warum* der Test fehlgeschlagen ist; umgekehrt, die `assert!` Makro zeigt nur an, dass es einen `false` Wert für den Ausdruck `==` erhalten hat, ohne die Werte auszugeben, die zu dem `false` Wert geführt haben.

In Listing 11.7 schreiben wir eine Funktion namens `add_two` , die `2` zu ihrem Parameter hinzufügt, dann testen wir diese Funktion mit Hilfe von `assert_eq!` Makro.

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-07/src/lib.rs}}
```

<span class="caption">Listing 11-7: Testen der Funktion <code>add_two</code> mit dem <code>assert_eq!</code> Makro</span>

Lassen Sie uns überprüfen, ob es besteht!

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-07/output.txt}}
```

Wir übergeben `4` als Argument an `assert_eq!` , was dem Ergebnis des Aufrufs von `add_two(2)` . Die Zeile für diesen Test lautet `test tests::it_adds_two ... ok` , und der Text `ok` zeigt an, dass unser Test bestanden wurde!

Lassen Sie uns einen Fehler in unseren Code einführen, um zu sehen, was `assert_eq!` sieht aus wie wenn es fehlschlägt. Ändern Sie die Implementierung der Funktion `add_two` , um stattdessen `3` hinzuzufügen:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-04-bug-in-add-two/src/lib.rs:here}}
```

Führen Sie die Tests erneut aus:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-04-bug-in-add-two/output.txt}}
```

Unser Test hat den Fehler erwischt! Der `it_adds_two` -Test ist fehlgeschlagen, und die Nachricht teilt uns mit, dass die `assertion failed: `(left == right)`` und was die `left` und `right` Werte sind. Diese Nachricht hilft uns, mit dem Debuggen zu beginnen: Das `left` Argument war `4` , aber das `right` Argument, wo wir `add_two(2)` hatten, war `5` . Sie können sich vorstellen, dass dies besonders hilfreich wäre, wenn wir viele Tests durchführen.

Beachten Sie, dass in einigen Sprachen und Test-Frameworks die Parameter für Gleichheitszusicherungsfunktionen als " `expected` " und " `actual` " bezeichnet werden und die Reihenfolge, in der wir die Argumente angeben, von Bedeutung ist. In Rust heißen sie jedoch `left` und `right` , und die Reihenfolge, in der wir den erwarteten Wert und den vom Code erzeugten Wert angeben, spielt keine Rolle. Wir könnten die Behauptung in diesem Test als `assert_eq!(add_two(2), 4)` schreiben, was zu derselben Fehlermeldung führen würde, die anzeigt, dass die `assertion failed: `(left == right)`` .

Die `assert_ne!` Makro wird passieren, wenn die beiden Werte, die wir ihm geben, nicht gleich sind und fehlschlagen, wenn sie gleich sind. Dieses Makro ist am nützlichsten für Fälle, in denen wir nicht sicher sind, was ein Wert sein *wird* , aber wir wissen, was der Wert definitiv *nicht* sein sollte. Wenn wir zum Beispiel eine Funktion testen, die ihre Eingabe garantiert auf irgendeine Weise ändert, die Art und Weise, in der die Eingabe geändert wird, jedoch vom Wochentag abhängt, an dem wir unsere Tests durchführen, ist das Beste, was wir behaupten könnten dass die Ausgabe der Funktion nicht gleich der Eingabe ist.

Unter der Oberfläche ist das `assert_eq!` und `assert_ne!` Makros verwenden die Operatoren `==` bzw. `!=` . Wenn die Zusicherungen fehlschlagen, geben diese Makros ihre Argumente unter Verwendung der Debug-Formatierung aus, was bedeutet, dass die verglichenen Werte die `PartialEq` und `Debug` -Merkmale implementieren müssen. Alle primitiven Typen und die meisten Standardbibliothekstypen implementieren diese Merkmale. Für Strukturen und Aufzählungen, die Sie selbst definieren, müssen Sie `PartialEq` implementieren, um die Gleichheit dieser Typen zu bestätigen. Sie müssen auch `Debug` implementieren, um die Werte auszugeben, wenn die Assertion fehlschlägt. Da es sich bei beiden Merkmalen um ableitbare Merkmale handelt, wie in Listing 5.12 in Kapitel 5 erwähnt, ist dies normalerweise so einfach wie das Hinzufügen der Annotation `#[derive(PartialEq, Debug)]` zu Ihrer Struct- oder Enum-Definition. Siehe Anhang C, [„Ableitbare Merkmale“,]<!-- ignorieren --> für weitere Einzelheiten zu diesen und anderen ableitbaren Merkmalen.

### Hinzufügen benutzerdefinierter Fehlermeldungen

Sie können auch eine benutzerdefinierte Nachricht hinzufügen, die mit der Fehlermeldung als optionale Argumente zum `assert!` , `assert_eq!` , und `assert_ne!` Makros. Alle nach den erforderlichen Argumenten angegebenen Argumente werden an das `format!` Makro (besprochen in Kapitel 8 im <a href="ch08-02-strings.html#concatenation-with-the--operator-or-the-format-macro" data-md-type="link">Abschnitt „Verkettung mit dem <code data-md-type="codespan">+</code> -Operator oder dem `format!` -Makro“</a><!-- ignorieren --> Abschnitt), sodass Sie eine Formatzeichenfolge übergeben können, die `{}` Platzhalter und Werte enthält, die in diese Platzhalter eingefügt werden sollen. Benutzerdefinierte Nachrichten sind nützlich, um zu dokumentieren, was eine Behauptung bedeutet; Wenn ein Test fehlschlägt, haben Sie eine bessere Vorstellung davon, was das Problem mit dem Code ist.

Angenommen, wir haben eine Funktion, die Personen mit Namen begrüßt, und wir möchten testen, ob der Name, den wir an die Funktion übergeben, in der Ausgabe erscheint:

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-05-greeter/src/lib.rs}}
```

Die Anforderungen für dieses Programm wurden noch nicht vereinbart, und wir sind uns ziemlich sicher, dass sich der `Hello` -Text am Anfang der Begrüßung ändern wird. Wir haben entschieden, dass wir den Test nicht aktualisieren müssen, wenn sich die Anforderungen ändern. Anstatt auf exakte Gleichheit mit dem von der `greeting` zurückgegebenen Wert zu prüfen, behaupten wir einfach, dass die Ausgabe den Text des Eingabeparameters enthält.

Lassen Sie uns nun einen Fehler in diesen Code einführen, indem wir die `greeting` ändern, um den `name` auszuschließen, um zu sehen, wie der standardmäßige Testfehler aussieht:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-06-greeter-with-bug/src/lib.rs:here}}
```

Das Ausführen dieses Tests ergibt Folgendes:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-06-greeter-with-bug/output.txt}}
```

Dieses Ergebnis zeigt nur an, dass die Behauptung fehlgeschlagen ist und in welcher Zeile sich die Behauptung befindet. Eine nützlichere Fehlermeldung würde den Wert aus der `greeting` drucken. Lassen Sie uns eine benutzerdefinierte Fehlermeldung hinzufügen, die aus einer Formatzeichenfolge mit einem Platzhalter besteht, der mit dem tatsächlichen Wert gefüllt ist, den wir von der `greeting` erhalten haben:

```rust,ignore
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-07-custom-failure-message/src/lib.rs:here}}
```

Wenn wir jetzt den Test ausführen, erhalten wir eine informativere Fehlermeldung:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-07-custom-failure-message/output.txt}}
```

Wir können den Wert sehen, den wir tatsächlich in der Testausgabe erhalten haben, was uns helfen würde, zu debuggen, was passiert ist, anstatt was wir erwartet haben.

### Auf Panics prüfen mit `should_panic`

Neben der Überprüfung der Rückgabewerte ist es wichtig zu überprüfen, ob unser Code Fehlerbedingungen wie erwartet behandelt. Betrachten Sie zum Beispiel den `Guess` -Typ, den wir in Kapitel 9, Listing 9-13 erstellt haben. Anderer Code, der `Guess` verwendet, hängt von der Garantie ab, dass `Guess` -Instanzen nur Werte zwischen 1 und 100 enthalten. Wir können einen Test schreiben, der sicherstellt, dass der Versuch, eine `Guess` -Instanz mit einem Wert außerhalb dieses Bereichs zu erstellen, in Panik gerät.

Dazu fügen wir unserer Testfunktion das Attribut `should_panic` hinzu. Der Test wird bestanden, wenn der Code innerhalb der Funktion in Panik gerät; Der Test schlägt fehl, wenn der Code innerhalb der Funktion nicht in Panik gerät.

Listing 11.8 zeigt einen Test, der überprüft, ob die Fehlerbedingungen von `Guess::new` auftreten, wenn wir es erwarten.

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-08/src/lib.rs}}
```

<span class="caption">Listing 11-8: Testen, ob eine Bedingung eine <code>panic!</code></span>

Wir platzieren das Attribut `#[should_panic]` nach dem Attribut `#[test]` und vor der Testfunktion, auf die es sich bezieht. Schauen wir uns das Ergebnis an, wenn dieser Test bestanden ist:

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-08/output.txt}}
```

Sieht gut aus! Lassen Sie uns nun einen Fehler in unseren Code einführen, indem wir die Bedingung entfernen, dass die `new` Funktion in Panik gerät, wenn der Wert größer als 100 ist:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-08-guess-with-bug/src/lib.rs:here}}
```

Wenn wir den Test in Listing 11.8 ausführen, schlägt er fehl:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-08-guess-with-bug/output.txt}}
```

Wir erhalten in diesem Fall keine sehr hilfreiche Nachricht, aber wenn wir uns die Testfunktion ansehen, sehen wir, dass sie mit `#[should_panic]` kommentiert ist. Der Fehler, den wir erhalten haben, bedeutet, dass der Code in der Testfunktion keine Panik verursacht hat.

Tests, die `should_panic` verwenden, können ungenau sein. Ein `should_panic` -Test würde auch dann bestanden, wenn der Test aus einem anderen als dem erwarteten Grund panisch wird. Um `should_panic` Tests präziser zu machen, können wir einen optionalen `expected` Parameter zum `should_panic` Attribut hinzufügen. Der Testrahmen stellt sicher, dass die Fehlermeldung den bereitgestellten Text enthält. Betrachten Sie zum Beispiel den modifizierten Code für `Guess` in Listing 11.9, wo die `new` Funktion mit unterschiedlichen Meldungen abstürzt, je nachdem, ob der Wert zu klein oder zu groß ist.

<span class="filename">Dateiname: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-09/src/lib.rs:here}}
```

<span class="caption">Listing 11-9: Test auf <code>panic!</code> mit einer bestimmten Panikmeldung</span>

Dieser Test wird bestanden, weil der Wert, den wir in den `expected` Parameter des Attributs `should_panic` setzen, ein Teilstring der Nachricht ist, mit der die `Guess::new` -Funktion in Panik gerät. Wir hätten die gesamte von uns erwartete Panikmeldung angeben können, was in diesem Fall `Guess value must be less than or equal to 100, got 200.` wäre. Was Sie angeben, hängt davon ab, wie viel der Panikmeldung einzigartig oder dynamisch ist wie genau der Test sein soll. In diesem Fall reicht ein Teilstring der Panikmeldung aus, um sicherzustellen, dass der Code in der Testfunktion den Fall `else if value > 100` ausführt.

Um zu sehen, was passiert, wenn ein `should_panic` -Test mit einer `expected` Nachricht fehlschlägt, fügen wir erneut einen Fehler in unseren Code ein, indem wir die Körper der `if value < 1` und der `else if value > 100` -Blöcke vertauschen:

```rust,ignore,not_desired_behavior
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-09-guess-with-panic-msg-bug/src/lib.rs:here}}
```

Wenn wir dieses Mal den `should_panic` -Test ausführen, schlägt er fehl:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-09-guess-with-panic-msg-bug/output.txt}}
```

Die Fehlermeldung weist darauf hin, dass bei diesem Test tatsächlich wie erwartet eine Panik aufgetreten ist, die Panikmeldung jedoch nicht die erwartete Zeichenfolge `'Guess value must be less than or equal to 100'` enthielt. Die Panikmeldung, die wir in diesem Fall erhalten haben, war `Guess value must be greater than or equal to 1, got 200.` Jetzt können wir anfangen, herauszufinden, wo unser Fehler ist!

### Verwenden von `Result<T, E>` in Tests

Unsere bisherigen Tests geraten alle in Panik, wenn sie fehlschlagen. Wir können auch Tests schreiben, die `Result<T, E>` verwenden! Hier ist der Test aus Listing 11.1, neu geschrieben, um `Result<T, E>` zu verwenden und ein `Err` zurückzugeben, anstatt in Panik zu verfallen:

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-10-result-in-tests/src/lib.rs}}
```

Die Funktion `it_works` hat jetzt den Rückgabetyp `Result<(), String>` . Im Hauptteil der Funktion anstelle des Aufrufs von `assert_eq!` Makro geben wir `Ok(())` zurück, wenn der Test bestanden wird, und ein `Err` mit einem darin enthaltenen `String` , wenn der Test fehlschlägt.

Tests so zu schreiben, dass sie ein `Result<T, E>` zurückgeben, ermöglicht es Ihnen, den Fragezeichenoperator im Hauptteil von Tests zu verwenden, was eine bequeme Möglichkeit sein kann, Tests zu schreiben, die fehlschlagen sollten, wenn eine Operation darin eine `Err` -Variante zurückgibt.

Sie können die Annotation `#[should_panic]` nicht für Tests verwenden, die `Result<T, E>` verwenden. Um zu bestätigen, dass eine Operation eine `Err` -Variante zurückgibt, verwenden Sie den Fragezeichenoperator *nicht* für den Wert `Result<T, E>` . Verwenden Sie stattdessen `assert!(value.is_err())` .

Nachdem Sie nun mehrere Möglichkeiten zum Schreiben von Tests kennen, wollen wir uns ansehen, was passiert, wenn wir unsere Tests ausführen, und die verschiedenen Optionen untersuchen, die wir mit `cargo test` verwenden können.


[der Dokumentation zu Benchmark-Tests]: ../unstable-book/library-features/test.html
[Abschnitt „Ignorieren einiger Tests, sofern nicht ausdrücklich angefordert“]: ch11-02-running-tests.html#ignoring-some-tests-unless-specifically-requested
[Abschnitt „Ausführen einer Teilmenge von Tests nach Namen“.]: ch11-02-running-tests.html#running-a-subset-of-tests-by-name
[„Ableitbare Merkmale“,]: appendix-03-derivable-traits.html
[„Dokumentationskommentare als Tests“]: ch14-02-publishing-to-crates-io.html#documentation-comments-as-tests
[„Pfade zum Verweisen auf ein Element in der Modulstruktur“]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html