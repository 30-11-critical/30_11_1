Erste Kopfzeile | Zweite Kopfzeile
--- | ---
Inhaltszelle | Inhaltszelle
Inhaltszelle | Inhaltszelle

Erste Kopfzeile | Zweite Kopfzeile
--- | ---
Inhaltszelle | Inhaltszelle
Inhaltszelle | Inhaltszelle

![Zitadelle](https://vignette.wikia.nocookie.net/masseffect/images/d/d7/MassEffect2Citadel.jpg/revision/latest?cb=20100721191415)

Linksbündig | Zentriert ausgerichtet | Rechtsbündig
:-- | :-: | --:
Spalte 3 ist | etwas wortreicher Text | **$1600**
Spalte 2 ist | zentriert | $12
Zebrastreifen | sind ordentlich | ~~$1~~

Dillinger ist ein Cloud-fähiger, mobilfähiger, Offline-Speicher, AngularJS-basierter HTML5-Markdown-Editor.

- Geben Sie links etwas Markdown ein
- Siehe HTML rechts
- Magie

# wahr

- Importieren Sie eine HTML-Datei und beobachten Sie, wie sie auf magische Weise in Markdown konvertiert wird
- Bilder ziehen und ablegen (erfordert die Verknüpfung Ihres Dropbox-Kontos)

Du kannst auch:

- Importieren und speichern Sie Dateien von GitHub, Dropbox, Google Drive und One Drive
- Ziehen Sie Markdown- und HTML-Dateien per Drag-and-Drop in Dillinger
- Exportieren Sie Dokumente als Markdown, HTML und PDF

Markdown ist eine leichtgewichtige Auszeichnungssprache, die auf den Formatierungskonventionen basiert, die Menschen normalerweise in E-Mails verwenden. Wie [John Gruber] auf der [Markdown-Site][df1] schreibt

> Das übergeordnete Designziel für die Formatierungssyntax von Markdown besteht darin, sie so lesbar wie möglich zu machen. Die Idee ist, dass ein Markdown-formatiertes Dokument so wie es ist, als reiner Text veröffentlicht werden kann, ohne dass es so aussieht, als wäre es mit Tags oder Formatierungsanweisungen markiert worden.

Dieser Text, den Sie hier sehen, ist *tatsächlich* in Markdown geschrieben! Um ein Gefühl für die Syntax von Markdown zu bekommen, geben Sie etwas Text in das linke Fenster ein und sehen Sie sich die Ergebnisse rechts an.

### FALSCH

Dillinger verwendet eine Reihe von Open-Source-Projekten, um ordnungsgemäß zu funktionieren:

- [AngularJS] - HTML verbessert für Web-Apps!
- [Ace Editor] - fantastischer webbasierter Texteditor
- [markdown-it] – Markdown-Parser richtig gemacht. Schnell und einfach zu verlängern.
- [Twitter Bootstrap] – großartige UI-Boilerplate für moderne Web-Apps
- [node.js] – Evented I/O für das Backend
- [Express] – schnelles node.js Netzwerk-App-Framework [@tjholowaychuk]
- [Gulp] – das Streaming-Build-System
- [Breakdance](https://breakdance.github.io/breakdance/) - HTML-zu-Markdown-Konverter
- [jQuery] - haha

Und natürlich ist Dillinger selbst Open Source mit einem [öffentlichen Repository][dill] auf GitHub.

### Installation

![Ilos](https://avatars.mds.yandex.net/get-zen_doc/1937051/pub_5eb5dbf2ce86a8785f6dd6c7_5eb5df2f9f339d1166717b83/scale_1200)

Dillinger benötigt zur Ausführung [Node.js](https://nodejs.org/) v4+.

Installieren Sie die Abhängigkeiten und devDependencies und starten Sie den Server.

```sh
$ cd dillinger
$ npm install -d
$ node app
```

Für Produktionsumgebungen...

```sh
$ npm install --production
$ NODE_ENV=production node app
```

### Plugins

Dillinger wird derzeit um die folgenden Plugins erweitert. Anweisungen zur Verwendung in Ihrer eigenen Anwendung sind unten verlinkt.

Plugin | Liesmich
--- | ---
Dropbox | [plugins/dropbox/README.md][PlDb]
GitHub | [plugins/github/README.md][PlGh]
Google Drive | [plugins/googledrive/README.md][PlGd]
Eine Fahrt | [plugins/onedrive/README.md][PlOd]
Mittel | [plugins/medium/README.md][PlMe]
Google Analytics | [plugins/googleanalytics/README.md][PlGa]

### Entwicklung

Möchten Sie einen Beitrag leisten? Toll!

Dillinger verwendet Gulp + Webpack für eine schnelle Entwicklung. Nehmen Sie eine Änderung in Ihrer Datei vor und sehen Sie sofort Ihre Aktualisierungen!

Öffnen Sie Ihr bevorzugtes Terminal und führen Sie diese Befehle aus.

Erste Registerkarte:

```sh
$ node app
```

Zweiter Reiter:

```sh
$ gulp watch
```

(optional) Drittens:

```sh
$ karma test
```

#### Gebäude für Quelle

Zur Produktionsfreigabe:

```sh
$ gulp build --prod
```

Generieren vorgefertigter ZIP-Archive für die Verteilung:

```sh
$ gulp build dist --prod
```

### Docker

Dillinger lässt sich sehr einfach in einem Docker-Container installieren und bereitstellen.

Standardmäßig legt Docker Port 8080 offen, ändern Sie dies also bei Bedarf in der Dockerfile. Wenn Sie fertig sind, verwenden Sie einfach das Dockerfile, um das Image zu erstellen.

```sh
cd dillinger
docker build -t joemccann/dillinger:${package.json.version} .
```

Dadurch wird das Dillinger-Image erstellt und die erforderlichen Abhängigkeiten eingefügt. Stellen Sie sicher, dass `${package.json.version}` durch die aktuelle Version von Dillinger ersetzen.

Führen Sie anschließend das Docker-Image aus und ordnen Sie den Port dem gewünschten Port auf Ihrem Host zu. In diesem Beispiel ordnen wir einfach Port 8000 des Hosts dem Port 8080 des Docker zu (oder dem Port, der in der Docker-Datei verfügbar gemacht wurde):

```sh
docker run -d -p 8000:8080 --restart="always" <youruser>/dillinger:${package.json.version}
```

Überprüfen Sie die Bereitstellung, indem Sie in Ihrem bevorzugten Browser zu Ihrer Serveradresse navigieren.

```sh
127.0.0.1:8000
```

#### Kubernetes + Google Cloud

Siehe [Kubernetes.md](https://github.com/joemccann/dillinger/blob/master/KUBERNETES.md)

### Tods
