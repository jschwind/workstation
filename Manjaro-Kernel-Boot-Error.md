# Fehler: Datei xxx nicht gefunden - Manjaro Kernel Boot Error
Das Problem scheint mit einer fehlenden oder beschädigten initramfs-Datei zusammenzuhängen, die für das Laden des Systems notwendig ist. Da die Festplatte verschlüsselt ist, ist es wichtig, vorsichtig vorzugehen, um keine Daten zu verlieren. Hier sind die Schritte, um das Problem zu lösen:

1. **Booten mit einem Live-System**:
   - Lade ein Manjaro-Live-ISO herunter und erstelle ein bootfähiges USB-Laufwerk.
   - Starte dein System vom Live-USB-Stick.

2. **Entschlüssele die Festplatte**:
   - Öffne ein Terminal und entschlüssele die Festplatte mit folgendem Befehl:
     ```bash
     sudo cryptsetup luksOpen /dev/sdX encrypted
     ```
     Ersetze `/dev/sdX` mit dem richtigen Gerätenamen deiner verschlüsselten Partition. Du kannst den Gerätenamen mit `lsblk` finden.

3. **Binde die Partition ein**:
   - Binde das Dateisystem ein:
     ```bash
     sudo /dev/mapper/encrypted /mnt
     ```
   - Falls separate Boot- oder EFI-Partitionen vorhanden sind, binde diese ebenfalls ein:
     ```bash
     sudo mount /dev/sdX1 /mnt/boot
     ```

4. **Chroot ins System**:
   - Wechsel in das installierte System:
     ```bash
     duso manjaro-chroot /mnt
     ```

5. **Repariere die initramfs-Datei**:
   - Stelle die initramfs-Datei wieder her:
     ```bash
     mkinitcpio -P
     ```
   - Dies generiert neue initramfs-Dateien für alle installierten Kernel.

6. **Überprüfe und aktualisiere GRUB**:
   - Aktualisiere den Bootloader:
     ```bash
     update-grub
     ```

7. **Neustart**:
   - Beende die chroot-Umgebung mit `exit`, unbinde die Partitionen mit `umount -R /mnt` und starte das System neu:
     ```bash
     reboot
     ```

Falls das Problem weiterhin besteht, könnte es an einem beschädigten Kernel oder einer unvollständigen Aktualisierung liegen. In diesem Fall wäre es hilfreich, den Kernel neu zu installieren. Das kannst du in der chroot-Umgebung mit folgendem Befehl tun:

```bash
mhwd-kernel -i linux6.1
```

Ersetze `linux6.1` durch die Kernel-Version, die du verwenden möchtest.