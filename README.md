
Um eine Automation in Home Assistant zu erstellen, die Backups löscht, die älter als x Tage sind, kannst du den folgenden Ansatz verwenden. Dazu benötigst du ein Skript, das regelmäßig ausgeführt wird, um alte Dateien aus deinem Backup-Ordner zu entfernen.

Hier sind die Schritte, um die Automation zu erstellen:

1. **Terminal & SSH Add-On installieren**:
Installiere das Terminal & SSH Add-On, falls noch nicht vorhanden, um auf das Dateisystem von Home Assistant zuzugreifen.

3. **Shell-Skript erstellen, das alte Dateien löscht**:
   Erstelle eine neue Datei namens `delete_old_backups.sh` im `config`-Verzeichnis und füge folgenden Inhalt ein:

   ```bash
   #!/bin/bash
   # Verzeichnis, wo die Backups gespeichert sind
   BACKUP_DIR="/pfad/zu/deinem/freigabeordner"
   # Lösche Dateien, die älter als x Tage sind
   find "$BACKUP_DIR" -type f -mtime +x -exec rm {} \;
   ```

   Ersetze `x` mit der gewünschten Anzahl an Tagen.

   Beispiel für 5 Tage:

   ```bash
   find "$BACKUP_DIR" -type f -mtime +5 -exec rm {} \;
   ```

   Ersetze `/pfad/zu/deinem/freigabeordner` mit dem tatsächlichen Pfad zu deinem Backup-Ordner,
   z. B. `/share/synology_backup_daten`.

4. **Skript ausführbar machen**:
   Öffne das Terminal in Home Assistant und führe folgenden Befehl aus:

   ```bash
   chmod +x /config/delete_old_backups.sh
   ```

5. **Automation in Home Assistant erstellen**:
   Erstelle eine Automation, um das Skript regelmäßig auszuführen. Öffne die `automations.yaml`-Datei (oder nutze den Automation-Editor in der Home Assistant-Oberfläche) und füge folgendes hinzu:

   ```yaml
   alias: "Alte Backups löschen"
   trigger:
     - platform: time
       at: "0x:00:00"  # Führe das Skript täglich um x Uhr aus
   action:
     - service: shell_command.delete_old_backups
   ```

   Ersetze `x` mit der gewünschten Uhrzeit.

6. **Shell Command in der `configuration.yaml`-Datei erstellen**:

   ```yaml
   shell_command:
     delete_old_backups: /bin/bash /config/delete_old_backups.sh
   ```

7. **Home Assistant neu starten**: Starte Home Assistant neu, damit die Änderungen wirksam werden.

Jetzt wird die Automation täglich zur angegebenen Uhrzeit ausgeführt und löscht alle Dateien im Backup-Ordner, die älter als x Tage sind.

