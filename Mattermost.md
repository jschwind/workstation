[Workspace](ReadMe.md) / Mattermost

# Mattermost - Workstation

## Problem beim Logintoken über Desktop App

Das Problem mit dem benutzerdefinierten URL-Schema `mattermost-dev://`, das nicht funktioniert, könnte auf eine fehlende oder falsch konfigurierte Anwendungseinstellung hinweisen, die verhindert, dass dein Betriebssystem diese URLs korrekt an die Mattermost-Anwendung weiterleitet. Hier sind detaillierte Schritte, um dieses Problem zu untersuchen und möglicherweise zu beheben:

### 1. Überprüfung der URL-Handler-Registrierung

Unter Linux müssen benutzerdefinierte URL-Schemata explizit im System registriert werden, damit sie mit den entsprechenden Anwendungen verknüpft werden können. Hier sind einige Schritte, um zu überprüfen und zu konfigurieren, wie dein System `mattermost-dev://` URLs handhabt:

#### Überprüfe vorhandene URL-Handler

- Öffne ein Terminal und führe folgenden Befehl aus, um zu sehen, ob ein Handler für `mattermost-dev` registriert ist:
  ```bash
  xdg-mime query default x-scheme-handler/mattermost-dev
  ```

Wenn dies keine Ausgabe liefert oder der falsche Anwendungshandler angezeigt wird, musst du einen Handler manuell registrieren.

#### Registriere den URL-Handler

- Du musst eine `.desktop` Datei für Mattermost erstellen, falls noch nicht vorhanden, oder die bestehende anpassen. Diese Datei befindet sich normalerweise im Verzeichnis `/usr/share/applications/` oder `~/.local/share/applications/`. Die Datei sollte in etwa so aussehen:

  ```ini
  [Desktop Entry]
  Name=Mattermost
  Exec=/pfad/zu/mattermost %u
  Type=Application
  NoDisplay=true
  MimeType=x-scheme-handler/mattermost-dev;
  ```

- Füge die URL-Schemata hinzu und stelle sicher, dass der `Exec` Pfad zum Mattermost-Client zeigt, wobei `%u` dafür steht, dass die URL an die Anwendung übergeben wird.

- Nachdem die `.desktop` Datei erstellt oder bearbeitet wurde, führe den folgenden Befehl aus, um die Änderungen im System zu registrieren:
  ```bash
  xdg-mime default mattermost.desktop x-scheme-handler/mattermost-dev
  ```

  Ersetze `mattermost.desktop` mit dem tatsächlichen Dateinamen deiner `.desktop` Datei.

### 2. Testen des URL-Handlers

- Nachdem du den URL-Handler registriert hast, kannst du testen, ob es funktioniert, indem du im Browser `mattermost-dev://test` eingibst. Dies sollte versuchen, die Mattermost-Anwendung zu öffnen.

### 3. Neustart und Überprüfung

- Manchmal können Änderungen erst nach einem Neustart des Systems oder der Sitzung vollständig übernommen werden. Starte deinen Computer neu und versuche dann erneut, die URL zu öffnen.

### 4. Überprüfe die Konfiguration und Protokollierung

- Wenn das Problem weiterhin besteht, überprüfe die Konfigurationseinstellungen und Protokolle von Mattermost für zusätzliche Fehlermeldungen, die darauf hinweisen könnten, warum das URL-Schema nicht funktioniert.

Durch das Befolgen dieser Schritte kannst du sicherstellen, dass dein System richtig konfiguriert ist, um benutzerdefinierte URL-Schemata für Mattermost zu behandeln. Wenn diese Lösungen nicht funktionieren, könnte es sich um ein spezifischeres Problem mit deiner Mattermost-Installation oder deinem Betriebssystem handeln. In diesem Fall könnte eine Anfrage im Mattermost-Forum oder eine genauere Fehleruntersuchung erforderlich sein.