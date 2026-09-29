# Deployment Blueprint: Node.js/Express & MongoDB Atlas auf Render 🚀

Diese Anleitung beschreibt Schritt für Schritt das praktische Deployment eines Node.js/Express-Backends auf **Render** in Kombination mit einer gehosteten MongoDB-Datenbank auf **MongoDB Atlas**.

---

## Schritt 1: Datenbank bereitstellen (MongoDB Atlas) 

Der erste Meilenstein ist das Erstellen und Konfigurieren der Cloud-Datenbank:

1. **Cluster erstellen:** Bei MongoDB Atlas anmelden und einen kostenlosen **M0 Cluster** anlegen.
2. **Datenbank-User anlegen:** Einen Benutzernamen und ein sicheres Passwort für den Datenbankzugriff erstellen.
3. **Netzwerkzugriff (IP Whitelist):** Die IP-Adresse `0.0.0.0/0` freigeben, damit der Render-Server von überall im Web auf die Datenbank zugreifen kann.
4. **Verbindungs-URI kopieren:** Den Connection String abrufen (z. B. `mongodb+srv://<username>:<password>@cluster0.mongodb.net/dbname`).

> ⚠️ **Wichtig:** Das Platzhalter-Passwort `<password>` muss durch das echte Passwort ersetzt werden. Der String darf niemals direkt im Code stehen!

---

## Schritt 2: Umgebungsvariablen (.env) sicher auf Render einrichten 

Da die `.env`-Datei aus Sicherheitsgründen nicht auf GitHub hochgeladen wird, müssen die Variablen auf Render hinterlegt werden:

1. **Environment-Einstellungen:** Im Render-Dashboard zur eigenen App navigieren und den Bereich **Environment** öffnen.
2. **Key-Value-Paare anlegen:** Die benötigten Variablen eintragen:
   * `MONGODB_URI` = `mongodb+srv://<username>:<password>@cluster0.mongodb.net/dbname`
   * `PORT` = `3000` (oder ein beliebiger Zielport)
3. **Sicherer Zugriff:** Render stellt diese Variablen dem laufenden Prozess zur Verfügung (im Code erreichbar über `process.env.MONGODB_URI`).

---

## Schritt 3: Build- und Startbefehle definieren 

Damit Render die Anwendung korrekt installieren und ausführen kann, werden im Dashboard folgende Befehle konfiguriert:

* **Build Command:** `npm install`  
  *(Installiert alle in der `package.json` definierten Abhängigkeiten auf dem Server)*
* **Start Command:** `npm start`  
  *(Startet den Serverprozess in der Produktionsumgebung, z. B. über `node server.js`)*

---

## Schritt 4: GitHub-Repository mit Render verbinden 

Die Verknüpfung mit GitHub ermöglicht eine automatisierte Bereitstellung (CI/CD):

1. **Repository verknüpfen:** Das gewünschte GitHub-Repository im Render-Dashboard auswählen.
2. **Automatisches Deployment:** Render registriert jeden neuen Push (`git push`) auf dem Haupt-Branch, zieht sich den aktuellen Code automatisch und führt den Build- sowie Start-Prozess neu aus.

---

## Schritt 5: Testen der API im Web 

Nach erfolgreichem Build stellt Render eine öffentliche URL bereit (z. B. `https://mein-backend.onrender.com`). Zur Überprüfung der Funktionalität werden zwei Tests durchgeführt:

* **Lesen (GET-Anfrage):** Aufrufen eines Endpunkts (z. B. `/api/items`), um zu prüfen, ob Daten erfolgreich aus MongoDB Atlas ausgelesen werden.
* **Schreib-Zugriff (POST-Anfrage):** Senden neuer Daten an die API (z. B. via Postman), um zu bestätigen, dass neue Dokumente korrekt in der Cloud-Datenbank gespeichert werden.