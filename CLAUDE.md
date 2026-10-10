# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Überblick

`schrank-konfigurator/index.html` ist ein Rechner für einen Korpusschrank mit aufliegenden Türen. Er erstellt Zuschnittliste, Material und Zeitermittlung, zeichnet eine Vorderansicht als SVG und bietet CSV-Export und Druckansicht. Die ganze App steckt in **einer** Datei: HTML, `<style>` und `<script>` stehen direkt darin. Nutzerdoku: `schrank-konfigurator/README.md`.

Kein Build, keine Abhängigkeiten, keine Tests und keine Linter. Zum Starten die `index.html` direkt im Browser öffnen. Die App muss offline laufen, deshalb keine CDN-Bibliotheken oder externen Ressourcen einbinden.

## Konventionen

- Oberfläche, Texte, Bezeichner und Kommentare sind **deutsch**. Zahlen werden mit `toLocaleString('de-DE')` formatiert (über den Helfer `fmt`), Beträge mit `eur`.
- Das Script steckt in einer IIFE. DOM-Zugriff läuft über den Helfer `$ = id => document.getElementById(id)`. Code-Abschnitte werden durch Kommentare wie `// ---------- Abschnitt ----------` getrennt.
- `localStorage` wird immer in `try/catch` angesprochen. Der Schlüssel `schrank.v1` trägt eine Version: Ändert sich die gespeicherte Struktur, wird die Version hochgezählt.
- Farben sind CSS-Variablen in `:root`. Der Dark Mode läuft über `prefers-color-scheme`, die Druckansicht über `@media print` (Eingabefelder und Buttons werden dort ausgeblendet).

## Architektur

Der Datenfluss läuft immer in dieser Reihenfolge: **`GROUPS` → `V` → `compute(V)` → `update()` / `draw()`**.

- `GROUPS` ist die einzige Quelle für alle Eingabefelder (`id`, `label`, `def`, `unit`, optional `type: 'text'|'select'`, `step`). `renderInputs()` baut daraus das Formular. Ein neuer einstellbarer Wert ist also nur ein neuer Eintrag in `GROUPS`; danach ist er über `v.<id>` in `compute` verfügbar. Die Werte werden automatisch gespeichert und beim Zurücksetzen wiederhergestellt.
- `compute(v)` ist eine reine Rechenfunktion ohne DOM-Zugriff. Sie liefert `parts` (Zuschnittliste), `mat` und `time` (Zeilen der Form `[Bezeichnung, Mengentext, Wert]`, Zeilen mit dem Wert 0 werden herausgefiltert), die Summen, `warnings` und die Geometriewerte für die Zeichnung.
  - Teile werden über `add(name, anz, L, Br, d, mat, kL, kB)` angelegt. `mat` muss einer der Schlüssel von `area` sein (`Korpus`, `Front`, `Rückwand`). `kL`/`kB` geben an, wie viele Längs- bzw. Breitseiten bekantet werden.
  - Die Konstruktionslogik: Boden und Deckel liegen zwischen den Seiten. Bei aufgesetzter Rückwand verringert sich die Teiletiefe um die Rückwanddicke. Bei eingenuteter Rückwand gilt das Maß Innenmaß + 2 × (Nuttiefe − 1). Einlegeböden sind Feldbreite − 2 mm lang und um `bodenRueck` kürzer in der Tiefe. Die Anzahl der Topfbänder hängt von der Türhöhe ab (`topfbaender`).
- `update()` füllt die Kennzahl-Kacheln, die Warnhinweise und die drei Tabellen per `innerHTML` und ruft dann `draw(r)` auf. `draw` zeichnet die Vorderansicht als SVG im festen Koordinatensystem 300×400, umgerechnet aus Millimetern über `X()`/`Y()`.
- Der CSV-Export verwendet Semikolons als Trenner, ein UTF-8-BOM und CRLF-Zeilenenden, damit die Datei direkt in einem deutschen Excel richtig aufgeht.
- Ändern sich Funktionen oder einstellbare Werte, muss `schrank-konfigurator/README.md` mit angepasst werden.
