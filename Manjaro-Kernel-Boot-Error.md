[Workspace](ReadMe.md) / Manjaro – Kernel Boot Error

# 🛑 Manjaro – Kernel Boot Error: „Datei xxx nicht gefunden“

Dieser Fehler tritt meist auf, wenn die **`initramfs`-Datei fehlt oder beschädigt ist** – sie wird zum Laden des Kernels benötigt. Besonders bei verschlüsselten Systemen ist Vorsicht geboten, um Datenverlust zu vermeiden.

---

## 🧰 Was du brauchst

* Einen zweiten PC zum Erstellen eines Live-USB-Sticks
* Manjaro-ISO: [https://manjaro.org/download/](https://manjaro.org/download/)
* USB-Stick (mind. 4 GB)

---

## 🔧 Schritt-für-Schritt Reparatur

---

### ✅ 1. Live-System starten

* Erstelle einen bootfähigen USB-Stick mit dem **Manjaro ISO** (z.B. via [balenaEtcher](https://etcher.io/))
* Starte dein defektes System vom Stick (evtl. Bootmenü mit `F12`, `ESC` o.ä.)

---

### ✅ 2. Verschlüsselte Festplatte entsperren

> Terminal öffnen im Live-System:

```bash
sudo cryptsetup luksOpen /dev/sdX encrypted
```

🔍 Ersetze `/dev/sdX` durch die verschlüsselte Partition (findbar mit `lsblk`)

Beispiel:

```bash
lsblk
```

→ `/dev/sda2` ist meist die verschlüsselte Root-Partition.

---

### ✅ 3. Partitionen einbinden

```bash
sudo mount /dev/mapper/encrypted /mnt
```

#### Falls separate Boot- oder EFI-Partitionen vorhanden sind:

```bash
sudo mount /dev/sdX1 /mnt/boot
sudo mount /dev/sdX2 /mnt/boot/efi
```

(*Passe `/dev/sdX1` usw. an deine Gegebenheiten an – siehe `lsblk`*)

---

### ✅ 4. chroot: In dein System wechseln

```bash
sudo manjaro-chroot /mnt
```

→ Du befindest dich nun im reparierbaren System.

---

### ✅ 5. Initramfs wiederherstellen

```bash
mkinitcpio -P
```

➡️ Erstellt die `initramfs`-Dateien für alle installierten Kernel neu.

---

### ✅ 6. GRUB-Bootloader aktualisieren

```bash
update-grub
```

---

### ✅ 7. Neustart vorbereiten

```bash
exit
sudo umount -R /mnt
sudo reboot
```

Entferne beim Neustart den USB-Stick.

---

## 🧪 Fehler besteht weiterhin?

### 🔄 Kernel neu installieren (in chroot)

Falls `mkinitcpio` nicht hilft, kann ein beschädigter Kernel schuld sein. Installiere z.B. Kernel 6.1 LTS:

```bash
mhwd-kernel -i linux61
mkinitcpio -P
update-grub
```

→ Danach erneut neustarten.

---

## 💡 Tipps

* Verwende **LTS-Kernel** (z.B. `linux61`, `linux510`) für mehr Stabilität
* Mit `mhwd-kernel -li` siehst du installierte Kernel
* Mit `mhwd-kernel -r linuxXYZ` entfernst du alte Kernel
