# Pre-Deployment Checkliste 

Diese Checkliste fasst alle kritischen Sicherheits-, Fehlerbehandlungs- und Konfigurationsschritte zusammen, die vor dem Live-Gang eines Node.js/Express-Backends durchgeführt werden müssen.

---

## 1. Sicherheit & Umgebungsvariablen 

- [ ] **`.env`-Datei schützen:** Die Datei `.env` ist in der `.gitignore` eingetragen und wird niemals auf GitHub hochgeladen.
- [ ] **Umgebungsvariablen im Dashboard setzen:** Alle Zugangsdaten (wie `MONGODB_URI`, API-Schlüssel, Secrets) sind auf der Hosting-Plattform (Render) unter *Environment* hinterlegt.
- [ ] **`NODE_ENV` auf `production` stellen:** In den Umgebungsvariablen ist `NODE_ENV=production` gesetzt, um Debug-Funktionen und detaillierte Express-Meldungen zu deaktivieren.
- [ ] **Sicherheits-Header verwenden:** Das Paket `helmet` ist eingebunden, um gängige Web-Sicherheitslücken (z. B. XSS, Clickjacking) abzuwehren.

---

## 2. Fehlerbehandlung (Error Handling) 

- [ ] **Keine Stacktraces für Endnutzer:** Sensible Fehlerdetails (Stacktraces) werden in der Produktionsumgebung nicht an Client-Antworten (JSON) angehängt.
- [ ] **Zentraler Error-Handler:** Eine Express-Fehler-Middleware fängt unerwartete Serverfehler ab und gibt eine allgemeine Fehlermeldung (`500 Internal Server Error`) zurück.
- [ ] **Gültige HTTP-Statuscodes:** Alle Endpunkte liefern semantisch korrekte Statuscodes zurück (z. B. `200 OK`, `201 Created`, `400 Bad Request`, `404 Not Found`).

---

## 3. Datenbank-Verbindung 

- [ ] **Sicheres Connection-Handling:** Die Datenbankverbindung wird beim Serverstart korrekt aufgebaut, und Fehler beim Verbindungsaufbau werden abgefangen.
- [ ] **Netzwerk-Zugriff freigegeben:** Die IP-Freigabe (`0.0.0.0/0`) ist im MongoDB Atlas Dashboard eingerichtet, damit der Cloud-Server auf die Datenbank zugreifen kann.
- [ ] **Passwort-Platzhalter ersetzt:** Der Verbindungs-String enthält das echte Datenbank-Passwort und keine Platzhalter mehr.

---

## 4. Server- & Plattform-Vorbereitung (Render) 

- [ ] **Dynamischer Port:** Der Server hört auf `process.env.PORT` (mit Fallback auf z. B. `3000`), um den von Render zugewiesenen Port zu nutzen.
- [ ] **Start-Script definiert:** In der `package.json` ist ein valides `start`-Script enthalten (z. B. `"start": "node server.js"`).
- [ ] **Build-Command überprüft:** Der Befehl `npm install` läuft fehlerfrei durch und alle benötigten Abhängigkeiten stehen unter `dependencies` (nicht nur `devDependencies`).

---

## 5. End-to-End Test nach dem Deployment 🧪

- [ ] **Lese-Zugriff (GET):** Ein GET-Aufruf an die öffentliche Render-URL liefert erfolgreich Daten aus der Cloud-Datenbank zurück.
- [ ] **Schreib-Zugriff (POST):** Ein POST-Aufruf speichert ein neues Dokument erfolgreich in der Cloud-Datenbank ab.