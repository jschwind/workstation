[Workspace](ReadMe.md) / PHPStorm – Einrichtung & Optimierung

# 🧠 PHPStorm – Einrichtung & Optimierung

Anleitung zur Konfiguration von PHPStorm unter Manjaro inkl. Komfortfunktionen und Fehlerbehebung bei GPG-Problemen.

---

## ✂️ Smart Paste – Escape in Strings aktivieren

> Navigiere zu:

```
File > Settings > Editor > General > Smart Keys > PHP
```

Aktiviere:

```
[x] Escape symbols on paste in string literals
```

Dadurch werden beim Einfügen von Inhalten in String-Literale automatisch Sonderzeichen escaped.

---

## 🚀 Manjaro für PHPStorm optimieren

Manjaro hat standardmäßig zu wenige Dateibeobachter aktiviert. PHPStorm verwendet viele davon zur Projektüberwachung.

### 🛠️ Systemwert erhöhen

1. Datei öffnen:

```bash
sudo nano /etc/sysctl.d/idea.conf
```

2. Folgenden Inhalt einfügen:

```
fs.inotify.max_user_watches = 524288
```

3. Änderungen übernehmen:

```bash
sudo sysctl -p --system
```

4. **PHPStorm neu starten**, damit die neue Einstellung wirksam wird.

---

## 🧩 GPG-Probleme bei Git-Commits in PHPStorm

### ❌ Fehler:

```bash
error: gpg failed to sign the data
```

### ✅ Lösung 1: Pinentry installieren

```bash
sudo pacman -S pinentry pinentry-qt
```

---

### ✅ Lösung 2: `.bashrc` anpassen

```bash
nano ~/.bashrc
```

Am Ende einfügen:

```bash
GPG_TTY=$(tty)
export GPG_TTY
```

Anschließend:

```bash
source ~/.bashrc
```

---

### ✅ gpg-agent konfigurieren

```bash
nano ~/.gnupg/gpg-agent.conf
```

#### Standardkonfiguration:

```bash
pinentry-program /usr/bin/pinentry-qt
allow-loopback-pinentry
```

#### Optional: Passwort speichern mit Gnome-Dialog

```bash
pinentry-program /usr/bin/pinentry-gnome3
```

#### Timeout für Passwortspeicherung auf 8 Stunden setzen:

```bash
allow-loopback-pinentry
default-cache-ttl 28800
max-cache-ttl 28800
```

### 🔄 GPG-Agent neu starten:

```bash
gpgconf --kill gpg-agent
```