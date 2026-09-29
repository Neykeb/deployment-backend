# Backend Deployment Dokumentation

Dieses Dokument deckt die theoretischen Grundlagen des Backend-Deployments, die Plattformauswahl sowie das Ressourcen- und Kostenmanagement für kostenlose Tarife ab.

---

## Teil 1: Grundlagen des Backend-Deployments

### Was ist Deployment und warum braucht man es?
Deployment bedeutet, dass eine Anwendung oder Software auf einem Server bereitgestellt und veröffentlich wird, sodass sie von Benutzerinnen und Benutzern über das Internet genutzt werden kann.

* **24/7-Verfügbarkeit:** Die Software läuft kontinuierlich und zuverlässig auf einer externen Infrastruktur.
* **Aktualität:** Änderungen, neue Funktionen und Bugfixes werden sofort zentral bereitgestellt, ohne dass Nutzer lokal etwas installieren müssen.

---

### Was passiert im Hintergrund nach einem Code-Push zu GitHub?
Sobald der Code in das Repository hochgeladen wird, startet die Hosting-Plattform automatisch folgenden Prozess:

1. **Code Push:** Ein neuer Commit wird zu GitHub gepusht. Via Webhook wird die Hosting-Plattform benachrichtigt.
2. **Abhängigkeiten installieren:** Der Paketmanager installiert alle benötigten Pakete (z. B. via `npm install`).
3. **Build-Prozess:** Falls notwendig, wird der Code kompiliert oder vorbereitet (z. B. `npm run build` oder `prisma generate`).
4. **Anwendung starten:** Der Serverprozess wird gestartet (z. B. via `npm start`).
5. **Veröffentlichung:** Die Anwendung wird auf dem Webserver bereitgestellt und unter einer öffentlichen URL erreichbar gemacht.

---

### Warum eignet sich `localhost` nicht für echte Anwendungen?
Eine Anwendung auf `localhost` auszuführen bedeutet, dass sie nur auf dem eigenen lokalen Computer erreichbar ist. Das bringt wesentliche Nachteile mit sich:

* **Nicht erreichbar:** Externe Nutzerinnen und Nutzer haben keinen Zugriff auf das lokale Netzwerk.
* **Eingeschränkte Verfügbarkeit:** Der eigene Computer müsste rund um die Uhr eingeschaltet und mit dem Internet verbunden bleiben.
* **Mangelnde Skalierbarkeit:** Ein gewöhnlicher PC ist nicht dafür ausgelegt, viele parallele Anfragen stabil zu verarbeiten.

---

### Warum werden Servercode und Datenbank getrennt gehostet?
Es entspricht dem Industriestandard, den Anwendungsserver (z. B. Express) und die Datenbank (z. B. MongoDB oder PostgreSQL) auf getrennten, verwalteten Diensten zu betreiben:

1. **Skalierbarkeit:** Rechenleistung (CPU/RAM für das Backend) und Speicherplatz (für die Datenbank) können unabhängig voneinander skaliert werden.
2. **Sicherheit:** Wird der Anwendungsserver kompromittiert, bleibt die Datenbank durch eigene Zugriffskontrollen und IP-Whitelists isoliert geschützt.
3. **Wartbarkeit:** Datenbank-Updates oder -Backups können durchgeführt werden, ohne den Anwendungsserver direkt zu beeinträchtigen.
4. **Leistung:** Beide Dienste sind für ihre spezifische Aufgabe optimiert (Rechenlogik vs. schnelle E/A-Datenbankzugriffe).

---

## Teil 2: Plattformvergleich & Kostenanalyse

### Kostenlose Option für Lernende
**Render** (für das Backend) und **MongoDB Atlas** (für die Datenbank) bilden die ideale Kombination für Lernende:

* **100 % Kostenfrei:** Beide Dienste bieten dauerhaft kostenlose Tarife ohne die Pflicht zur Angabe einer Kreditkarte.
* **Harter Stopp (Hard Limit):** Bei Überschreiten von Limits wird der Dienst einfach pausiert. Es entstehen keine unerwarteten Kosten.

---

### Vergleich: Render vs. MongoDB Atlas

| Eigenschaft | Render (Web Service) | MongoDB Atlas (M0 Cluster) |
| :--- | :--- | :--- |
| **Dienst-Typ** | Application Hosting (Express Backend) | Database-as-a-Service (DBaaS) |
| **Monatliche Kosten** | $0 (Free Tier) | $0 (Dauerhaft kostenlos) |
| **Arbeitsspeicher (RAM)** | 512 MB | Geteilter RAM (Shared) |
| **Speicherplatz** | Ephemerer Speicher (Keine dauerhaften Dateien) | 512 MB Speicherplatz |
| **Inaktivität / Verhalten** | Ruhezustand nach 15 Min. ohne Anfragen (Cold Start) | Bleibt dauerhaft aktiv |
| **Ressourcen-Kontingent** | 750 Laufzeit-Stunden / Monat | Max. 500 simultane Verbindungen |
| **Monatliche Bandbreite** | 5 GB ausgehender Datenverkehr | Ausreichend für Lese-/Schreibzugriffe |
| **Kreditkarte erforderlich?** | Nein | Nein |
| **Optimal geeignet für** | Node.js / Express Server | Mongoose (MongoDB Datenbank) |

---

### Alternative Kombination: Prisma + PostgreSQL
Für Projekte mit SQL-Datenbanken bietet sich folgende Kombination an:

* **Backend-Hosting:** Vercel oder Render
* **Datenbank (DBaaS):** Neon (PostgreSQL) oder Supabase
* **Neon (PostgreSQL):** Bietet 0.5 GB kostenlosen Speicher und schaltet die Datenbank bei Inaktivität in den Ruhezustand (Auto-Suspend). Funktioniert hervorragend mit dem Prisma ORM.

---

### Abrechnungsmodelle & Sicherheit
Plattformen unterscheiden sich grundlegend in ihrer Kostenstruktur:

* **Harte Stopps (Hard Limits):** Der Dienst wird bei Limit-Erreichung pausiert. Es entstehen **0 € Zusatzkosten**. Ideal für Lernende.
* **Nutzungsbasierte Abrechnung (Pay-as-you-go / Overages):** Bei hinterlegter Kreditkarte bucht der Anbieter für jede weitere Anfrage automatisch Geld ab.

---

## Teil 3: Ressourcenlimits in kostenlosen Tarifen

### 1. RAM- / Arbeitsspeicherlimits (z. B. 512 MB)
* **Bedeutung:** Bietet Speicherplatz für den laufenden Node.js-Prozess.
* **Auswirkung bei Überschreitung:** Der Server stürzt mit einem *Out of Memory (OOM)*-Fehler ab. Die API antwortet mit einem `500` oder `502` Server-Fehler.

### 2. Cold Starts (Inaktivitäts-Timeouts)
* **Bedeutung:** Nach z. B. 15 Minuten ohne eingehende Anfragen wird die Instanz in den Ruhezustand versetzt.
* **Auswirkung bei Aufruf:** Der erste Aufruf nach einer Pause dauert ca. 30–50 Sekunden, da die Instanz neu hochfahren muss (Cold Start).

### 3. Rechenstunden & CPU-Kontingente (z. B. 750 Std./Monat)
* **Bedeutung:** Beschränkt die maximale monatliche Laufzeit der Anwendung. 750 Stunden reichen exakt für einen durchgehend laufenden Dienst pro Monat.
* **Auswirkung bei Überschreitung:** Laufen mehrere kostenlose Dienste auf einem Konto, ist das Kontingent vor Monatsende aufgebraucht und alle Dienste werden bis zum nächsten Monat deaktiviert.

### 4. Datenbankspeicher & Verbindungslimits
* **Bedeutung:** Begrenzt die Speichermenge (z. B. 512 MB) und die Anzahl der parallelen Datenbankverbindungen (z. B. max. 500).
* **Einfluss von ORMs:** Mongoose oder Prisma nutzen Connection Pools. Bei vielen gleichzeitigen Anfragen werden schnell viele Verbindungen geöffnet.
* **Auswirkung bei Überschreitung:** Neue Schreibanfragen scheitern oder Verbindungsaufrufe werden von der Datenbank abgelehnt.

### 5. Bandbreite / Ausgehender Datentransfer (z. B. 5 GB/Monat)
* **Bedeutung:** Misst das Datenvolumen, das vom Server ins Internet gesendet wird (z. B. JSON-Antworten).
* **Auswirkung bei Überschreitung:** Der Datenverkehr wird gedrosselt oder die Anwendung wird bis zum Monatsende gesperrt.