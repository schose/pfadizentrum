# Pfadizentrum Drei Gleichen — Hugo

Statische Hugo-Site für **www.pfadizentrum.de**, hervorgegangen aus dem
WordPress-Auftritt (WordPress 6.8.2, Theme *Rife Free* 2.4.21 / Apollo13
Framework). Das Design wurde nicht nachempfunden, sondern die Original-Assets
des Themes wurden übernommen — die Darstellung entspricht dem Original.

**Inhalte pflegen? → [INHALTE-PFLEGEN.md](INHALTE-PFLEGEN.md)**

## Bauen

```bash
hugo server          # Vorschau auf http://localhost:1313
hugo                 # Produktions-Build nach ./public
```

Benötigt Hugo **extended** ≥ 0.146 (entwickelt/getestet mit 0.165.0).
Das Deployment nach GitHub Pages erledigt `.github/workflows/hugo.yaml`; die
`baseURL` setzt der Workflow selbst auf die Pages-Adresse.

## Aufbau

```
hugo.toml                     Socials, Footer-Widgets, ApolloParams, Mounts
content/                      Seiteninhalte als Markdown
  _index.md                   Willkommen (/)
  platz-gebaeude.md           Platz & Gebäude
  die-region.md               Die Region
  preise.md                   Preise & Belegung
  verein.md                   Verein
  kontakt.md                  Kontakt (inkl. Impressum & Datenschutz)
layouts/
  baseof.html                 Grundgerüst (Preloader, Header, #mid, Footer)
  all.html                    Inhaltsblock für Start- und Unterseiten
  404.html                    Fehlerseite im Theme-Layout
  partials/                   head, header, footer, socials, scripts, url
  shortcodes/                 die Inhaltsbausteine
  _markup/                    Render-Hooks für Bilder und Links
static/css/pfadizentrum.css   Aussehen der Inhaltsbausteine
static/wp-content/            Theme-CSS/JS/Fonts + alle Uploads (Bilder)
static/wp-includes/           jQuery, WordPress Block-Library-CSS
```

Die WordPress-Pfade (`/wp-content/...`) wurden bewusst beibehalten, damit die
CSS-internen `url()`-Referenzen des Themes unverändert funktionieren und alte
Deep-Links auf Bilder weiter gültig bleiben.

## Frontmatter pro Seite

| Feld         | Bedeutung                                                      |
|--------------|----------------------------------------------------------------|
| `title`      | Überschrift der Title-Bar, Menüname und Basis des `<title>`     |
| `menu.main.weight` | Position im Hauptmenü (weglassen = nicht im Menü)         |
| `titleBarBg` | *optional:* abweichendes Hintergrundbild der Title-Bar          |
| `extraCSS`   | *optional:* seitenspezifisches CSS                              |

Die Body-Klassen des Themes erzeugt `layouts/baseof.html`; sie stehen nicht
mehr im Frontmatter.

## Bilder

`static/wp-content/uploads/` ist zusätzlich als Hugo-Asset eingebunden
(`[module.mounts]` in `hugo.toml`). Der Render-Hook
`layouts/_markup/render-image.html` erzeugt daraus die responsiven Größen und
das `srcset` — abgelegt werden muss nur die Originaldatei.

Aus dem WordPress-Export stammen noch die dort erzeugten Größenvarianten
(`name-300x199.jpg` usw.). Sie werden nicht mehr gebraucht, bleiben aber
liegen, damit alte Deep-Links gültig bleiben.

## Bewusste Abweichungen vom Original

Rein WordPress-spezifisch, ohne Einfluss auf die Darstellung:

- entfernt: `wp-json`/REST-Links, `xmlrpc.php`, oEmbed-, Shortlink- und
  WordPress-Generator-Meta, Emoji-Detection-Skript, Speculation-Rules
- Feed: `/index.xml` (Hugo) statt `/feed/`; der Kommentar-Feed entfällt
- Bild-`src`-Attribute, die im Original auf das nicht mehr befüllte
  `www2.pfadfinderzentrum.org` zeigten (und dort eine HTML-Fehlerseite
  lieferten), verweisen jetzt auf die lokalen Dateien
- alle Verweise sind root-relativ statt absolut auf die Domain
- Gutenberg-Klassen ohne Stylesheet-Entsprechung (`section_head`,
  `services-item`, `wp-container-core-*` …) sind entfallen; die wenigen
  wirksamen Regeln stehen in `static/css/pfadizentrum.css`
- das Logo-Raster auf der Startseite bricht automatisch um, statt feste
  Zeilen zu haben — die Reihenaufteilung kann dadurch abweichen
- E-Mail-Adressen und URLs im Fließtext werden zu Links (Markdown-Autolink)
- die drei Spendenlinks auf der Kontaktseite öffnen jetzt wie alle anderen
  externen Links in einem neuen Tab

## Verifikation

Beim Umbau von WordPress-HTML auf Markdown (siehe Historie) wurde gegen einen
Referenz-Build des vorherigen Standes geprüft:

- **Textvergleich** aller sechs Seiten: keine Abweichung — kein Satz verloren,
  keiner hinzugekommen
- **Linkvergleich** aller sechs Seiten: identisch bis auf die oben genannten
  Spendenlinks und Autolinks
- 202 interne Referenzen im Build aufgelöst, keine toten Links, keine ohne
  Unterverzeichnis-Präfix
- Sichtprüfung im Browser gegen den Referenz-Build

Aus dem ursprünglichen WordPress-Nachbau:

- DOM-Vergleich gegen die Live-Site: identisch bis auf die genannten Punkte
- Alle 188 Asset-Dateien byte-identisch mit dem Live-Server geprüft
- Pixelvergleich (1440 px und 390 px Breite, ganze Seiten): 0 abweichende Pixel
