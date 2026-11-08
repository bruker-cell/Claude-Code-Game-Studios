# Urzeit-Odyssee — Prototyp (HTML)

**Hypothese:** Ein „Fressen und Wachsen“-Spiel mit echten Epochen-Fakten macht am Handy
Spaß, wenn jede Epoche in 2–4 Minuten schaffbar ist und der Asteroid als Finale
die Geschichte abschließt.

**Status:** in Arbeit (Konzept-Prototyp, `modes.rigor: minimal`).

**Starten:** `index.html` im Browser öffnen (mit Internet: Three.js r128 kommt von cdnjs,
Schriften von Google Fonts). Die Datei ist zugleich als claude.ai-Artifact veröffentlicht.

---

## Game Brief: Urzeit-Odyssee

**One-sentence pitch:** Schlüpfe als winziger Dinosaurier, friss dich durch Trias, Jura
und Kreide nach oben — und überlebe den Asteroiden als kleiner Vogel.

### Core loop
- Kleinere Tiere jagen (grüner Ring) → wachsen → größere Tiere werden zur Beute.
- Räuber und wehrhafte Pflanzenfresser (roter Ring) meiden, mit Sprint fliehen.
- Leuchtende Fossilien finden → Fakt fürs Fossilbuch.

### Player goal & fail state
- Ziel: in jeder Epoche 100 % Wachstum, dann 45 s Einschläge überleben.
- Niederlage: alle 3 Herzen verloren (gefressen, von Hörnern/Keulen getroffen,
  vom Einschlag getroffen oder verhungert). Neustart der Epoche ist sofort möglich.

### MVP
1. Bewegung per Ziehen (Touch/Maus) und WASD, Sprint mit Abklingzeit
2. Fressen/Wachsen mit Größenregel (Beute ≤ 80 % der eigenen Größe)
3. Tier-KI: Beute flieht, Räuber jagen (mit Aufgeben nach 4,5 s)
4. Drei Epochen mit eigener Tierwelt, Pflanzen und Farben
5. Finale: Einschläge mit Vorwarnung, Samen als Energie
6. Fossilien mit Fakten, Fossilbuch und Rekord (lokal gespeichert)

### Out of scope
- Kein Mehrspieler, keine Accounts, kein Server
- Keine Grafik-Assets: alle 3D-Modelle werden im Code aus Grundformen gebaut
- Keine Meta-Progression (Upgrades, Shop)
- Keine Musik, nur kurze synthetische Effekte

### Build order
1. Bewegung, Kamera und Fressen in der Trias (Kern-Spaß zuerst)
2. Gefahr: Räuber-KI, Herzen, Rückstoß
3. Jura und Kreide als Daten (`ERAS`)
4. Finale mit Einschlägen
5. Fossilien, Karten, Ton, Feinschliff

---
**Who it's for / what they feel:** Dino-Fans am Handy, die in 10 Minuten eine kleine
Erdgeschichte erleben wollen: vom Gejagten zum Jäger.

**Art & audio direction:** Draufsicht wie ein Feldbuch: warme Erdtöne pro Epoche,
gezeichnete Farne, Palmfarne und Nadelbäume, Bernstein als Akzent.

**Reference game:** Genre „eat smaller, avoid bigger“ — davon nur die Wachstumsregel;
keine Level-Auswahl, keine Upgrades.

---

## Inhalte

Alle Fakten stammen aus dem Second Brain (`obsidian-second-brain/Dinosaurier/`):
Epochen-Notizen, „Jäger und Räuber“, „Fortpflanzung, Eier und Brutpflege“ und
„Die Vögel als überlebende Dinosaurier“.

## Bearbeiten

- `CONFIG` oben im Skript: alle Tuning-Werte (Tempo, Größenregeln, Finale-Dauer).
- `ERAS`: Epochen, Tierarten, Fakten, Farben, Pflanzen.
- Konsole: `__urzeit.startEra(3)` springt direkt ins Finale.

## Findings

Screenshots im Ordner `evidence/` (iPhone-13-Ansicht: Titel, Kreide, Finale).

Bot-Messung (läuft zur nächsten Beute, weicht Gefahren aus, ohne Sprint):

| Epoche | Ergebnis | Zeit | Herzen übrig |
|---|---|---:|---:|
| Trias | geschafft | 59 s | 2 |
| Jura | geschafft | 118 s | 2 |
| Kreide | geschafft | 148 s | 1 |
| Finale ohne Ausweichen | tot | 26 s | 0 |
| Finale mit Ausweichen | überlebt | 45 s | 1 |

Änderungen aufgrund der Messung:
- Beute bemerkte den Spieler schon außerhalb des Bildschirms und floh → Sichtweite der Beute verkürzt.
- Jura war am Anfang zäh → kleines Säugetier *Fruitafossor* als leichte Beute ergänzt.
- Finale etwas entschärft (längere Vorwarnung, etwas kleinere Einschläge).

### Version 2: 3D (nach Wunsch des Spielers)

- Grafik von 2D-Canvas auf Three.js umgestellt: feste Kamera schräg von oben (55°),
  Low-Poly-Tiere aus Grundformen mit Lauf-, Schwanz- und Flügelanimation, echte Schatten.
- Pflanzen und Tiere zwischen Kamera und Spieler werden um ihn herum ausgeblendet
  (Durchsicht-Kreis im Shader), damit man die Figur nie verliert.
- Neues Futter, das nicht wegläuft: Nester (+ ca. 4 % Größe) und Aas (+ ca. 9 %).
  Kreide-Eier sind blaugrün wie die nachgewiesenen Oviraptoriden-Eier.
- Jäger laufen höchstens 93 % deines Tempos (`CONFIG.hunterSpeed`). Test: Postosuchus,
  Allosaurus und T. rex jagen, der Abstand wächst um etwa 10 Einheiten pro Sekunde.
- Leistung: 220–300 Zeichenaufrufe pro Bild. Bei unter 40 FPS schaltet das Spiel erst die
  Schatten, dann die Auflösung herunter.
- Gefundener Fehler: Die Spielfigur hatte kein `speed` in den Daten → Animation rechnete
  mit NaN, Figur unsichtbar. Behoben mit Rückfall auf `CONFIG.playerSpeed`.

Noch offen: menschlicher Spieltest am Handy (Spaß, Schwierigkeit, Bildrate) → Urteil
PROCEED / PIVOT / KILL.
