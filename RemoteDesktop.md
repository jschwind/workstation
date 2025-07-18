[Workspace](ReadMe.md) / Remote Desktop – Verbindung zu Windows über FreeRDP

# 💻️ Remote Desktop – Verbindung zu Windows über FreeRDP

Alternative zu Remmina – unterstützt mehrere Monitore und flexible Konfiguration.

---

## 📦 Softwareinstallation

> Öffne **„Software hinzufügen/entfernen“** und installiere:

* `freerdp` (Paketname)

---

## 🔌 Verbindung zu Windows-PC herstellen

### 📄 Direkt in der Konsole

**Ohne Passwort:**

```bash
xfreerdp /u:{username} /v:{server} /d:{domain} /multimon
```

**Mit Passwort:**

```bash
xfreerdp /u:{username} /v:{server} /d:{domain} /multimon /p:{password}
```

---

## 🖋️ Shellscript erstellen (Start per Klick möglich)

### Variante ohne Passwort:

```bash
#!/bin/bash

# Benutzername, Server-Adresse und Domäne
USERNAME="{username}"
SERVER="{server}"
DOMAIN="{domain}"

# Remote Desktop starten
xfreerdp /u:$USERNAME /v:$SERVER /d:$DOMAIN /multimon
```

### Variante mit Passwort:

```bash
#!/bin/bash

# Benutzername, Passwort, Server-Adresse und Domäne
USERNAME="{username}"
PASSWORD="{password}"
SERVER="{server}"
DOMAIN="{domain}"

# Remote Desktop starten
xfreerdp /u:$USERNAME /v:$SERVER /d:$DOMAIN /multimon /p:$PASSWORD
```

> Speichern als z.B. `~/rdp.sh` und ausführbar machen:

```bash
chmod +x ~/rdp.sh
```

---

## 🖼️ Desktop-Verknüpfung erstellen

### A) Für alle Benutzer:

```bash
sudo nano /usr/share/applications/rdp.desktop
```

### B) Nur für aktuellen Benutzer:

```bash
nano ~/.local/share/applications/rdp.desktop
```

### Inhalt der Datei:

```ini
[Desktop Entry]
Name=Remote
Comment=Windows Remote Desktop
Exec=konsole -e "/home/pi/rdp.sh"
Icon=/home/pi/icon.png
Terminal=false
Type=Application
Categories=Utility;
StartupNotify=true
```

> Pfade ggf. anpassen (`/home/pi/rdp.sh`, Icon optional)

---

## ⚙️ Nützliche Optionen für `xfreerdp`

| Option      | Bedeutung                      |
| ----------- | ------------------------------ |
| `/sound`    | Audio vom Remote-PC übertragen |
| `/multimon` | Nutzung mehrerer Monitore      |
| `/w:1920`   | Fensterbreite                  |
| `/h:1080`   | Fensterhöhe                    |

---

## ⌨️ Tastenkombinationen

| Shortcut             | Funktion                                      |
| -------------------- | --------------------------------------------- |
| `STRG + ALT + ENTER` | Umschalten zwischen Fenstermodus und Vollbild |
