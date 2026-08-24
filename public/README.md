# Pfadizentrum Drei Gleichen — Hugo

1:1-Nachbau des WordPress-Auftritts **www.pfadizentrum.de** als statische Hugo-Site.

Original: WordPress 6.8.2, Theme *Rife Free* 2.4.21 (Apollo13 Framework).
Das Design wurde nicht nachempfunden, sondern das gerenderte Markup und die
Original-Assets des Themes wurden übernommen — die Ausgabe ist pixelgleich.

## Bauen

```bash
hugo server          # Vorschau auf http://localhost:1313
hugo                 # Produktions-Build nach ./public
```

Benötigt Hugo **extended** ≥ 0.146 (entwickelt/getestet mit 0.148.2).
Vor dem Deployment `baseURL` in `hugo.toml` prüfen.

## Aufbau

```
hugo.toml                     Menü, Socials, Footer-Widgets, ApolloParams
content/                      Seiteninhalte als HTML mit JSON-Frontmatter
  _index.html                 Willkommen (/)
  platz-gebaeude.html         Platz & Gebäude
  die-region.html             Die Region
  preise.html                 Preise & Belegung
  verein.html                 Verein
  kontakt.html                Kontakt (inkl. Impressum & Datenschutz)
layouts/
  baseof.html                 Grundgerüst (Preloader, Header, #mid, Footer)
  home.html / page.html       Inhaltsblock
  404.html                    Fehlerseite im Theme-Layout
  partials/                   head, header, footer, socials, scripts
static/wp-content/            Theme-CSS/JS/Fonts + alle Uploads (Bilder)
static/wp-includes/           jQuery, WordPress Block-Library-CSS
```

Die WordPress-Pfade (`/wp-content/...`) wurden bewusst beibehalten, damit die
CSS-internen `url()`-Referenzen des Themes unverändert funktionieren und alte
Deep-Links auf Bilder weiter gültig bleiben.

## Frontmatter pro Seite

| Feld          | Bedeutung                                              |
|---------------|--------------------------------------------------------|
| `title`       | `<title>` der Seite                                     |
| `pageTitle`   | Überschrift in der Title-Bar (darf HTML enthalten)      |
| `postId`      | WordPress-Post-ID, hält die CSS-Klassen `post-<id>`     |
| `bodyClass`   | Original-Klassen am `<body>`                            |
| `titleBarBg`  | Hintergrundbild der Title-Bar                           |
| `extraCSS`    | seitenspezifisches Inline-CSS aus WordPress             |

## Inhalte pflegen

Die Seiten sind HTML-Dateien; der Inhalt ist das Markup aus dem WordPress-
Editor (Gutenberg-Blockklassen). Text lässt sich direkt bearbeiten. Neue Bilder
gehören nach `static/wp-content/uploads/`.

## Bewusste Abweichungen vom Original

Rein WordPress-spezifisch, ohne Einfluss auf die Darstellung:

- entfernt: `wp-json`/REST-Links, `xmlrpc.php`, oEmbed-, Shortlink- und
  WordPress-Generator-Meta, Emoji-Detection-Skript, Speculation-Rules
- Feed: `/index.xml` (Hugo) statt `/feed/`; der Kommentar-Feed entfällt
- Bild-`src`-Attribute, die im Original auf das nicht mehr befüllte
  `www2.pfadfinderzentrum.org` zeigten (und dort eine HTML-Fehlerseite
  lieferten), verweisen jetzt auf die lokalen Dateien
- alle Verweise sind root-relativ statt absolut auf die Domain

## Verifikation

- DOM-Vergleich gegen die Live-Site: identisch bis auf die oben genannten Punkte
- Alle 188 Asset-Dateien byte-identisch mit dem Live-Server geprüft
- 174 interne Referenzen im Build aufgelöst, keine toten Links
- Pixelvergleich (1440 px und 390 px Breite, ganze Seiten): 0 abweichende Pixel
  auf allen Seiten inkl. 404. Restdifferenzen zur Live-Site auf `/` und
  `/platz-gebaeude/` sind Aufnahme-Artefakte (animiertes GIF bzw. Lazy-Load-
  Timing) — nachgewiesen, indem das Original-WordPress-HTML lokal mit denselben
  Assets ausgeliefert und verglichen wurde: 0 abweichende Pixel.
