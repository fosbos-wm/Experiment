# Das Experiment – Selbstlernkurs mit Klassencode

Eigenständige Web-App (GitHub Pages + Firebase), unabhängig von F11Sb und F12Sb.
Die Lehrkraft legt einen **Raum** mit einem 6-stelligen **Klassencode** an. Schüler:innen geben Code und Spitznamen ein, werden **per Los in Gruppe A oder B** eingeteilt (gleich große Gruppen), führen das Kaugummi-Experiment allein am eigenen Gerät durch und senden ihr Ergebnis **anonym** an die Klasse. Danach folgen die Merkmale des Experiments, die Anwendungsaufgaben (mit „Lösung anzeigen“) und das Abschlussquiz mit PDF-Zusammenfassung.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | die ganze App (Kurs, Beitritt, Lehrkraftansicht) |
| `backend.js` | Datenschicht: Firebase, oder ohne Konfiguration ein Demo-Modus nur im eigenen Browser |
| `firebase-config.js` | **hier die Konfiguration deines Firebase-Projekts eintragen** |
| `firestore.rules` | Sicherheitsregeln für Firestore (in die Firebase-Konsole kopieren) |
| `zusammenfassung-experiment.pdf` | Download am Ende des Kurses |

## Einrichtung (einmalig, ca. 20 Minuten)

1. **Firebase-Projekt anlegen** (Konsole → Projekt hinzufügen). Google Analytics wird nicht benötigt.
2. **Web-App hinzufügen** (Projekteinstellungen → Allgemein → „Web-App“). Die angezeigte Konfiguration in `firebase-config.js` eintragen.
3. **Authentication** → Sign-in-Methode:
   - **Anonym** aktivieren (damit Schüler:innen ohne Konto teilnehmen können),
   - **E-Mail/Passwort** aktivieren und unter „Users“ ein Konto für die Lehrkraft anlegen. Die **UID** dieses Kontos kopieren.
4. **Firestore Database** anlegen (Produktionsmodus, Region z. B. `eur3` oder Frankfurt `europe-west3`).
5. **Regeln**: Inhalt von `firestore.rules` im Reiter „Regeln“ einfügen und **Veröffentlichen**.
6. **Lehrkraft freischalten**: In Firestore eine Sammlung `lehrer` anlegen, Dokument-ID = **UID der Lehrkraft** (ein beliebiges Feld, z. B. `name: "…"`).
   Weitere Lehrkräfte: gleiches Vorgehen mit deren UID.
7. **Autorisierte Domains**: Authentication → Einstellungen → Autorisierte Domains → deine GitHub-Pages-Domain (`DEINNAME.github.io`) hinzufügen.
8. **Neues GitHub-Repo** anlegen, alle Dateien aus diesem Ordner hochladen (direkt ins Hauptverzeichnis), unter *Settings → Pages* „Deploy from a branch“ (`main`, `/ (root)`) aktivieren.

## Benutzung

- **Lehrkraft:** Startseite → „Ich bin Lehrkraft“ (oder `…/#lehrkraft`) → anmelden → „Neuen Raum anlegen“. „Code groß zeigen“ für den Beamer.
  Mit „Anmeldung offen“ steuerst du, ob noch jemand beitreten kann. Mit **„Vergleich für die Klasse freigeben“** sehen die Schüler:innen erst dann die Gruppenergebnisse (am besten, wenn alle fertig sind). Unter „Ergebnisse ansehen“ siehst du live, wer fertig ist, sowie Mittelwerte und eine CSV ohne Namen.
- **Schüler:innen:** Seite öffnen, Klassencode und Spitznamen eingeben (bitte keinen vollen Namen), Kurs durcharbeiten.
- **Testlauf mit kurzen Zeiten:** `…/index.html?test=1` (Lernphase 12 s statt 5 min).

## Wichtig zu wissen

- **Ohne Firebase-Konfiguration** startet die App im **Demo-Modus** (gelbes Banner). Dann liegen alle Daten nur im eigenen Browser; mehrere Tabs im selben Browser sehen sich. Praktisch zum Ausprobieren, nicht für die Klasse.
- **Lokal testen** geht nur über einen Webserver (z. B. `python -m http.server`), nicht durch Doppelklick auf `index.html`.
- **Code-Länge:** 6 Ziffern (`CODE_LAENGE` in `backend.js`, passend dazu `[0-9]{6}` in `firestore.rules`). Wer den Code kennt, kann beitreten. Schließe die Anmeldung, sobald alle drin sind.
- **Zufallsgruppen:** Die kleinere Gruppe wird zuerst aufgefüllt, bei Gleichstand entscheidet der Zufall. Die Regeln prüfen das serverseitig.
- **Datenschutz:** Gespeichert werden Spitzname, Gruppe, Wörterzahl und Zeitstempel. Die anonymen Ergebnisse enthalten keinen Namen. Räume bitte nach dem Unterricht über „Löschen“ entfernen (löscht auch alle Ergebnisse). Das Konzept sollte mit Schulleitung/Datenschutzbeauftragten abgestimmt werden.
- **Kursstand** (besuchte Abschnitte, Quizantworten) bleibt im Browser der jeweiligen Person.
- Lokale Speicherschlüssel beginnen mit `expapp:`, damit sich die App nicht mit anderen Apps auf `github.io` überschneidet.
