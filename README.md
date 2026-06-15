# Discord Battleship Engine (powered by Node-RED)

Dieses Projekt bringt das klassische Strategiespiel **Schiffe versenken** (Battleship) direkt auf Ihren Discord-Server. Die gesamte Spiellogik wird über eine ressourcenschonende Engine in Node-RED gesteuert, die sowohl Einzelspieler-Partien gegen eine künstliche Intelligenz (KI) als auch abhörsichere PvP-Duelle zwischen zwei Server-Mitgliedern ermöglicht.

🔗 **Projekt-Link auf GitHub:** [https://github.com/DEIN_BENUTZERNAME/DEIN_REPOSITORY](https://github.com/GonTh724/discord_battleship)

---

## Was ist dieses Projekt?

Dieses Projekt ist eine Event-gesteuerte Spiel-Engine für Discord, implementiert in Node-RED. 

### Hauptmerkmale:
* **Einzelspieler-Modus (PvE):** Treten Sie gegen eine KI an, die unmittelbar nach jedem Ihrer Spielzüge mit einem eigenen, zufälligen Schuss reagiert.
* **Mehrspieler-Modus (PvP):** Fordern Sie andere Server-Mitglieder im Chat heraus.
* **Double-Blind-System für PvP:** Um Schummeln im öffentlichen Chat zu verhindern, werden die Schiffspositionen beider Spieler komplett geheim gehalten. Die Spielfelder werden für alle sichtbar als reine "Zielboards" (nur Treffer und Fehlschüsse) dargestellt.
* **Status-Sicherung:** Der Spielfluss wird über den Flow-Context in Node-RED kanalweise gespeichert, sodass mehrere Kanäle auf einem Server gleichzeitig eigene Partien spielen können.

---

## Welche Hardware wird benutzt?

Da die gesamte Logik in Node-RED (basierend auf Node.js) ausgeführt wird, ist das Projekt hardwareunabhängig. Es kann auf fast jedem System betrieben werden, das rund um die Uhr online ist:

* **SBCs (Single Board Computer):** Ein **Raspberry Pi** (Modell 3, 4, 5 oder Zero 2 W) ist für den 24/7-Betrieb optimal geeignet.
* **Lokale Server/PCs:** Jeder Heimserver, NAS (z. B. Synology via Docker) oder Desktop-PC (Windows, macOS, Linux).
* **Cloud-Hosting:** Virtuelle Server (VPS), FlowFuse oder Cloud-Instanzen (z. B. AWS, DigitalOcean).
* **Systemanforderungen:** Äußerst gering. Node-RED benötigt für diese Anwendung weniger als 256 MB freien Arbeitsspeicher und minimale CPU-Ressourcen.

---

## Wie wird dieses Projekt installiert?

### 1. Discord Bot einrichten
1. Öffnen Sie das [Discord Developer Portal](https://discord.com/developers/applications).
2. Erstellen Sie eine **New Application** und fügen Sie unter "Bot" einen neuen Bot hinzu.
3. **Wichtig:** Aktivieren Sie im Bereich *Privileged Gateway Intents* den Regler für **Message Content Intent**.
4. Setzen Sie das Bot-Token zurück und kopieren Sie es.
5. Generieren Sie unter *OAuth2 -> URL Generator* einen Einladungslink mit dem Scope `bot` und den Berechtigungen `Send Messages` und `Read Message History`. Fügen Sie den Bot Ihrem Server hinzu.

### 2. Node-RED vorbereiten
1. Installieren Sie die Discord-Bibliothek über das Node-RED-Menü (*Manage Palette -> Install*):
   `node-red-contrib-discord-updated` (oder eine vergleichbare, gepflegte Version).
2. Importieren Sie die bereitgestellte `flow.json` in Ihre Node-RED-Instanz.
3. Konfigurieren Sie die Knoten:
   * Öffnen Sie den Knoten **Listen to Messages** (Discord Rx) und hinterlegen Sie Ihr Bot-Token.
   * Stellen Sie sicher, dass der Knoten **Send Discord Response** (Discord Tx) dieselbe Verbindung nutzt.
4. Klicken Sie auf **Deploy**.

---

## Wie funktioniert es?

Sobald der Bot online und mit Node-RED verbunden ist, reagiert er auf folgende Chat-Befehle:

### Spielbefehle:
* `!bs help` – Öffnet die Anleitung und zeigt die Legende der Emojis.
* `!bs start` – Startet ein schnelles Einzelspieler-Spiel gegen die KI.
* `!bs challenge @Nutzer` – Fordert ein anderes Server-Mitglied zu einem PvP-Duell heraus.
* `!bs accept` – Nimmt eine offene Herausforderung an und startet das PvP-Match.
* `!bs decline` – Lehnt eine offene Herausforderung ab.
* `!bs fire [Koordinate]` (z. B. `!bs fire C4`) – Feuert eine Salve auf das angegebene Feld.
* `!bs board` – Zeigt den aktuellen Zwischenstand der Spielfelder an.
* `!bs forfeit` – Beendet die aktuelle Partie vorzeitig und deckt alle geheimen Schiffspositionen auf.

### Spielfeld-Symbole:
* 🟦 – Unbeschossenes Wasser
* 🚢 – Eigenes Schiff (Nur im PvE-Modus auf dem eigenen Board sichtbar)
* 💥 – Direkter Treffer
* ⚪ – Fehlschuss
