[Workspace](ReadMe.md) / Manjaro

# 🖥️ Manjaro – Workstation einrichten

Schritt-für-Schritt-Anleitung zur Installation, Konfiguration und Softwarebereitstellung eines Manjaro-Arbeitsplatzes.

---

## 🔧 Vorbereitungen

1. **Mini-PC anschließen**
   [🛒 Mini-PC-Empfehlung](https://www.amazon.de/s?k=mini+pc+ryzen&tag=partyworms0c-21)

2. **Manjaro herunterladen**
   [➡️ Manjaro Download (Plasma Desktop, x86\_64)](https://manjaro.org/download/)

3. **Bootfähigen USB-Stick erstellen**
   [🛒 Bootfähiger USB-Stick](https://www.amazon.de/s?k=bootf%C3%A4higer+usb+stick&tag=partyworms0c-21)

---

## 💿 Manjaro installieren

1. Beim Booten `Boot with open source drivers` wählen
2. „Installer starten“ klicken

### 🗺️ Installer-Schritte

* **Sprache:** Deutsch
* **Standort:** Europe / Berlin
* **Tastatur:** German > Default
* **Partitionierung:**

    * „Festplatte löschen“
    * Kein Swap
    * ext4
    * Verschlüsseltes System (Passwort eingeben)
* **Benutzer:** alle Felder ausfüllen
* **Office (optional):** LibreOffice

---

## 🚀 Erste Schritte nach Installation

### 🕒 Zeitzone & Uhrzeit

> Einstellungen > Datum & Uhrzeit:

* „Datum und Uhrzeit automatisch einstellen“ aktivieren
* „Zeitzone des Systems wechseln“ → Berlin

### 🧰 AUR aktivieren (optional)

> Einstellungen > Software hinzufügen/entfernen > „Drittanbieter“:

* „AUR-Unterstützung aktivieren“
* „Auf Updates prüfen“ aktivieren

---

## 📦 Softwareinstallation (via Software-Center)

> **Empfohlene Pakete:**

| Paket                       | Beschreibung        | Optional                               |
| --------------------------- | ------------------- | -------------------------------------- |
| **chromium**                | Webbrowser          | 🔘 (alle Abhängigkeiten außer kwallet) |
| **docker + docker-compose** | Containerverwaltung | 🔘 (alle optionalen Abhängigkeiten)    |
| **dropbox (AUR)**           | Cloudspeicher       | ✅                                      |
| **flameshot**               | Screenshot-Tool     | 🔘                                     |
| **keepass**                 | Passwort-Manager    | 🔘                                     |
| **mattermost-desktop**      | Team-Chat           | ✅                                      |
| **remmina**                 | RDP-Client          | ✅                                      |
| **spotify (AUR)**           | Musikstreaming      | ✅                                      |
| **yakuake**                 | Drop-down Terminal  | evtl. schon installiert                |
| **veracrypt**               | Verschlüsselung     | ✅                                      |

---

## 🧰 JetBrains Toolbox + PHPStorm

1. [Toolbox App herunterladen](https://www.jetbrains.com/de-de/toolbox-app/)
2. App starten → Einloggen
3. PHPStorm installieren

---

## ⚙️ System-Einstellungen

### 🧬 Autostart

> Einstellungen > Autostart

* Dropbox (falls installiert)
* Flameshot
* JetBrains Toolbox
* Yakuake

### 🔐 Git konfigurieren

```bash
git config --global user.email "name@domain.tld"
git config --global user.name "Dein Name"
```

### 🐳 Docker aktivieren

```bash
sudo systemctl start docker.service
sudo systemctl enable docker.service
sudo usermod -aG docker $USER
```

### 🔑 Chromium + KeePass

1. [Kee-Passwortmanager Plugin](https://chrome.google.com/webstore/detail/kee-password-manager/mmhlniccooihdimnnjhamobppdhaolme) installieren
2. [KeePassRPC Plugin](https://github.com/kee-org/keepassrpc/tags) herunterladen
3. Plugin nach `/usr/share/keepass/Plugins/` kopieren:

```bash
sudo cp KeePassRPC.plgx /usr/share/keepass/Plugins/
```

---

## 🛠️ Workarounds & Tipps

### 🧱 Zweiten Kernel installieren (Backup)

> Anwendung „Kernel“ öffnen
> LTS-Kernel zusätzlich installieren (z.B. 6.6.x)

---

## 🧹 Systempflege & Aufräumen

| Befehl                            | Funktion                                |
| --------------------------------- | --------------------------------------- |
| `sudo pacman -Syu`                | System & Pakete aktualisieren           |
| `sudo pacman -Syyu`               | Erzwungene Spiegelserver-Aktualisierung |
| `pacman -Qdt`                     | Liste verwaister Pakete                 |
| `sudo pacman -Rs $(pacman -Qdtq)` | Verwaiste Pakete entfernen              |
| `sudo pacman -Sc`                 | Cache veralteter Pakete leeren          |

👉 Quelle: [Manjaro Wiki – Pacman Übersicht](https://wiki.manjaro.org/index.php/Pacman_Overview/de)
