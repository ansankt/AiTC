# AiTC · Apron Control Lab

Interaktiver Prototyp für Aufgabe 3: menschliches Beobachten, Entscheiden und Eingreifen in eine automatisierte Vorfeldsteuerung. HTML, CSS und JavaScript ohne Build-Schritt.

## Start

`index.html` direkt im Browser öffnen oder im Projektordner `python3 -m http.server 8080 --bind 127.0.0.1` ausführen und http://127.0.0.1:8080 öffnen.

## Bedienung

- Starten/Pause, Tempo und Neustart steuern die Simulation.
- Oben: tatsächliche Verkehrslage. Unten: ausschließlich erfasste Flugzeuge, mit Ziel und Status.
- Ein Flugzeug anklicken; Halten oder Freigeben übernimmt es manuell. Freigeben setzt seine feste Route fort, ohne automatische Konfliktprüfung.
- Zurück an Automation gibt die Steuerung ausdrücklich zurück. Das setzt aktuelle Sensordaten voraus; sonst erscheint eine Ablehnung im Protokoll.
- Unter Szenario & Sensorfehler lässt sich ein von Beginn an fehlendes Flugzeug auswählen (anschließend Neustart). Es erscheint unten nicht und erzeugt keine Warnung. Seine unabhängige Bewegung repräsentiert im Versuch Verkehr außerhalb der Automation.
- Zufälliger Signalverlust betrifft rollende Flugzeuge. Die letzte Position bleibt rot markiert stehen und eine Warnung zeigt ihr Alter. Das echte Flugzeug führt den letzten Befehl weiter aus.
- Signalverlust und Wiederherstellung lassen sich für das ausgewählte Flugzeug auch manuell auslösen. Diese Bedienelemente gehören zur Versuchsleitung, nicht zum Controller.

## Modell und Grenzen

Fünf Parkpositionen, zwei schematische Runways, drei Abflüge und zwei Ankünfte. Abflüge enden am Holding Point und werden nach sechs Sekunden an die nachgelagerte Kontrolle übergeben; Ankünfte enden auf ihrer Parkposition. Kein Start-/Landebetrieb.

Die Automation reserviert konservativ die gesamte verbleibende Route einer freigegebenen Bewegung. Freie Routen werden in der Reihenfolge der längsten Wartezeit vergeben, bei Gleichstand nach Rufzeichen. Es handelt sich um eine einfache Konfliktvermeidungsregel, nicht um eine globale Optimierung von Rollzeiten. Veraltete Tracks blockieren nur ihren zuletzt bekannten Abschnitt. Unbekannter Verkehr geht niemals in die Planung ein. Manuelle Freigaben können Reservierungen übergehen.

Die Simulation stoppt bei einer tatsächlichen Unterschreitung des modellierten Mindestabstands. Dieser Ausgangscheck nutzt die wirkliche Lage ausschließlich zum Beenden des Versuchs, nicht zur Vorauswarnung oder Planung. Abmessungen, Geschwindigkeit, Abstände und Pushback sind schematisch. Statische Routen, keine Pilotenreaktionszeiten, kein Funk und keine realistische Schleppfahrzeugphysik.

Für Aufgabe 3: Der Mensch vergleicht die Ansichten, erkennt fehlende Flugzeuge bzw. bewertet veraltete Daten, entscheidet über Halten/Freigeben und übernimmt einzelne Flugzeuge. Die Kontrolle wird auf der Ebene einzelner Bewegungsbefehle ausdrücklich übergeben und zurückgegeben.

## Prüfen

`node --check app.js`

`node tests/simulation.cjs`

Die Simulationstests prüfen den fehlerfreien Durchlauf, verborgene Tracks, eingefrorene Sensordaten, manuellen Halt und die Rückgabe nur mit aktuellen Daten.
