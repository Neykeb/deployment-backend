# Recherche zum praktischen Deployment und Deployment-Plan
**Schritt 1: Datenbank bereitstellen (MongoDB Atlas)** 
Der erste Meilenstein in der Anleitung ist das Erstellen der Datenbank im Cloud-Dienst:

Cluster erstellen: Bei MongoDB Atlas einen kostenlosen M0 Cluster anlegen.

Datenbank-User anlegen: Einen Benutzernamen und ein sicheres Passwort für den Datenbankzugriff erstellen.

Netzwerkzugriff (IP Whitelist): Die IP-Adresse 0.0.0.0/0 freigeben, damit der Render-Server von überall im Web auf die Datenbank zugreifen kann.

Verbindungs-URI kopieren: Den Connection String (z. B. mongodb+srv://<username>:<password>@cluster0.mongodb.net/dbname) abrufen.

**Schritt 2: Umgebungsvariablen (.env) sicher auf Render einrichten**
Da die .env-Datei nicht auf GitHub hochgeladen wird, kennt der Hosting-Server (Render) deine Passwörter erst einmal nicht.

Environment Variables auf Render: Im Render-Dashboard navigiert man zu den Einstellungen der App (Environment).

Key-Value-Paare anlegen: Man trägt dort die Namen und Werte ein (z. B. MONGODB_URI=mongodb+srv://... oder PORT=3000).

Sicherer Zugriff: Render stellt diese Variablen dem laufenden Node.js-Prozess zur Verfügung (Zugriff im Code über process.env.MONGODB_URI).

**Schritt 3: Build- und Startbefehle definieren (npm install, npm start)**
In Node.js-Projekten nutzt man im Render-Dashboard typischerweise folgende Befehle:

Build Command: npm install (installiert alle notwendigen Pakete aus der package.json).

Start Command: npm start (startet die Produktion-Anwendung, z. B. über node server.js).

**Schritt 4: GitHub-Repository mit Render verbinden**
Sobald Render mit deinem GitHub-Konto verbunden ist, wählst du dein Projekt-Repository aus.
Automatisches Deployment: Render erkennt jeden neuen Code, den du zu GitHub pushst (git push), zieht sich die Änderungen automatisch und führt Build- sowie Start-Befehle neu aus.

**Schritt 5: Testen der API im Web**
Nachdem der Server erfolgreich gestartet ist, stellt Render eine öffentliche URL bereit (z. B. [https://mein-backend.onrender.com](https://mein-backend.onrender.com)).

Um sicherzustellen, dass die API und die Datenbank korrekt zusammenarbeiten, führt man zwei Tests durch:

* Lesen (GET-Anfrage): Aufrufen eines Endpunkts (z. B. /api/items), um Daten aus der MongoDB-Datenbank abzurufen.

* Schreiben (POST-Anfrage): Senden neuer Daten an die API (z. B. über Postman oder eine Frontend-Anwendung), um zu überprüfen, ob ein neues Dokument in der Cloud-Datenbank gespeichert wird.
