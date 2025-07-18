## ✅ Ziel:

* Der **iX500 wird per USB an den Raspberry Pi** angeschlossen.
* Durch **Drücken der Taste am Scanner** wird ein beidseitiger Farbscan ausgeführt.
* Das Ergebnis ist ein **durchsuchbares PDF (mit OCR)**.
* Dieses wird automatisch **in einem Ordner auf dem Raspberry gespeichert**.

---

## 🧰 Was du brauchst:

* Raspberry Pi 3B (oder neuer)
* microSD-Karte (mind. 16GB, besser 32GB)
* USB-Kabel für den Scanner
* Fujitsu ScanSnap iX500
* Windows-/Mac-PC mit SD-Kartenleser (zum Start)

---

# 🧾 **Schritt-für-Schritt Anleitung**

---

## 🔹 1. Raspberry Pi OS auf SD-Karte installieren

### A) Lade Raspberry Pi Imager herunter:

* Offizielle Seite: [https://www.raspberrypi.com/software](https://www.raspberrypi.com/software)

### B) SD-Karte vorbereiten:

1. Starte den Raspberry Pi Imager.
2. Wähle „Raspberry Pi OS (64-bit, empfohlen)“.
3. Wähle deine SD-Karte.
4. Klicke auf ⚙️ (Zahnrad) → aktiviere:

    * Hostname (z.B. `raspberrypi`)
    * SSH (optional, falls Fernzugriff gewünscht)
    * Benutzername: `pi`, Passwort: `raspberry`
5. Klicke auf „Schreiben“.

---

## 🔹 2. Raspberry Pi anschließen & starten

1. Stecke die SD-Karte in den Pi.
2. Verbinde Bildschirm, Tastatur, Maus (optional: per SSH verbinden).
3. Schließe den **iX500 per USB** an den Pi.
4. Stecke Strom ein → der Pi startet.

---

## 🔹 3. Terminal öffnen & System vorbereiten

Öffne ein Terminal (oder SSH) und gib Folgendes ein:

```bash
sudo apt update
sudo apt upgrade -y
```

Dann installiere benötigte Pakete:

```bash
sudo apt install -y sane-utils scanbd ocrmypdf tesseract-ocr tesseract-ocr-de imagemagick ghostscript
```

---

## 🔹 4. Scanner testen

Scanner einschalten, dann im Terminal prüfen:

```bash
scanimage -L
```

Wenn du sowas siehst wie:

```
device `fujitsu:ScanSnap iX500:...' is a Fujitsu ScanSnap iX500
```

✅ Alles gut! Scanner wird erkannt.

---

## 🔹 5. Scan-Skript anlegen

Erstelle ein Verzeichnis für Scans:

```bash
mkdir -p ~/scans
```

Dann das Skript öffnen:

```bash
sudo nano /usr/local/bin/scan.sh
```

Füge folgendes ein:

```bash
#!/bin/bash

set -e

TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
OUTDIR="/home/pi/scans"
TMPDIR="/tmp/scansnap_$TIMESTAMP"
OUTPDF="$OUTDIR/scan_$TIMESTAMP.pdf"
OCRPDF="$OUTDIR/scan_$TIMESTAMP_ocr.pdf"

mkdir -p "$OUTDIR"
mkdir -p "$TMPDIR"

scanimage \
  --device-name "fujitsu:ScanSnap iX500" \
  --source "ADF Duplex" \
  --resolution 300 \
  --mode Color \
  --format=tiff \
  --batch="$TMPDIR/page_%03d.tiff"

convert "$TMPDIR"/page_*.tiff "$OUTPDF"

ocrmypdf --language deu "$OUTPDF" "$OCRPDF"

rm -rf "$TMPDIR"
rm "$OUTPDF"

echo "Gescanntes OCR-PDF gespeichert unter: $OCRPDF"
```

Speichern mit `Strg+O`, dann `Enter`, beenden mit `Strg+X`.

**Skript ausführbar machen:**

```bash
sudo chmod +x /usr/local/bin/scan.sh
```

---

## 🔹 6. scanbd konfigurieren (Taste aktivieren)

### A) Deaktiviere alten saned-Dienst:

```bash
sudo systemctl stop saned.socket
sudo systemctl disable saned.socket
```

### B) `scanbd.conf` anpassen:

```bash
sudo nano /etc/scanbd/scanbd.conf
```

Suche den Block `action scan {` und ändere:

```conf
script = "/usr/local/bin/scan.sh"
```

Speichern mit `Strg+O`, `Enter`, beenden mit `Strg+X`.

---

## 🔹 7. scanbd aktivieren

```bash
sudo systemctl enable scanbd
sudo systemctl start scanbd
```

---

## 🔹 8. Test: Scan mit Taste auslösen

1. Stelle sicher, dass:

    * der Scanner eingeschaltet ist,
    * der Raspberry Pi läuft (scanbd ist aktiv),
2. Lege ein Dokument ein.
3. Drücke die **blaue Taste am iX500**.

➡️ Innerhalb ca. 30 Sekunden entsteht ein PDF unter:

```bash
/home/pi/scans/
```

Mit Namen wie:

```
scan_2025-07-18_13-12-55_ocr.pdf
```

---

## ✅ Geschafft!

Du hast jetzt:

* **OCR-fähige Duplex-Farbscans** auf deinem Raspberry Pi
* **automatische Verarbeitung per Tastendruck**
* **keine zusätzliche Software auf PC oder Mac nötig**
