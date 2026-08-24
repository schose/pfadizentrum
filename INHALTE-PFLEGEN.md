# Inhalte pflegen

Alle Seiten liegen als Markdown-Dateien im Ordner `content/`. Mehr als Markdown
und eine Handvoll Bausteine wird nicht gebraucht — HTML muss niemand anfassen.

| Datei | Seite |
| --- | --- |
| `content/_index.md` | Startseite (WILLKOMMEN) |
| `content/platz-gebaeude.md` | PLATZ & GEBÄUDE |
| `content/die-region.md` | DIE REGION |
| `content/preise.md` | PREISE & BELEGUNG |
| `content/verein.md` | VEREIN |
| `content/kontakt.md` | KONTAKT |

Nach dem Speichern und Hochladen (Commit auf den Branch) baut GitHub die Seite
automatisch neu. Das dauert ein bis zwei Minuten.

---

## Der Kopf jeder Datei

Ganz oben in jeder Datei stehen zwischen zwei Zeilen aus drei Strichen ein paar
Angaben zur Seite:

```yaml
---
title: "PREISE & BELEGUNG"
menu:
  main:
    weight: 40
---
```

* `title` — Überschrift im Bild-Banner **und** Name im Menü.
* `weight` — Position im Menü. Kleinere Zahl heißt weiter links.

Eine **neue Seite** anlegen: neue Datei `content/oeffnungszeiten.md` mit so einem
Kopf erstellen. Sie ist danach unter `/oeffnungszeiten/` erreichbar und steht im
Menü. Soll sie **nicht** ins Menü, lässt man den `menu:`-Block einfach weg.

---

## Normaler Text

Ganz normales Markdown:

```markdown
**fett**   *kursiv*   ***fett und kursiv***

Ein Absatz. Eine Leerzeile trennt Absätze.
Ein einfacher Zeilenumbruch bleibt ein Zeilenumbruch.

[Text des Links](https://beispiel.de)

- Ein Aufzählungspunkt
- Noch einer
```

Links auf andere Seiten dieser Website mit führendem Schrägstrich schreiben:
`[Unser Verein](/verein/)`. Externe Links öffnen automatisch in einem neuen Tab,
interne nicht.

---

## Bilder

**Eine** Bilddatei in `static/wp-content/uploads/JAHR/MONAT/` ablegen — am besten
in voller Größe. Die kleineren Fassungen für Handys erzeugt die Website selbst.

```markdown
![Blick über den Zeltplatz](2024/05/zeltplatz.jpg)
![Blick über den Zeltplatz](2024/05/zeltplatz.jpg "Unterschrift unter dem Bild")
```

* In den eckigen Klammern steht der **Alternativtext**. Den lesen blinde
  Besucherinnen und Besucher vor — bitte immer ausfüllen.
* In Anführungszeichen dahinter steht die sichtbare **Bildunterschrift**
  (optional).
* Der Pfad ist immer relativ zu `static/wp-content/uploads/`.

Fehlt eine Bilddatei, meldet das der Build mit einer Warnung samt Dateiname.

---

## Bausteine

Für alles, was Markdown nicht kann, gibt es sechs Bausteine. Sie stehen in
doppelten geschweiften Klammern.

### Abschnittsüberschrift

```markdown
{{< titel kicker="Faire Preise für Gruppen" >}}Preisliste{{< /titel >}}
```

`kicker` ist die kleine Zeile darüber und kann weggelassen werden.
Für eine zentrierte Überschrift mit Trennlinie (wie auf der Startseite):

```markdown
{{< titel zentriert="true" kicker="Bei uns zu Hause" >}}Unsere Partner{{< /titel >}}
```

### Zwei Spalten: links Text, rechts Bilder

```markdown
{{< abschnitt >}}
{{% text %}}
{{< titel kicker="4500 m² für Outdooraktivitäten" >}}Der Zeltplatz{{< /titel >}}

Hier steht der Fließtext, ganz normal in Markdown.
{{% /text %}}
{{% medien %}}
![Der Zeltplatz von oben](2024/05/platz1.jpg)

![Die Jurtenburg](2024/05/jurtenburg.jpg "Jurtenburg")
{{% /medien %}}
{{< /abschnitt >}}
```

Zwischen zwei Bildern gehört eine **Leerzeile**.

Ein Abschnitt ohne Bilder braucht das Ganze nicht — dann einfach `{{< titel >}}`
und darunter den Text schreiben.

> Merkhilfe: `{{< … >}}` mit spitzer Klammer sind Bausteine, `{{% … %}}` mit
> Prozentzeichen umschließen Text, in dem weiter Markdown gilt.

### Logo-Raster (Partner, Förderer)

```markdown
{{< logos >}}
{{< logo src="2024/05/na.png" url="https://www.nesse-apfelstaedt.de" >}}Gemeinde Nesse-Apfelstädt{{< /logo >}}
{{< logo src="2024/05/gth.png" url="https://www.landkreis-gotha.de" >}}Landkreis Gotha{{< /logo >}}
{{< /logos >}}
```

Eine neue Partnerorganisation ist genau **eine zusätzliche Zeile**. Das Raster
bricht von selbst um, Zeilen müssen nicht abgezählt werden. `url` ist optional.

### Kalender, Karte, Video einbetten

```markdown
{{< einbetten hoehe="800" src="https://calendar.google.com/calendar/embed?…" >}}
```

Die Adresse bekommt man beim jeweiligen Dienst über „Einbetten“ bzw. „Teilen“ —
dort den Teil aus `src="…"` kopieren.

---

## Aussehen ändern

Farben, Abstände und Rahmen der Bausteine stehen gesammelt in
`static/css/pfadizentrum.css`. Ganz oben in der Datei liegen zwei Stellschrauben:

```css
--pz-akzent: currentColor;   /* Farbe der kleinen Überschriftenzeile */
--pz-linie: #555;            /* Farbe des senkrechten Strichs davor */
```

Im WordPress-Original war als Akzentfarbe `var(--wp--preset--color--primary)`
eingetragen. Diese Variable war nie definiert, die Farbe blieb deshalb ohne
Wirkung. Wer die Akzentfarbe des Themes möchte, trägt `#3957ff` ein.

---

## Wo liegt was?

| Ordner | Inhalt |
| --- | --- |
| `content/` | die Seitentexte — hier wird gepflegt |
| `static/wp-content/uploads/` | Bilder |
| `static/css/pfadizentrum.css` | Aussehen der Bausteine |
| `layouts/shortcodes/` | die Bausteine selbst (technisch) |
| `layouts/` | Seitengerüst, Kopf- und Fußzeile (technisch) |

## Lokale Vorschau

Wer Hugo installiert hat, sieht Änderungen sofort im Browser:

```bash
hugo server
```

Danach <http://localhost:1313/> aufrufen.
