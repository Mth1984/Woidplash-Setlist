# Woidplash Setlist

Live-Setlist-Web-App für die Band **Woidplash** — pro Song Tonart, Tuning, BPM, Akkord-Struktur und Solo-Lage aus Gitarrist-Sicht. Gebaut für die Bühne: dunkles Layout, große Schrift, Akkorde in Rot, und das schnelle Umschalten neben dem In-Ear-Mixer (Soundcraft Ui24R per `ui.io` im Split-View).

## Features

- **2 Sets × 15 Songs**, jeder Song mit eigener Karte: Akkord-Struktur, Solo-Lage, BPM, Tonart, Tuning
- **Tuning-Wechsel-Marker** automatisch zwischen den Songs berechnet:
  - 🔴 *Hin* zum Spezial-Tuning (rot)
  - 🟠 *Zurück* auf E-Standard (Amber, laut) — damit das Zurückstimmen nicht verpennt wird
- **Vorausschau** auf jeder Song-Karte: warnt vor dem kommenden Tuning-Wechsel
- **Offline-tauglich** (PWA mit Service-Worker-Cache) — läuft auf der Bühne ohne Internet
- **Installierbar** als Homescreen-App auf dem iPad

## Nutzung

**Auf dem iPad (empfohlen):** Die GitHub-Pages-URL in Safari öffnen → Teilen → **„Zum Home-Bildschirm"**. Danach startet die App als Vollbild-Kachel und läuft offline.

**Lokal:** `index.html` in einem Browser öffnen. (Auf dem iPad öffnet Safari keine lokalen HTML-Dateien mehr — dafür z.B. „Documents by Readdle" nutzen.)

## Songs bearbeiten

Alle Songdaten stehen im `SONGS`-Array in [`index.html`](index.html). Pro Song:

```js
{ s:1, n:1, t:"Titel", a:"Interpret", key:"Tonart", tune:"E", bpm:"120",
  sec:[["Abschnitt","Akkorde"]], solo:"Solo-Lage", note:"optional", tx:"Tuning-Detail", segue:"optional" }
```

`tune` akzeptiert `"E"`, `"D-Std"` oder `"Drop D"` — die Wechsel-Marker werden automatisch daraus erzeugt. Beim Ändern der App-Version die `CACHE`-Konstante in `sw.js` hochzählen (z.B. `-v2`), damit das iPad die neue Version lädt.

## Stack

Eine einzelne `index.html` (kein Build, keine Dependencies), System-Fonts, Vanilla JS. PWA über `manifest.webmanifest` + `sw.js`. Hosting via GitHub Pages.
