[Workspace](ReadMe.md) / Brother MFC-L3750CDW – Drucker unter Manjaro einrichten

# 🖨️ Brother MFC-L3750CDW – Drucker unter Manjaro einrichten

Anleitung zur Installation von Drucker und Scanner-Funktion unter Manjaro Linux mit AUR-Unterstützung.

---

## ✅ 1. 📡 Drucker im Netzwerk verbinden

Stelle sicher, dass dein Drucker:

* über **LAN-Kabel** mit dem Router verbunden ist
* eine **feste IP-Adresse** hat (am Gerät oder via DHCP-Reservierung)

### 🔍 IP-Adresse herausfinden:

> Menü am Drucker:
> `Menü → Druckberichte → Netzwerkkonfiguration`
> → Auf dem Ausdruck steht die aktuelle IP-Adresse (z.B. `192.168.1.100`)

---

## ✅ 2. 📦 Treiber installieren (AUR: Brother-Originaltreiber)

1. Terminal öffnen
2. AUR-Treiber installieren:

```bash
pamac build brother-mfc-l3750cdw
```

> 📦 Installiert LPR- und CUPS-Treiber speziell für dein Modell.

---

## ✅ 3. ⚙️ CUPS aktivieren & Drucker hinzufügen

### 🟢 CUPS starten und aktivieren:

```bash
sudo systemctl enable --now cups.service
```

### 🌐 CUPS Webinterface aufrufen:

[http://localhost:631](http://localhost:631)

### ➕ Drucker hinzufügen:

1. Menü: **Verwaltung → Drucker hinzufügen**
2. Authentifiziere dich (Benutzername z.B. `root` oder dein Nutzername)
3. Wähle: **„LPD/LPR Host or Printer“**

🔗 URL eingeben (IP-Adresse anpassen):

```
lpd://192.168.1.100/BINARY_P1
```

4. Wähle passenden Treiber:
   z.B. **„Brother MFC-L3750CDW CUPS“**

---

## ✅ 4. 🧪 Testseite drucken

1. Im CUPS-Webinterface oder
2. über eine beliebige Anwendung (z.B. **LibreOffice**)
   → **Testseite drucken** und prüfen, ob der Drucker korrekt arbeitet

---

## ✅ 5. 📠 Scannerfunktion aktivieren (Simple Scan / XSane)

### 📦 Scanner-Treiber installieren:

```bash
pamac build brscan4
```

### ➕ Gerät registrieren:

```bash
brsaneconfig4 -a name=MFC-L3750CDW model=MFC-L3750CDW ip=192.168.1.100
```

> Nun ist der Scanner nutzbar in:

* **Simple Scan**
* **XSane**
* **gscan2pdf** (für OCR & PDF)
