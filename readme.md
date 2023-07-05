# Workstation

## Vorbereitungen
- PC anschließen
- Erstellen eines bootfähigen USB-Sticks mit Manjaro
- Download unter https://manjaro.org/download/ > X86_64 > Plasma Desktop

## Manjaro installieren
- tz=European/Berlin, keytable=de, lang=de_DE
- "Boot with open source drivers" auswählen
- "Installer starten" klicken

### Installer: Willkommen
- Deutsch auswählen

### Installer: Standort
- Region: Europe
- Zeitzone: Berlin

### Installer: Tastatur
- "German" > "Default" auswählen

### Installer: Partitionen
- "Festplatte löschen" auswählen > "Kein Swap" > "ext4"
- "Verschlüsseltes System" wählen und Passwort eintragen

### Installer: Benutzer
- Alle Felder entsprechend ausfüllen

### Installer: Office-Paket
- LibreOffice (optional)

## Manjaro erste Schritte

### Uhr und Zeitzone

- Über die Uhrzeit in die Einstellungen > "Zeitzonen" > "Zeitzone des Sysmtems wechseln" > "Datum & Uhrzeit"
- "Datum und Uhrzeit automatisch einstellen" anhaken und dann "Anwenden" 

### Software hinzufügen/entfernen  (optional)

- Über das Burgermenü in die Einstellungen > Drittanbeiter
- "AUR Unterstützung" und "Auf Updates prüfen" aktivieren

## Software

### Folgende Software über "Software hinzufügen/entfernen" installieren

- Dropox (AUR) (optional)
- KeePass Password Safe (keepass) > xdotool
- Chromium Web Browser (chromium)
- spotify (AUR) (optional)
- docker (Alle 3 anhaken)
- veracrypt
- yakuake
- Remmina (Free RDP)
- docker-compose

### JetBrains Toolbox App installieren

- JetBrains Toolbox App (https://www.jetbrains.com/de-de/toolbox-app/) installieren
- Einloggen
- PHP Storm installieren

## Einstellungen

### Autostart

Folgenden Programm in den Autostart 

- yakuake
- dropbox
- jetbrain toolbox

### Git

Git Benutzereinstellungen global im System hinterlegen:

    git config --global user.email "name@domain.tld"
    git config --global user.name "name"

### Docker

Docker ohne root ausführen und starten

    sudo systemctl start docker.service
    sudo systemctl enable docker.service
    sudo usermod -aG docker $USER

## Workarounds

### Git Probleme mit PHP Storm

Sollte das Absenden von Commits mit der Fehlermeldung `error: gpg failed to sign the data` enden, liegt es an einer älteren curl Version. Um diesen Fehler zu beheben, muss man die `.bashrc` bearbeiten.

    nano ~/.bashrc

Änderung in der Datei


    #
    # ~/.bashrc
    #
    
    GPG_TTY=$(tty)  <- Diese Zeile muss rein
    export GPG_TTY  <- Diese Zeile muss rein

Wenn es dann immer noch nicht geht Folgendes:

    nano ~/.gnupg/gpg-agent.conf

Folgender Inhalt

    pinentry-program /usr/bin/pinentry-qt
    allow-loopback-pinentry

### Hilfen zum Aufräumen des Systems

Um die Paketdatenbank zu aktualisieren und alle Pakete auf dem System zu aktualisieren

    sudo pacman -Syu

Um eine vollständige Auffrischung der Paketdatenbank zu erzwingen und alle Pakete auf dem System zu aktualisieren. Dies ist erforderlich für das Umschalten von Auslieferungszweigen oder das Umschalten von Spiegelservern.

    sudo pacman -Syyu

Um alle installierten "verwaisten" Pakete anzuzeigen, von denen kein anderes Paket abhängt und welche somit nicht mehr nötig sein sollten:

    pacman -Qdt

Um alle verwaisten Pakete zu entfernen:

    sudo pacman -Rs $(pacman -Qdtq)

Um den Cache von Paketen zu bereinigen, die nicht mehr installiert sind, gib das folgende Kommando ein:

    sudo pacman -Sc


Quelle: https://wiki.manjaro.org/index.php/Pacman_Overview/de