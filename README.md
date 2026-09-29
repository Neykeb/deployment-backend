# Was ist Deployment und warum man es braucht: 
Deployment bedeutet, dass man eine Anwendung oder Software auf einem Server bereitstellt oder veröffentlicht, sodass sie von Benutzern genutzt werden kann.
Damit die Software 24/7 erreichbar ist, zuverlässig läuft und Nutzer immer die neuesten Funktionen und Bugfixes bekommen. Ohne Deployment könnten Benutzer nicht auf die neuesten Versionen der Software zugreifen, was die Benutzererfahrung beeinträchtigen würde.

# Was passiert im Hintergrund, nachdem du deinen Code zu GitHub gepusht hast, bis die API im Web antwortet?
1. **Code Push**: Du pushst deinen Code zu einem GitHub-Repository.
2. **Abhängigkeiten installieren**: Die benötigten Abhängigkeiten werden aus dem Package-Manager installiert (z.b. npm install).
3. **Build-Prozess**: Der Code wird kompiliert und in ein ausführbares Format umgewandelt.
4. **Anwendung starten**: Die Anwendung wird gestartet und auf dem Server ausgeführt.
5. **Veröffentlichung**: Die Anwendung wird auf einem Webserver bereitgestellt, sodass sie über das Internet zugänglich ist.

# Warum eignet es sich nicht für echte Nutzerinnen und Nutzer, eine Anwendung auf localhost auszuführen (oder deinen eigenen Computer rund um die Uhr eingeschaltet zu lassen)?
Eine Anwendung auf localhost auszuführen bedeutet, dass sie nur auf deinem eigenen Computer verfügbar ist. Das hat mehrere Nachteile:
- **Nicht erreichbar**: Andere Benutzer können nicht auf die Anwendung zugreifen, da sie nur auf deinem lokalen Rechner läuft.
- **Verfügbarkeit**: Dein Computer muss ständig eingeschaltet sein, damit die Anwendung erreichbar ist. Das ist unpraktisch und energieaufwendig.
- **Skalierbarkeit**: Wenn viele Benutzer gleichzeitig auf die Anwendung zugreifen möchten, kann dein Computer die Last nicht bewältigen. Ein Server ist dafür ausgelegt, mehrere Anfragen gleichzeitig zu verarbeiten. 

# Warum ist es in der Branche Standard, deinen Servercode (Express) und deine Datenbank (PostgreSQL oder MongoDB) in der Produktion auf getrennten verwalteten Diensten zu hosten, statt beides auf demselben Server zu betreiben?
1. **Skalierbarkeit**: Getrennte Dienste sind leichter skalierbar. Du kannst die Ressourcen für den Server und die Datenbank unabhängig voneinander anpassen, je nach Bedarf.
2. **Sicherheit**: Es ist sicherer, die Datenbank auf einem separaten Server zu hosten, da dies das Risiko von Sicherheitsverletzungen verringert. Wenn der Server kompromittiert wird, ist die Datenbank immer noch geschützt.
3. **Wartbarkeit**: Getrennte Dienste erleichtern die Wartung und Updates. Du kannst die Datenbank aktualisieren oder warten, ohne den Servercode zu beeinträchtigen, und umgekehrt.
4. **Leistung**: Durch die Trennung können beide Dienste optimiert werden, um die bestmögliche Leistung zu erzielen. Der Server kann sich auf die Verarbeitung von Anfragen konzentrieren, während die Datenbank für schnelle Datenabfragen optimiert wird.

# Welche Optionen sind für Lernende am sichersten, wenn sie unerwartete Kosten vermeiden möchten?
Render für das Backend und MongoDB Atlas für die Datenbank bilden die ideale Kombination für Lernende. Beide Plattformen bieten voll verwaltete, dauerhaft kostenlose Tarife ohne Angabe einer Kreditkarte. Das macht sie besonders sicher: Sollte das kostenlose Nutzungslimit erreicht werden, greift ein automatischer harter Stopp (Hard Limit) – der Dienst pausiert einfach, anstatt unerwartete Kosten zu verursachen.

# Kostenlose Tarife für Node.js (Mongoose / Prisma)

| Eigenschaft | Render (Web Service) | MongoDB Atlas (M0 Cluster) |
| :--- | :--- | :--- |
| **Dienst-Typ** | Application Hosting (Express Backend) | Database-as-a-Service (DBaaS) |
| **Monatliche Kosten** | $0 (Free Tier) | $0 (Dauerhaft kostenlos) |
| **Arbeitsspeicher (RAM)** | 512 MB | Geteilter RAM (Shared) |
| **Speicherplatz** | Kein fester Speicher (Ephemeral Storage) | 512 MB Speicherplatz |
| **Verfalleffekt / Inaktivität** | Geht nach 15 Minuten ohne Anfragen in den Ruhezustand (Cold Start) | Bleibt dauerhaft aktiv (Kein automatisches Löschen) |
| **Ressourcen-Kontingent** | 750 Laufzeit-Stunden pro Monat | Maximal 500 simultane Verbindungen |
| **Monatliche Bandbreite** | 5 GB ausgehender Datenverkehr | Reicht für normale Lese- und Schreibzugriffe |
| **Kreditkarte erforderlich?** | Nein | Nein |
| **Optimal geeignet für** | Node.js / Express Server | Mongoose (MongoDB Datenbank) |

# 1. Das zweite Paar: Prisma + PostgreSQL
 ### Für Lernende und Entwickler, die mit Prisma und einer SQL-Datenbank (PostgreSQL) arbeiten, sind folgende kostenlosen Tarife sehr beliebt:
Backend-Hosting: Vercel oder Render
Datenbank (DBaaS): Neon oder Supabase

**Neon (PostgreSQL):**

Kostenloser Tarif: Bietet ein festes Speicherlimit (z. B. 0.5 GB) und schaltet die Datenbank bei Inaktivität automatisch in den Ruhezustand (Auto-Suspend).

Vorteil für Prisma: Funktioniert hervorragend mit dem Prisma ORM.

# 2. Kostenanalyse & Sicherheit vor Überraschungen
Plattformen unterscheiden sich stark darin, was passiert, wenn du ein Limit erreichst:
Harte Stopps (Hard Limits): Die Anwendung wird einfach gestoppt oder pausiert, sobald das Limit erreicht ist. Es entstehen 0 € Zusatzkosten.
Nutzungsbasierte Abrechnung (Pay-as-you-go / Overages): Wenn eine Kreditkarte hinterlegt ist, bucht der Anbieter für jede weitere Anfrage oder jedes Megabyte automatisch Geld ab.

# 3. Ressourcenlimits
## 1. RAM- / Arbeitsspeicherlimits (z. B. 512 MB)
Bedeutung: Der Arbeitsspeicher (RAM) stellt den kurzfristigen Speicher für den laufenden Serverprozess bereit. 512 MB reichen für einfache Logik aus, bieten aber wenig Platz für große Datenmengen im Speicher.

Auswirkung: Wird das RAM-Limit überschritten, stürzt die Anwendung mit einem Out of Memory (OOM)-Fehler ab. Für Nutzerinnen und Nutzer ist die API schlagartig nicht mehr erreichbar (Fehlermeldung 500 oder 502).

## 2. Cold Starts / Ruhephasen / Inaktivitäts-Timeouts
Bedeutung: Werden für einen bestimmten Zeitraum (z. B. 15 Minuten) keine Anfragen an den Server gesendet, schaltet die Plattform die Instanz in den Ruhezustand, um Ressourcen zu sparen.

Auswirkung: Die nächste Person, die die Anwendung nach der Pause aufruft, erlebt eine Verzögerung: Der Server muss erst „aufwecken“ und neu starten. Dieser Cold Start führt dazu, dass die erste Antwort 30 bis 50 Sekunden dauern kann. Danach reagiert das System wieder normal.

## 3. Rechenstunden und CPU-Kontingente (z. B. 750 Stunden/Monat) 
Bedeutung: Das Kontingent beschreibt die maximale Zeit, die ein Server im Monat aktiv laufen darf. Da ein Monat etwa 720–744 Stunden hat, reicht ein Limit von 750 Stunden exakt für eine durchgehend laufende Anwendung.

Auswirkung: Werden mehrere kostenlose Dienste gleichzeitig auf demselben Konto betrieben, ist das Kontingent vor Monatsende aufgebraucht. Sobald das Limit erreicht ist, werden die Dienste bis zum Beginn des nächsten Monats vollständig deaktiviert.

## 4. Datenbankspeicher und Limits für aktive Verbindungen
Bedeutung: Beschränkt die maximale Speichermenge für Datensätze (z. B. 512 MB) sowie die Anzahl der Personen/Prozesse, die gleichzeitig mit der Datenbank verbunden sein dürfen (z. B. max. 500 Verbindungen).

Einfluss von ORMs (Prisma / Mongoose): ORMs bauen für eingehende Anfragen automatisch Verbindungspools (Connection Pools) auf. Bei hoher Last werden rasch viele parallele Verbindungen geöffnet.

Auswirkung: Ist der Speicher voll, können keine neuen Daten gespeichert werden. Bei Erreichen des Verbindungslimits lehnt die Datenbank neue Anfragen ab, wodurch Fehlermeldungen in der Anwendung entstehen.

## 5. Ausgehender Datentransfer / Bandbreite (z. B. 5 GB pro Monat)
Bedeutung: Misst die Datenmenge, die vom Server an die Nutzerinnen und Nutzer ins Internet gesendet wird (z. B. JSON-Antworten oder Bilder).

Auswirkung: Bei Überschreiten der monatlichen Datenmenge wird der Datenverkehr entweder gedrosselt, der Dienst bis zum Monatsende gesperrt oder es fallen (bei hinterlegten Zahlungsmethoden) Zusatzkosten an.

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

# Sicherheits- & Pre-Deployment-Checkliste
