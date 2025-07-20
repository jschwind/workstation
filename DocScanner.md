[Workspace](ReadMe.md) / Fujitsu iX500 + Raspberry Pi: Automatischer Farb-Duplex-Scan mit OCR

# 📄 Fujitsu iX500 + Raspberry Pi: Automatischer Farb-Duplex-Scan mit OCR

---

## ✅ Ziel:

* Der **ScanSnap iX500 wird per USB** mit dem **Raspberry Pi** verbunden.
* **Ein Tastendruck am Scanner** löst einen **beidseitigen Farbscan** aus.
* Das Ergebnis ist ein **durchsuchbares PDF (OCR)**.
* Die Datei wird automatisch in **`/home/pi/scans/`** gespeichert.

---

## 🧰 Was du brauchst:

* Raspberry Pi 3B (oder neuer)
* microSD-Karte (min. 16GB empfohlen)
* USB-Kabel für den Scanner
* Fujitsu ScanSnap iX500
* PC oder Mac mit Kartenleser (zum Schreiben des Images)

---

# 🧾 Schritt-für-Schritt Anleitung

---

## 🔹 1. Raspberry Pi OS auf SD-Karte installieren

### A) Raspberry Pi Imager herunterladen

➡️ [Offizielle Website](https://www.raspberrypi.com/software)

### B) SD-Karte vorbereiten

1. Imager starten
2. System wählen: `Raspberry Pi OS Lite (64-bit)`
3. SD-Karte auswählen
4. ⚙️ Einstellungen (empfohlen aktivieren):

   * Hostname: `raspberrypi`
   * Benutzername: `pi`, Passwort: `raspberry`
   * SSH aktivieren (optional)
5. Klicke auf „**Schreiben**“

---

## 🔹 2. Raspberry Pi starten & Scanner anschließen

1. SD-Karte in den Pi einlegen
2. Scanner per USB verbinden
3. Bildschirm & Tastatur anschließen (oder SSH verwenden)
4. Pi mit Strom versorgen → startet automatisch

---

## 🔹 3. Terminal öffnen & System vorbereiten

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y sane-utils scanbd ocrmypdf tesseract-ocr tesseract-ocr-deu imagemagick ghostscript
```

---

## 🔹 4. Scanner testen

```bash
scanimage -L
```

🟢 Ausgabe sollte etwa lauten:

```
device `fujitsu:ScanSnap iX500:...` is a Fujitsu ScanSnap iX500
```

---

## 🔹 5. Scan-Skript erstellen

### A) Verzeichnis für Scans anlegen

```bash
mkdir -p ~/scans
```

### B) Scan-Skript erstellen

```bash
sudo nano /usr/local/bin/scan.sh
```

**Inhalt:**

```bash
#!/bin/bash

set -e

TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
OUTDIR="/home/pi/scans"
TMPDIR="/tmp/scansnap_$TIMESTAMP"
OUTPDF="$OUTDIR/scan_$TIMESTAMP.pdf"
OCRPDF="$OUTDIR/scan_$TIMESTAMP_ocr.pdf"

mkdir -p "$OUTDIR" "$TMPDIR"

scanimage \
  --device-name "fujitsu:ScanSnap iX500" \
  --source "ADF Duplex" \
  --resolution 300 \
  --mode Color \
  --format=tiff \
  --batch="$TMPDIR/page_%03d.tiff"

convert "$TMPDIR"/page_*.tiff "$OUTPDF"
ocrmypdf --language deu "$OUTPDF" "$OCRPDF"

rm -rf "$TMPDIR" "$OUTPDF"

echo "OCR-PDF gespeichert unter: $OCRPDF"
```

### C) Skript ausführbar machen

```bash
sudo chmod +x /usr/local/bin/scan.sh
```

---

## 🔹 6. `scanbd` konfigurieren (Taste aktivieren)

### A) Saned deaktivieren

```bash
sudo systemctl stop saned.socket
sudo systemctl disable saned.socket
```

### B) Scan-Aktion definieren

```bash
sudo nano /etc/scanbd/scanbd.conf
```

➡️ Im Block `action scan {` folgendes ändern:

```conf
script = "/usr/local/bin/scan.sh"
```

**Speichern mit `Strg+O`, beenden mit `Strg+X`.**

---

## 🔹 7. `scanbd` aktivieren & starten

```bash
sudo systemctl enable scanbd
sudo systemctl start scanbd
```

---

## 🔹 8. Scan per Taste testen

1. Stelle sicher:

   * Raspberry Pi ist eingeschaltet
   * Scanner ist per USB verbunden und aktiviert
2. Lege ein Dokument in den ADF
3. **Drücke die blaue Scan-Taste am iX500**

📁 Das Ergebnis erscheint nach ca. 30 Sekunden in:

```
/home/pi/scans/scan_YYYY-MM-DD_HH-MM-SS_ocr.pdf
```

---

## 🟢 Geschafft!

Du hast nun:

✔️ **Farb- & Duplex-Scan per Tastendruck**
✔️ **OCR-fähige PDF-Dateien automatisch erstellt**
✔️ **Keine Windows-/Mac-Software mehr nötig**