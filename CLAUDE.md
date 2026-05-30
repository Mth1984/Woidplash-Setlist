# CLAUDE.md — Woidplash Setlist

Anleitung für Claude (und Menschen), die an dieser App arbeiten — **auch vom Smartphone/Tablet beim Gig**. Lies das zuerst, dann bist du sofort produktiv.

## Was das ist

Statische **Setlist-PWA** für die Rock/Pop-Cover-Band **Woidplash**. Eine einzige `index.html` (kein Build-Step, keine Dependencies, keine Frameworks). Gehostet auf **GitHub Pages**, installierbar als Homescreen-App, **offline-tauglich** — läuft auf der Bühne auf dem iPad neben dem Soundcraft-Ui24R-Mixer (kein Internet dort).

Live: https://mth1984.github.io/Woidplash-Setlist/

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | **Die ganze App**: CSS + JS + das `SONGS`-Array (alle Songdaten). Songs werden HIER geändert. |
| `sw.js` | Service-Worker (Offline-Cache). Enthält die `CACHE`-Versionsnummer. |
| `manifest.webmanifest` | PWA-Metadaten (Name, Icons, Farben). |
| `icon-*.png` | App-Icons (rotes WP-Monogramm). |
| `_icon-src.html` | Quelle zum Neu-Rendern der Icons (selten gebraucht). |

## ⚠️ DIE WICHTIGSTE REGEL

**Nach JEDER Änderung an `index.html`: die `CACHE`-Konstante in `sw.js` um 1 hochzählen** (z.B. `woidplash-setlist-v3` → `v4`).

Sonst zeigt das iPad weiter die alte, gecachte Version — die Änderung käme auf der Bühne **nicht an**. Das ist der einzige echte Stolperstein dieser App. Immer mitmachen.

## Einen Song ändern / hinzufügen / entfernen

Im `SONGS`-Array in `index.html`. Schema pro Song:

```js
{
  s: 1,                 // Set: 1 oder 2
  n: 1,                 // Nummer innerhalb des Sets
  t: "Titel",           // Songtitel (Apostrophe ok, weil in doppelten Quotes)
  a: "Interpret",
  key: "E-Moll",        // Tonart, Freitext (z.B. "E-Moll (Refrain G-Dur)")
  tune: "E",            // NUR "E" | "D-Std" | "Drop D"  → steuert die Tuning-Marker
  tx: "Original Eb",    // OPTIONAL: Tuning-Detail
  bpm: "120",           // als String ("~140" erlaubt)
  segue: "⮕ Hey Jude",  // OPTIONAL: direkter Übergang zum nächsten Teil
  sec: [["Strophe","Em – C – G"],["Refrain","G – D – C"]],  // Abschnitte als [Name, Akkorde]-Paare
  solo: "E-Moll-Pentatonik · 12. Bund",  // Solo-Lage aus Gitarrist-Sicht
  note: "Hinweis"       // OPTIONAL: wird in grauer Box auf der Karte gezeigt
}
```

- **Reihenfolge ändern:** das `n`-Feld anpassen und die Objekte im Array entsprechend umsortieren.
- **Song hinzufügen:** ein neues Objekt an der richtigen Stelle einfügen, `s`/`n` setzen.
- Gültiges JS-Objekt halten: Kommas zwischen Feldern und Objekten, alle Strings in `"..."`.

### Akkord-Konvention: Griff-Namen, nicht transponiert

Akkorde immer so notieren, **wie sie gegriffen werden** — nicht wie sie durch das Tuning klingend heißen. Beispiel: in D-Standard (Ganzton runter) eine **A-Form** greifen → als `A` notieren, NICHT als `G` (der klingende Name). Mathias denkt beim Spielen in Griff-Formen; transponierte Klang-Namen verwirren. Gilt genauso für `key` (Griff-Lage angeben) und `solo` (Bund-Position wie gegriffen, z.B. „A-Moll-Pentatonik 5. Bund", auch wenn sie durch D-Standard tiefer klingt). Betrifft v.a. Kickstart My Heart (D-Standard). Bei Drop D (nur tiefe Saite) heißen die Powerchords ohnehin wie sie klingen.

## Tuning-Marker sind automatisch

Die roten („Umstimmen") und amber („Zurück auf E-Standard") Wechsel-Marker zwischen den Songs rechnet die App selbst aus dem `tune`-Feld aus. Du setzt nur `tune` korrekt — die Marker erscheinen von allein. Nicht manuell einfügen.

## Gig-Schnell-Workflow (vom Handy)

1. **Sag in normaler Sprache, was du willst** — z.B. „bei Killing das Tempo auf 90", „tausch Song 5 und 6 in Set 2", „Refrain von Hush ist C–G–D".
2. Ich ändere `index.html`, **zähle `sw.js`-Cache hoch**, committe + pushe auf `main`.
3. GitHub Pages baut automatisch neu (~1 Minute).
4. Auf dem iPad: PWA **einmal mit Internet** öffnen / neu laden → neue Version ist drin.
5. Danach wieder offline-tauglich für die Bühne.

> Tipp fürs Backstage: Änderungen lieber **vor** dem Soundcheck machen (da gibt's meist WLAN), nicht 30 Sekunden vor dem Song.

## Nicht kaputt machen

- Die Render-Funktionen (`renderList`, `openCard`, `applyHash`) und die PWA-Mechanik (Service-Worker-Registrierung, `manifest`-Link im `<head>`) **nicht anfassen**, außer es ist ausdrücklich gewünscht.
- `tune` akzeptiert nur `"E"`, `"D-Std"`, `"Drop D"` — andere Werte brechen die Marker-Logik und die Badge-Farben.
- Keine externen Ressourcen (CDN-Fonts, externe Scripts) einbauen — die App muss **offline** laufen. System-Fonts only.

## Commit & Deploy

- Kurze Commit-Message, direkt auf `main` pushen (Pages deployt von `main`/root automatisch).
- Bei reinen Datenänderungen reicht z.B.: `Setlist: Killing BPM 90` oder `Setlist: Set 2 Reihenfolge angepasst`.
- Kein Build, kein Deploy-Befehl nötig — Push genügt.

## Hintergrund-Kontext

Akkorde/Tonarten wurden gegen Ultimate Guitar, Songsterr, Tunebat u.a. recherchiert. Manche Tonart-Angaben sind „Riff-Zentrum" statt akademischer Tonart (z.B. Gimme All Your Lovin' als C statt F) — das ist für den Gitarristen gewollt. Band-Abweichungen vom Original (anderes Tuning) stehen im `tx`-Feld bzw. sind im `tune` abgebildet.
