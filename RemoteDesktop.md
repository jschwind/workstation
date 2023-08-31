[Workspace](ReadMe.md) / RemoteDesktop

# RemoteDesktop

Alternative zu Remmina. Funktioniert mit mehreren Monitoren.

### Folgende Software über "Software hinzufügen/entfernen" installieren

- freerdp

### Verbindung zu Windows-PC herstellen

konsole:

    xfreerdp /u:{username} /v:{server} /d:{domain} /multimon

konsole mit Passwort:

    xfreerdp /u:{username} /v:{server} /d:{domain} /multimon /p:{password}

shellscript:

    #!/bin/bash

    # Benutzername, Server-Adresse und Domäne
    USERNAME="{username}"
    SERVER="{server}"
    DOMAIN="{domain}"

    # xfreerdp Befehl ausführen
    xfreerdp /u:$USERNAME /v:$SERVER /d:$DOMAIN /multimon


shellscript mit Passwort:

    #!/bin/bash

    # Benutzername, Passwort, Server-Adresse und Domäne
    USERNAME="{username}"
    SERVER="{server}"
    DOMAIN="{domain}"
    PASSWORD="{password}"

    # xfreerdp Befehl ausführen
    xfreerdp /u:$USERNAME /v:$SERVER /d:$DOMAIN /multimon /p:$PASSWORD

### Erstellen eines Desktop-Icons

    sudo nano /usr/share/applications/rdp.desktop

oder nur für den aktuellen Benutzer:

    nano ~/.local/share/applications/rdp.desktop

Inhalt:

    [Desktop Entry]
    Categories=Utility;
    Comment=
    Exec=konsole -e "{path}/rdp.sh"
    GenericName=
    Icon={path}/icon.png
    MimeType=
    Name=Remote
    Path=
    StartupNotify=true
    Terminal=false
    TerminalOptions=
    Type=Application
    X-KDE-SubstituteUID=false
    X-KDE-Username=

### Optionen

- `/sound` - Sound übertragen
- `/multimon` - Mehrere Monitore übertragen
- `/w:1920` - Breite des Fensters
- `/h:1080` - Höhe des Fensters

### Shortcuts

- Fenstermodus mit `STRG + ALT + ENTER`
- Vollbildmodus mit `STRG + ALT + ENTER`