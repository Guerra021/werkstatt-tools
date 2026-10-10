# 🪚 Schrank-Konfigurator

Ein Rechner im Browser für einen einfachen Korpusschrank mit aufliegenden Türen. Du gibst die Maße ein und bekommst sofort Zuschnittliste, Materialkosten und Fertigungszeit.

## Starten

`index.html` im Browser öffnen (Doppelklick genügt). Es braucht keine Installation und keine Internetverbindung.

## Was berechnet wird

- **Vorderansicht**: maßstäbliche Skizze mit Bemaßung. Die Türen lassen sich ein- und ausblenden.
- **Zuschnittliste**: alle Teile mit Fertigmaßen in mm, Material und Kantenangabe (Kante L / B = Anzahl bekanteter Längs- bzw. Breitseiten).
- **Material & Beschläge**: Plattenfläche inkl. Verschnitt, Kanten inkl. Zugabe, Topfbänder, Bodenträger, Griffe und Kleinmaterial.
- **Zeitermittlung**: Fertigung in der Werkstatt ohne Montage beim Kunden. Lohn = Zeit × Stundensatz, ohne Zuschläge.
- **Hinweise**: Warnungen z. B. bei durchbiegenden Einlegeböden (über ca. 800 mm), zu breiten Türen (über 600 mm) oder wenn die Türanzahl nicht zu den Feldern passt.

## Einstellbare Werte

| Gruppe | Werte |
|---|---|
| Projekt | Kunde / Projektname |
| Außenmaße | Breite, Höhe, Korpustiefe, Plattenstärke, Sockelhöhe |
| Aufteilung | Mittelseiten, Einlegeböden pro Feld, Rücksprung der Böden, Türen, Türfuge |
| Rückwand | eingenutet oder aufgesetzt, Dicke, Nuttiefe |
| Preise | €/m² für Korpus, Front und Rückwand, Verschnitt, Kante, Beschläge, Kleinmaterial |
| Zeitwerte | Minuten je Arbeitsgang, Stundensatz |

Alle Eingaben werden automatisch im Browser gespeichert. **Zurücksetzen** stellt die Beispielwerte wieder her.

## Export

- **Zuschnitt → Excel**: lädt die Zuschnittliste als CSV-Datei (Semikolon-getrennt, öffnet direkt in Excel).
- **Drucken / PDF**: druckfertige Ansicht ohne Eingabefelder.
