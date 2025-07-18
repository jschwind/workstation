[Workspace](ReadMe.md) / Mattermost – Workstation Einrichtung

# 💬 Mattermost – Workstation Einrichtung

Anleitung zur Lösung des Login-Problems über die Desktop-App mit dem Schema `mattermost-dev://`.

---

## ❗ Problem: Login-Link funktioniert nicht

Beim Klick auf einen **Login-Link** mit dem Schema `mattermost-dev://` passiert nichts?

➡️ Grund: Der Link wird nicht vom Betriebssystem korrekt an die Mattermost-App weitergeleitet.

---

## 🛠️ Lösung: URL-Handler unter Linux korrekt registrieren

---

### ✅ 1. Prüfen, ob ein URL-Handler existiert

```bash
xdg-mime query default x-scheme-handler/mattermost-dev
```

📌 Ausgabe leer? → Kein Handler registriert → weiter mit Schritt 2

---

### ✅ 2. `.desktop`-Datei erstellen oder anpassen

1. Öffne ein Terminal
2. Erstelle oder bearbeite die Datei:

```bash
nano ~/.local/share/applications/mattermost.desktop
```

3. Inhalt einfügen:

```ini
[Desktop Entry]
Name=Mattermost
Exec=/opt/Mattermost/mattermost-desktop %u
Type=Application
NoDisplay=true
MimeType=x-scheme-handler/mattermost-dev;
```

> 🛠️ Passe den Pfad bei `Exec=` an den tatsächlichen Installationsort deiner Mattermost-App an! (z.B. `/opt`, Snap, Flatpak, etc.)

---

### ✅ 3. Handler registrieren

```bash
xdg-mime default mattermost.desktop x-scheme-handler/mattermost-dev
```

---

### ✅ 4. Testen

1. Gib in einem beliebigen Browser ein:

```
mattermost-dev://test
```

2. Die **Mattermost Desktop App sollte sich öffnen** (ggf. mit Fehlermeldung „ungültiger Token“ – das ist in diesem Fall gewollt).

---

### 🔄 5. Neustart oder Session neu laden

Manchmal greifen Änderungen erst nach einem Neustart der Sitzung oder des gesamten Systems.

```bash
reboot
```

---

### 🧪 Optional: Registrierung systemweit (für alle Benutzer)

Falls du den Handler systemweit setzen willst:

```bash
sudo cp ~/.local/share/applications/mattermost.desktop /usr/share/applications/
sudo xdg-mime default mattermost.desktop x-scheme-handler/mattermost-dev
```

---

## 🧾 Hinweise zur Fehleranalyse

Falls es weiterhin nicht funktioniert:

* Logs der Mattermost-App prüfen
* Den Pfad in der `.desktop`-Datei **exakt prüfen**
* Bei Flatpak/Snap ggf. ein Wrapper-Skript für `Exec=` nutzen
* Testweise `xdg-open "mattermost-dev://test"` im Terminal ausführen
