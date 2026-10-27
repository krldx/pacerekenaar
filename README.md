# Pacerechner Mallorca by UTMB – GitHub Pages

Dieser Ordner ist die eigenständige Webseite für GitHub Pages:

- `index.html` – die Seite
- `tiles/` – Kartenkacheln (müssen neben `index.html` liegen)
- `firebase-rules.json` – Sicherheitsregeln für die gemeinsame Datenbank (enthält das Passwort, liegt nur lokal und wird per `.gitignore` nicht hochgeladen)
- `.nojekyll` – GitHub Pages liefert die Dateien unverändert aus

## Wie das gemeinsame Speichern funktioniert

GitHub Pages kann nur Dateien ausliefern, aber keine Daten speichern. Die Eingaben
(Zielzeiten, Modell-Faktoren, Livedaten) liegen deshalb in einer kostenlosen
**Firebase Realtime Database**. Alle Besucher lesen sie live mit. Ändert jemand etwas,
sehen es alle anderen nach etwa einer Sekunde, ohne neu zu laden.

Die Eingabe ist standardmäßig gesperrt. Über den Schalter 🔒 oben rechts und das Passwort
wird sie freigeschaltet. Die Freischaltung wird im Browser gespeichert und bleibt auch nach dem Schließen und erneuten Öffnen erhalten, bis der Schalter wieder
umgelegt wird. Geprüft wird das Passwort **auf dem Server** durch die Sicherheitsregeln:
Wer den Seitenquelltext liest, kann ohne Passwort trotzdem nichts ändern.

Ohne eingerichtete Datenbank funktioniert die Seite auch. Die Eingaben werden dann aber nur
im eigenen Browser gespeichert. Das zeigt die Seite bei „Livedaten“ an.

## Einrichtung (einmalig, ca. 10 Minuten)

1. **Firebase-Projekt anlegen:** <https://console.firebase.google.com> → „Projekt
   hinzufügen“ → Name z. B. `pacerechner` (Google Analytics kann aus bleiben).
   Der kostenlose Spark-Tarif genügt, eine Kreditkarte ist nicht nötig.
2. **Datenbank anlegen:** links „Build“ → „Realtime Database“ → „Datenbank erstellen“ →
   Standort `europe-west1` → „Im gesperrten Modus starten“.
3. **Regeln einfügen:** in der Realtime Database den Reiter „Regeln“ öffnen, den Inhalt
   durch `firebase-rules.json` ersetzen und auf „Veröffentlichen“ klicken.
4. **Adresse eintragen:** Oben im Reiter „Daten“ steht die Datenbank-Adresse, z. B.
   `https://pacerechner-1234-default-rtdb.europe-west1.firebasedatabase.app`.
   In `index.html` diese Zeile suchen:

   ```js
   const DB_URL="";
   ```

   und die Adresse zwischen die Anführungszeichen setzen.
5. **Auf GitHub veröffentlichen:** Den Inhalt dieses Ordners in ein GitHub-Repository hochladen.
   Dann unter Settings → Pages → „Deploy from a branch“ den Branch `main` und den Ordner
   `/ (root)` wählen. Nach ca. 1 Minute ist die Seite unter
   `https://<benutzername>.github.io/<repository>/` erreichbar.

Testen: Seite öffnen, Schalter umlegen, Passwort eingeben und eine Zielzeit ändern. In einem
zweiten Browser bzw. am Handy sollte die Änderung sofort erscheinen.

## Passwort ändern

Das Passwort steht an zwei Stellen:

- in den **Firebase-Regeln** (in der `key`-Regel). Diese Stelle ist entscheidend.
  Danach in der Firebase-Konsole wieder „Veröffentlichen“ klicken.
- in `index.html` als SHA-256-Wert `PW_HASH`. Er wird nur ohne Datenbank verwendet.
  Neuen Wert erzeugen mit: `printf 'neuesPasswort' | sha256sum`

## Hinweis zur Sicherheit

Die Sperre schützt vor versehentlichen oder fremden Änderungen. Sie ist aber kein
Hochsicherheitsschutz: Das Passwort wird bei jedem Speichern (verschlüsselt per HTTPS)
mitgeschickt. Wer es kennt, kann ändern. Lesen kann jeder, der die Seite aufruft.
