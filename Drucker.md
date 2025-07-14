[Workspace](ReadMe.md) / Drucker

# Drucker MFC-L3750CDW unter Manjaro Linux einrichten

### ✅ **1. Drucker im Netzwerk verbinden**

Stelle sicher, dass dein Drucker:

* korrekt mit deinem Router verbunden ist (per LAN-Kabel).
* eine **feste IP-Adresse** hat (entweder im Druckermenü oder per DHCP-Reservierung im Router).

Du kannst die IP-Adresse des Druckers am Gerät anzeigen lassen:
**Menü → Druckberichte → Netzwerkkonfiguration**

---

### ✅ **2. Treiber installieren (empfohlen: Brother-Originaltreiber)**

Brother bietet Linux-Treiber, die auch unter Arch/Manjaro laufen.

Manjaro unterstützt AUR direkt. Öffne ein Terminal und tippe:

```bash
pamac build brother-mfc-l3750cdw
```

Diese Pakete installieren die passenden LPR- und CUPS-Treiber für dein Modell.

---

### ✅ **3. CUPS aktivieren und Drucker hinzufügen**

#### CUPS Webinterface aktivieren:

1. Stelle sicher, dass CUPS läuft:

   ```bash
   sudo systemctl enable --now cups.service
   ```

2. Öffne das CUPS-Webinterface im Browser:

   ```
   http://localhost:631
   ```

3. Gehe zu **Verwaltung → Drucker hinzufügen**

    * Benutzername: `root` oder dein Nutzername

    * Druckerprotokoll: wähle z.B. **"LPD/LPR Host or Printer"**

    * URL (ersetze `192.168.1.100` durch die IP deines Druckers):

      ```
      lpd://192.168.1.100/BINARY_P1
      ```

    * Wähle danach den passenden Treiber aus (z.B. „Brother MFC-L3750CDW CUPS“)

---

### ✅ **4. Testseite drucken**

Sobald der Drucker hinzugefügt ist, solltest du über das CUPS-Webinterface oder über das Druckmenü in z.B. LibreOffice eine **Testseite drucken** können.

---

### ✅ **5. Optional: Scanner einrichten (für XSane / SimpleScan)**

Installiere `brscan4` (auch über AUR):

```bash
pamac build brscan4
```

Dann:

```bash
brsaneconfig4 -a name=MFC-L3750CDW model=MFC-L3750CDW ip=192.168.1.100
```

Nun sollte der Scanner in `Simple Scan` oder `XSane` funktionieren.
