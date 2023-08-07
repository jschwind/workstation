[Workspace](ReadMe.md) / Securepoint

# Securepoint

### VPN unter Manjaro konfigurieren

 - Unter https://domain.tld:port/ einloggen und die VPN-Verbindung "Konfiguration und Zertifikat" herunterladen
 - Zip-Datei entpacken (Am besten nach Dokumente, denn die Zertifikate können nicht nach dem Importieren nicht gelöscht werden)
 - Im Programm "Verbindungen" eine "Neue Verbindung hinzufügen" vom Typ "VPN-Verbindung importieren" importieren
 - Diese Verbindung auswählen und "Benutzername" und "Passwort", ohne 2. Faktor eintragen
 - Danach auf "Erweitert..." > "Sicherheit" und "Chiffre" und "HMAC-Authentifizierung" auf "Standard" setzen
 
Sollte ein eigener Nameserver verwendet werden, so muss dieser unter "IPv4" und ggf. "IPV6" eingetragen werden.

