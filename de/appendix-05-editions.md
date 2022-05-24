## Anhang E - Ausgaben

In Kapitel 1 haben Sie gesehen, dass `cargo new` Ihrer *Cargo.toml* -Datei ein paar Metadaten über eine Edition hinzufügt. In diesem Anhang erfahren Sie, was das bedeutet!

Die Rust-Sprache und der Compiler haben einen sechswöchigen Veröffentlichungszyklus, was bedeutet, dass Benutzer einen konstanten Strom neuer Funktionen erhalten. Andere Programmiersprachen veröffentlichen seltener größere Änderungen; Rust veröffentlicht häufiger kleinere Updates. Nach einer Weile summieren sich all diese kleinen Änderungen. Aber von Release zu Release kann es schwierig sein, zurückzublicken und zu sagen: „Wow, zwischen Rust 1.10 und Rust 1.31 hat sich bei Rust viel verändert!“

Alle zwei bis drei Jahre produziert das Rust-Team eine neue Rust- *Edition* . Jede Ausgabe vereint die Funktionen, die in einem übersichtlichen Paket mit vollständig aktualisierter Dokumentation und Tools gelandet sind. Neue Editionen werden im Rahmen des üblichen sechswöchigen Veröffentlichungsprozesses ausgeliefert.

Editionen dienen verschiedenen Zwecken für verschiedene Personen:

- Für aktive Rust-Anwender fasst eine Neuauflage inkrementelle Änderungen in einem leicht verständlichen Paket zusammen.
- Für Nicht-Benutzer signalisiert eine neue Ausgabe, dass einige wichtige Fortschritte gelandet sind, die einen weiteren Blick auf Rust wert machen könnten.
- Für diejenigen, die Rust entwickeln, bietet eine neue Ausgabe einen Sammelpunkt für das Projekt als Ganzes.

Zum Zeitpunkt der Erstellung dieses Artikels sind drei Rust-Editionen verfügbar: Rust 2015, Rust 2018 und Rust 2021. Dieses Buch wurde unter Verwendung der Redewendungen der Rust 2021-Edition geschrieben.

Der `edition` in *Cargo.toml* gibt an, welche Edition der Compiler für Ihren Code verwenden soll. Wenn der Schlüssel nicht existiert, verwendet Rust aus Gründen der Abwärtskompatibilität `2015` als Editionswert.

Jedes Projekt kann sich für eine andere Edition als die Standardversion 2015 entscheiden. Editionen können inkompatible Änderungen enthalten, z. B. das Einfügen eines neuen Schlüsselworts, das mit Bezeichnern im Code in Konflikt steht. Wenn Sie sich jedoch nicht für diese Änderungen entscheiden, wird Ihr Code auch dann weiter kompiliert, wenn Sie die von Ihnen verwendete Rust-Compiler-Version aktualisieren.

Alle Rust-Compiler-Versionen unterstützen jede Edition, die vor der Veröffentlichung dieses Compilers existierte, und sie können Kisten aller unterstützten Editionen miteinander verknüpfen. Editionsänderungen wirken sich nur darauf aus, wie der Compiler den Code anfänglich analysiert. Wenn Sie also Rust 2015 verwenden und eine Ihrer Abhängigkeiten Rust 2018 verwendet, wird Ihr Projekt kompiliert und kann diese Abhängigkeit verwenden. Die umgekehrte Situation, in der Ihr Projekt Rust 2018 und eine Abhängigkeit Rust 2015 verwendet, funktioniert ebenfalls.

Um es klar zu sagen: Die meisten Funktionen werden in allen Editionen verfügbar sein. Entwickler, die eine beliebige Rust-Edition verwenden, werden weiterhin Verbesserungen sehen, wenn neue stabile Versionen veröffentlicht werden. In einigen Fällen, hauptsächlich wenn neue Schlüsselwörter hinzugefügt werden, sind einige neue Funktionen jedoch möglicherweise erst in späteren Ausgaben verfügbar. Sie müssen die Edition wechseln, wenn Sie solche Funktionen nutzen möchten.

Weitere Einzelheiten finden Sie im [*Edition Guide*](https://doc.rust-lang.org/stable/edition-guide/) , einem vollständigen Buch über Editionen, das die Unterschiede zwischen den Editionen auflistet und erklärt, wie Sie Ihren Code automatisch über `cargo fix` auf eine neue Edition aktualisieren.
