# 102 Installation und Paketverwaltung

## 102.1 Fesplattenaufteilung

### Größe planen

Warum Verzeichnisbaum auf meherere Partitionen aufteilen?

* Überfüllung einer Partition unerheblich für Stabilität des Systems.
* Datenaufbewahrung und Datensicherung folgen einer gemeinsamen Logik.
* Dynamischer Inhalt / häufige Sicherung:
  * `/ ......... 40 GB minus alle separierten Partitionen`
  * `/home ..... Größe abhängig von der Gesamtdatenmenge aller Benutzer`
  * `/var ...... 10 GB`
* Statischer Inhalt / seltene oder keine Sicherung:
  * `/boot ..... 500 MB`
  * `/boot/efi . 100 MB (ESP - wird automatisch erstellt)`
  * `/usr ...... 20 GB`
  * `/opt ...... keine Angabe`
* Temporärer Inhalt / keine Sicherung:
  * `/proc ..... virtuell`
  * `/dev ...... virtuell`
  * `/tmp ...... 500 MB`
  * `SWAP ...... Doppelter RAM`
* Datenbanken üblich unter `/var`

### ESP (EFI Systempartition)

* ESP wird bei Installation des BS automatisch angelegt.
* ESP Gebräuchliche Größe: 100 MB.
* Die ESP ist unter `/boot/efi` gemountet (Standard)
* GPT (GUID Partition Table) als Partitionsschema.
* GPT bietet bis zu 128 Partitionen.
* Der MBR wird geschützt (protective).
* Daten der GPT ausgeben:
  * `gdisk -l /dev/sda`

### LVM (Logical Volume Manager)

* LVM wird über das Paket `lvm2` angeboten.
* Abstraktion physikalischer Datenträger gegenüber Dateisystem.
* RAID: Redundanz möglich, LVM: keine Redundanz.
* logisches Volumen kann nachträglich durch ... werden:
  * Hinzufügen von Partitionen vergrößert
  * Entfernen von Partitionen verkleinert
* Vorteile von LVM und RAID lassen sich kombinieren:
  * LVM auf bestehendem RAID-ARRAY einrichten
* LVM-Komonenten:
  * pv (Physikalische Volumen)
    * vergleichbar mit Partitionen
    * mit `fdisk` (Dateisystemtyp 8E) vorbereiten
  * vg (Volumen-Gruppen)
    * Zusammenschluß aus vielen pv
    * Nachträgliches Hinzufügen/Entfernen von vg möglich
  * lv (Logische Volumen)
    * werden in vg's erstellt
    * sind aus Sicht des Dateisystems Partitionen
    * lv's werden gemountet
* LVM-Befehle auf lokalem System ermitteln:
  * `ls -l /sbin/pv* /sbin/lv* /sbin/vg*`
* Tools:
  * `fdisk`
  * `cfdisk`
* Dateisystem:
  * Linux LVM
  * 8E

## 102.2 Bootmanager

### GRUB-Legacy

* Ursprünglich zweistufig, heute dreistufig.
* Stage 1
  * Im MBR der Boot-Festplatte.
  * Kopie unter `/boot/grub/stage1`.
* Stage 1,5
  * Pfad: `/boot/grub/foo_stage1_5`
  * Beispiel: `/boot/grub/reiserfs_stage1_5`
  * Programme für Unterstützung verschiedener Dateisysteme.
* Stage 2
  * Pfad: `/boot/grub/stage2`
  * Bootmenü
  * Starten des Kernels
* GRUB-Legacy-Prompt:
  * Mit dem Bootloader per Kommandozeile interagieren.
  * System kann manuell gebootet werden.
  * Benötigte Informationen:
    * Position des Hauptverzeichnisses.
    * Position des Kernels
    * eventl. Position der initialen RAM-Disk.
  * Booten von der ersten Partition der ersten Festplatte:
    * `grub> root (hd0,0)`
    * `grub> kernel /boot/vmlinuz-2.6.23.1-10.fc7 root=/dev/sda2`
    * `grub> initrd /boot/initrd-2.6.23.1-10.fc7.img`
    * `grub> boot`
* Konfigurationsdateien
  * `/boot/grub/device.map`
    * Zuordnung der GRUB-Notation `(hd0)` zur Linux Notation `/dev/sda`.
  * `/boot/grub/menu.lst` = Softlink auf `/boot/grub/grub.conf`
    * Menüeinträge für die verschiedenen Betriebssysteme.

### GRUB 2

* Komplette Neuentwicklung
* Aktuelle Einstellungen für den Bootvorgang in:
* Konfiguration des Bootloaders wird unterhalb von
  * `/etc` bzw.
  * `/etc/default/grub` bzw.
  * `/etc/grub.d`
* editiert und mittels
  * `grub-mkconfig` oder
  * `grub2-mkconfig`
* in die Datei
  * `/boot/grub/grub.cfg` oder
  * `/boot/grub2/grub.cfg`
* generiert. Diese Datei nutzt dann der Bootloader.
* Nach Hinzufügen eines neuen Kernels:
  * `update-grub`
* GRUB 2 auf einem Datenträger für den Bootvorgang installieren:
  * `grub-install /dev/sd{x}`
