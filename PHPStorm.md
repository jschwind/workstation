[Workspace](ReadMe.md) / PHPStorm

# PHPStorm

### Escape von String in PHPStorm aktivieren

    File > Settings > Editor > General > Smart Keys > PHP > [ ] Escape symbols on paste in string literals

### Optimieren von Manjaro für PHPStorm

    sudo nano /etc/sysctl.d/idea.conf

Folgenden Inhalt einfügen und speichern

    fs.inotify.max_user_watches = 524288

Zum Übernehmen folgendes ausführen

    sudo sysctl -p --system

IDE neu starten.