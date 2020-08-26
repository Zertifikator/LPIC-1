# 101   Systemarchitektur

## 101.1 Hardwareeinstellungen ermitteln und konfigurieren

### sysfs und /sys

* `/sys` wird vom Dateisystem `sysfs` erzeugt.
* stellt Informationen über geladene Treibermodule im Userspace zur Verfügung.
* Zusammenhänge werden über symbolische Links dargestellt.
* `ls -l /sys`

### /proc und Tools

```bash
cat /proc/modules               # aktuell geladene Module
cat /proc/interrupts            # aktuell verwendete Interrupts
cat /proc/ioports               # aktuell verwendete IO-Ports
cat /proc/dma                   # aktuell verwendete DMA-Kanäle (direct memory access)
ls /proc/pci                    # **veraltet (aktuelle Belegung des PCI-Busses)
ls /proc/bus/pci                # aktuelle Belegung des PCI-Busses
ls /proc/scsi                   # aktuelle Belegung des SCSII-Busses
ls /proc/scsi/usb-storage       # aktuell verwendete USB-Geräte
cat /proc/sys/kernel/osrelease  # Kernelversion
cat /proc/sys/kernel/ostype     # Typ des Betriebssystems (Linux)
cat /proc/sys/kernel/hostname   # Hostname des Systems
cat /proc/sys/kernel/modprobe   # Pfad zum Systemtool `modprobe`
lspci                           # Informationen zum PCI-Bus (/proc/bus/pci)
sudo lspci -vvv                 # Detaillierte Informationen zu allen PCI-Bus-Geräten
lsusb                           # Informationen zum USB-Bus (dev/bus/usb)
lsusb -t                        # -tree: Baumansicht
lsusb -d foo -v                 # -device: ein bestimmtes USB-Device detailliert anzeigen
```

### udev und /dev

* Udev erzeugt oder verwirft Gerätedateien unter `/dev` dynamisch.
* Bis 2006 war dies Aufgabe von `devfs`.
* Damals gab es auch Gerätedateien für physikalisch nicht vorhandene Geräte.
* Udev ist in systemd integriert.

```bash
ps ax | grep udev               # udev läuft?
/dev/sda, /dev/sdb              # Erstes und zweites SCSII/SATA-Blockdevice
/dev/sda1                       # Erste Partition des ersten SCSII/SATA-Blockdevices
/dev/hda, /dev/hdb              # Erstes und zweites IDE-Blockdevice
```

### uname

```bash
uname -r                        # Kernelversion
4.6.3-37-generic
-- 4                            # Major Release
-- 6                            # Minor Release
-- 3                            # Patch Level
-- -37-generic                  # optionale nähere Bezeichnung
uname -a                        # Infos zu Kernel und Betriebsystem
```

### lsmod, insmod, rmmod, modprobe

```bash
cat /proc/modules               # aktuell geladene Module
lsmod                           # menschenlesbar
insmod /lib/modules/4.6.3/kernel/drivers/usb/storage/usb-storage.ko
                                # Im Erfolgsfall keine Bestätigungsmeldung
rmmod usbcore                   # Entladen des Moduls ohne Pfadangabe
rmmod -f                        # Entladen erzwingen (force)
rmmod -v                        # Verbose-Mode
modprobe                        # Kombination aus insmod und rmmod
modprobe -t                     # -type: veraltet
modprobe -l                     # -list: veraltet; welche können geladen werden?
```

### dbus / hald

* D-Bus ist in aktuellen Distributionen vorhanden.
* HAL und damit `hald` wurde mittlerweise durch `dbus-daemon` aus `systemd` ersetzt?!

```bash
ps ax | grep dbus
```

## 101.2 Systemstart

### BIOS/UEFI

BIOS (Basic Input Output System) bzw. UEFI (Unified Extensible Firmware Interface) führen nach dem Rechnerstart den POST (Power-On-Self-Test) durch. Danach wird in der Bootreihenfolge ein Boot-Device gesucht. Der MBR des Boot-Devices wird in den RAM geladen und wenn er ausführbaren Code enthält, gestartet und die Kontrolle des weiteren Startvorgangs übergeben.

Ist kein initialer Bootloader vorhanden, sucht das BIOS in den Startbereichen derjenigen Partitionen nach ausführbarem Code, die in der Partitionstabelle mit der Markierung 0x80 versehen sind. Findet das BIOS keinen ausführbaren Bootcode, beendet es den Bootvorgang vorzeitig und gibt eine entsprechende akkustische und visuelle Meldung aus.

Aufbau des MBR:

| Start-Offset | Ende-Offset | Größe in Oktets | Funktion / Inhalt |
|:-------------|:------------|:---------------:|:------------------|
| 0x0000 | 0x01B7 | 440 | Startprogramm in Maschinencode |
| 0x01B8 | 0x01BB | 4 | Datenträgersignatur |
| 0x01BC | 0x01BD | 2 | Word mit dem Wert 0 |
| 0x01BE | 0x01FD | 64 | Primäre Partitionstabelle |
| 0x01FE | 0x01FF | 2 | Magic Word 0x55aa |

MBR ausgeben:
```
sudo dd if=$BLKDEV bs=1 count=512 status=none | od -A x -t x1z -v --endian=big | sed '$d'
```
### Neuerungen im UEFI
Das UEFI weißt gegenüber dem BIOS folgende Neuerungen auf:

* Grafisches Setup
* GPT-Unterstützung (GUID-Partitionstabelle)
* Booten von Festplatten bis 8 Zebibytes
* Netzwerk und Fernwartung
* Treiber auf Firmwareebene
* Integrierter Bootlader
* DRM

DRM verhindert im reinen UEFI-Bootmodus das Starten von nicht signierter Software, also auch von "unbekannten" Betriebssystemen.

* ESP (EFI System Partition) wird von UEFI zum Booten genutzt
  * FAT-Dateisystem
  * ESP wird unter /boot/efi bereitgestellt
  * enthält Bootloader
  * enthält eventuell den Kernel
  * GRUB benötigt den Kernel nicht in der ESP

### Kernel-Parameter

* Kernel-Parameter können vom Bootloader an den Kernel übergeben werden
  * dauerhaft über Konfiguration
    * /boot/grub/grub.cfg
    * linux /boot/vmlinuz-4.6.3 root=/dev/sda1 apm=off
  * temporär über den Boot-Prompt
    * Eintrag auswählen
    * Taste [E]
    * Editor öffnet
    * Zeile editieren
    * GRUB legacy: Tasten [ENTER], [B]
    * GRUB 2: Tasten [STRG]+[X]

Beispiel für temporären Parameter: `init=/bin/bash`

Bash startet ohne Passwortabfrage; Root-Dateisystem allerdings im Nur-Lese-Modus.
```
mount -o remount,rw /
passwd root
```
Argumente für dieses Leak:
* Prompt von GRUB ist abschaltbar
* nicht autorisierte Person darf ohnehin keinen physischen Zugang zu einem Rechner erlangen, da Diebstahl von Festplatte, Computer oder Sabotage nicht ausgeschlossen werden kann

### initrd, initramfs

* Image einer initialen Ramdisk
* `/boot/initrd`
* Systemstart-Module des Kernels sind in die initiale RAM-Disk eingebunden.
* Befehl zur Erzeugung einer neuen `/boot/initrd`
  * Red Hat: `mkinitrd`
  * Debian: `mkinitramfs`

### Protokolle des Bootvorgangs

* Kernel-Messages werden im Kernel-Ring-Puffer gespeichert
* Auslesen per `dmesg`, besser: `dmesg | less`
* Ältere Informationen in `/var/log/messages` und `/var/log/syslog`
* heutige Linux'e: `journalctl` oder `jouralctl -k` (-kernel) wg. _systemd_

## 101.3 Runlevel und Boot-Targets

* Der Begriff _Runlevel_ bezieht sich auf das SysVinit-Systeme
* Der Begriff _Boot-Target_ bezieht sich auf systemd-Systeme

Folgende Zustände können ein Runlevel bzw. Boot-Target spezifizieren:

* System Halt
* Single-User / Multi-User
* kein Netzwerk / Netzwerk
* Grafische Oberfläche GUI
* System Reboot

### SysVinit (init)

#### Runlevel im Normalfall:

* 0 = System herunterfahren
* 1/S/s = Single User Mode, kein Netzwerk, nur eine Anmeldung
* 2 = Multiuser ja, Netzwerk nein/ja, Grafikmodus nein
* 3 = Multiuser ja, Netzwerk ja, Grafikmodus nein/ja
* 4 = wird nicht genutzt
* 5 = Multiuser ja, Netzwerk ja, Grafikmodus ja
* 6 = System herunterfahren und neu starten

Der aktuelle Runlevel wird mit `runlevel` abgefragt.

[`/etc/inittab`](files/etc.inittab.md)  
[`man runlevel`](https://manpages.debian.org/testing/manpages-de/runlevel.8.de.html)

#### inittab

Hauptkonfigurationsdatei des init-Prozesses (auch Init-Deamon, PID 1, Vater aller Prozesse)`/sbin/init` ist `/etc/inittab`.  Die Zustände der einzelnen Runlevel 0 bis 6 sind in `/etc/inittab` beschrieben, wobei die Definition der Runlevel von Distri zu Distri unterschiedlich ist und keiner Normung unterliegt.

[`/etc/inittab`](files/etc.inittab.md)  
[`man inittab`](https://manpages.debian.org/testing/manpages-de/inittab.5.de.html)  
[`man init`](https://manpages.debian.org/testing/manpages-de/init.8.de.html)  
[`man telinit`](https://manpages.debian.org/testing/manpages-de/telinit.8.de.html)  
[`man login`](https://manpages.debian.org/testing/login/login.1.de.html)


Wichtige Schlagwörter/Komponenten:
* respawn = wieder hervorbringen
* Runlevel 1 ist anders konfiguriert ???!!!
* /bin/login
* /sbin/mingetty
* TTY = Teletyper
* `ca::ctrlaltdel:/sbin/shutdown -r +4`
* `telinit q` = inittab neu einlesen

#### Init-Scripte

Sowohl der Systemstart als auch der Wechsel von Runleveln erfolgt über Init-Scripte.

* Die Init-Scripte liegen in `/etc/init.d` oder `/etc/rc.d`
* Beispiel [`/etc/init.d/README`](/files/etc.init.d.README.md)
* Beispiel [`/etc/init.d/networking`](/files/etc.init.d.networking.md)
* Diese werden beispielweise in die Verzeichnisse `/etc/rc{RL}.d` verlinkt
* Dies geschieht mit dem Programm [`update-rc.d`](https://manpages.debian.org/buster/init-system-helpers/update-rc.d.8.en.html)
* Red Hat -> monolithisches Init-Script
* Andere ->  Einzelscripte
* Möglichkeiten Orte der Runlevel-Links für `init`:
  * `/etc/rc{RL}.d`
  * `/etc/init.d/rc{RL}.d`
  * `/etc/rc.d/rc{RL}.d`
* Link-Name (Beispiel K05sendmail, S13named, Kxxfoo):
  * S = Start
  * K = Kill
  * xx = Start/Killreihenfolge

Standardaktionen der Skripte:
```
start
restart
reload
status
```
Derzeitiger Runlevel:
```
runlevel
1 3
1=vorheriger RL
3=jetziger RL
wenn 'N', dann unbekannt
```
Runlevel wechseln:
```
sudo init 1
sudo telinit 1
sudo init 0      # System sofort herunterfahren, keine Benachrichtigungen
sudo init 6      # System sofort neu starten
```
System sauber herunterfahren und neu starten:
```
sudo shutdown -r    # reboot
    -h              # halt
    -t sekunden     # time
    -k              # nur Konsolenmeldungen
    -f              # kein fsck nach reboot
    -F              # fsck erzwingen

shutdown -f -r now  # beliebte Prüfungsfrage: Computer wird sofort neu gestartet und Festplatten werden nicht geprüft (nach Kernelkompilierung auf einem Produktionsserver mit sehr großen Partitionen)
```
Unsaubere Methoden:
```
halt
reboot
poweroff
```
#### acpid
Der _acpid_ (Advanced Configuration and Power Interface Daemon) reagiert auf Betätigung des Powertasters oder des Notbook-Deckels und führt voreingestellte Befehle aus.
#### wall
Alternative zu `shutdown -k`.  
Liest von `stdin` und schreibt nach `stdout` der Terminals aller angemeldeten User.
```
> echo 'Shutdown in 60 Sekunden' | wall
```
### Upstart
Zeitlich gesehen zwischen SysVinit und systemd.

Gemeinsamkeiten mit SysVinit:

* Kommandos init, telinit, runlevel, sysctl
* PID 1 = init
* /etc/rc.d

Unterschiede zu SysVinit:

* Prozesse können parallel gestartet werden
* Konfiguration und Skripte in /etc/init und /etc/event.d
* /etc/inittab fällt weg
* Softlinks unter /etc/rc{RL}.d fallen weg
* Informationen sind im Skript selbst enthalten
  * Beispiel:
  * start on (foo)
  * stop on runlevel [!2345]
* Kurze Kommandos für Upstart-Systeme:
  * start
  * stop
  * restart
  * reload
  * status
  * Beispiel: reload smbd

### systemd

* SysVinit - Upstart - systemd (Entwicklungsstrang)
* Dienste parallel starten - auch diejenigen, die voneinander abhängig sind
* Sockets für Kommunikation zw. Diensten
* grundsätzlich kompatibel zu SysVinit Skripten
* bestehen weiterhin: `runlevel, init, sysctl`
* `init 3 == systemctl isolate runlevel3.target`
* `init 1 == systemctl isolate rescue.target`
* `init 0 == systemctl isolate poweroff.target`
* `init 6 == systemctl isolate reboot.target`
* Startskripten (SysVinit) ~~ Units (systemd)
* Units und Targets sind installiert unter:
  * `/usr/lib/systemd/system`
  * `/lib/systemd/system`
* Verknüpfung (Administration) dieser Targets unter
  * `/etc/systemd/system`
* Standard-Target (default.target) einrichten:
  * `> ln -s /usr/lib/systemd/system/multi-user.target /etc/systemd/system/default.target`
* Dienst starten:
  * `systemctl start sshd.service`
* Dienste Optionen:
  * start
  * stop
  * restart
  * status
  * enable
  * disable
