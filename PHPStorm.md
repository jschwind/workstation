[Workspace](ReadMe.md) / PHPStorm

# PHPStorm

### Optimieren von Manjaro für PHPStorm

    sudo nano /etc/sysctl.d/idea.conf

Folgenden Inhalt einfügen und speichern

    fs.inotify.max_user_watches = 524288

Zum Übernehmen folgendes ausführen

    sudo sysctl -p --system

IDE neu starten.