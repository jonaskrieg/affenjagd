# Affenjagd

Fangis auf einem Baum. Zwei Pixel-Affen jagen sich einen Baum hoch und runter —
gedacht fürs iPhone, einhändig im Hochformat, und per Link in WhatsApp teilbar.

## Regeln

Einer der beiden Affen ist **„es"** und muss den anderen fangen. Bei Berührung
wechselt die Rolle. **Punkte gibt es nur, solange du nicht „es" bist** — 10 pro
Sekunde in Freiheit, plus 25 dafür, dass du dich losfängst. Eine Runde dauert
60 Sekunden, der Rekord bleibt auf dem Gerät gespeichert.

## Steuerung

Daumen irgendwo auf den Bildschirm legen und ziehen — der Steuerknüppel
erscheint dort, wo du hintippst.

| Eingabe | Wirkung |
|---|---|
| links / rechts | laufen |
| hoch an einer Liane | klettern |
| hoch auf einem Ast | springen |
| runter an einer Liane | herunterklettern |
| runter auf einem Ast | durchfallen lassen (der schnelle Fluchtweg) |
| Knopf unten rechts | Item werfen |

Ein zweiter Finger irgendwo auf dem Bildschirm wirft das Item ebenfalls.
Am Rechner: Pfeiltasten oder WASD, Leertaste fürs Item, Enter zum Starten.

## Die beiden Items

Jeder Affe trägt sein eigenes Item sichtbar vor sich her. Ist die Hand leer,
lädt es nach — das ist gleichzeitig die Anzeige.

- **Banane** (brauner Affe): der andere rutscht aus und verliert für einen
  Moment die Kontrolle. Gut, um jemanden über sein Ziel hinausschießen zu lassen.
- **Weisse Blume** (grauer Affe): der andere bleibt stehen und schnuppert,
  weil es so gut riecht. Bringt Distanz.

Das eigene Item schadet einem selbst nicht.

## Aufbau

Alles steckt in `index.html` — kein Build, keine Abhängigkeiten, keine externen
Dateien. Die Grafik sind 16×16-Sprites, die als Zeichenraster direkt im Code
liegen und ohne Kantenglättung hochskaliert werden. Der Baum wird beim Start aus
einem festen Startwert erzeugt, ist also in jeder Runde gleich, und passt seine
Höhe an das Seitenverhältnis des Geräts an.

Der Gegner findet seinen Weg über einen Navigationsgraphen: Äste erlauben
Seitwärtsschritte, Lianen Schritte nach oben, und nach unten geht es überall.
Eine Breitensuche vom Spieler aus liefert das Entfernungsfeld, dem der Gegner
folgt — bergab, wenn er jagt, bergauf, wenn er flieht.

`icon.png` wird aus dem Sprite in `index.html` erzeugt und dient als Symbol für
„Zum Home-Bildschirm" sowie als Bild der Linkvorschau.

## Veröffentlichen

GitHub Pages auf `main` / Wurzelverzeichnis stellen. Danach in `index.html` bei
`og:image` die vollständige Adresse eintragen, damit WhatsApp die Vorschau
anzeigt:

```html
<meta property="og:image" content="https://BENUTZER.github.io/REPO/icon.png">
```
