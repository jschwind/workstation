[Workspace](ReadMe.md) / Securepoint

# 🔐 Securepoint – VPN unter Manjaro einrichten

Anleitung zur Einrichtung einer Securepoint-VPN-Verbindung unter Manjaro Linux.

---

## 🌐 1. VPN-Konfiguration herunterladen

1. Im Browser aufrufen:
   `https://domain.tld:port`
   *(URL je nach VPN-Zugang deiner Organisation)*

2. Einloggen mit Benutzername & Passwort

3. **VPN-Konfiguration + Zertifikat herunterladen**
   → ZIP-Datei wird gespeichert

---

## 🗂️ 2. Zertifikate vorbereiten

1. ZIP-Datei **entpacken** (z.B. in `~/Dokumente/Securepoint/`)
   ⚠️ **Wichtig:** Nicht direkt löschen – Zertifikate werden dauerhaft eingebunden.

---

## ➕ 3. VPN-Verbindung importieren

1. Öffne das Programm **„Verbindungen“** (Netzwerkmanager)

2. Klicke auf:

    * „+ Neue Verbindung hinzufügen“
    * Wähle **„VPN-Verbindung importieren“**

3. Navigiere zum entpackten Ordner und wähle die `.ovpn`-Datei

---

## 🔑 4. Zugangsdaten eintragen

1. Wähle die neue VPN-Verbindung aus
2. Trage **Benutzername** und **Passwort (ohne 2FA)** ein

---

## ⚙️ 5. Erweiterte Einstellungen

1. Klicke auf **„Erweitert...“**
2. Wechsle zu **„Sicherheit“**

    * **Chiffre:** `Standard`
    * **HMAC-Authentifizierung:** `Standard`

---

## 🌍 6. Nameserver eintragen (optional)

Wenn ein interner DNS-Server verwendet wird:

> Reiter: **IPv4** (ggf. auch **IPv6**)

* Methode: „Automatisch (nur Adressen)“
* DNS-Server: z.B. `192.168.1.1`

---

## ✅ Verbindung testen

* VPN aktivieren durch Klick auf das Netzwerksymbol
* Nach erfolgreicher Verbindung: IP-Adresse prüfen über z.B. [https://www.whatismyip.com](https://www.whatismyip.com)