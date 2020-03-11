# Die Debian Paketverwaltung

Dieses Dokument ist zugeschnitten auf die Lerninhalte des LPIC-1.

## Einleitung

Folgende Distributionen nutzen die Debian Paketverwaltung (Liste nicht vollständig):

* Debian
* Ubuntu
* Linux Mint
* MX Linux
* KNOPPIX
* DSL (Damn Small Linux)
* Kali Linux
* PureOS
* elementary OS
* KDE neon

Für den Bereich Administration _benötigte (required)_ Pakete der Debian Paketverwaltung:

* dpkg
* debconf
  
Für die Arbeit mit der Paketverwaltung _wichtige (important)_ Pakete:
* apt
* apt-utils
* debconf-i18n

Weitere optionale Pakete sind:
* aptitude
* tasksel
* synaptic
* wajig

## debian packages

Auf Debian basierende Distributionen verwenden zur Verteilung von Softwarepaketen _debian packages_.

Namenskonvention von debian packages:
```
nano_3.2-3_amd64.deb

# nano      ---     package name
# 3.2       ---     version
# 3         ---     release
# amd64     ---     architecture (arch)
# .deb      ---     suffix for debian package
```

## dpkg - debian package management system

Das Paket _dpkg_ beinhaltet folgende Programme:
```
/usr/bin/dpkg                      # Hauptprogramm
/usr/bin/dpkg-deb                  # Debian Pakete inspizieren/manipulieren
/usr/bin/dpkg-divert               # Dateien an anderer Stelle installieren
/usr/bin/dpkg-maintscript-helper   # Einschränkungen von Dpkg umgehen
/usr/bin/dpkg-query                # Paketdatenbank des Systems abfragen
/usr/bin/dpkg-split                # Debian Pakete in kleinere Teile splitten
/usr/bin/dpkg-statoverride         # Benutzerrechte von Paketkomponenten ändern
/usr/bin/dpkg-trigger              # Trigger im aktuellen dpkg prüfen/auslösen
/usr/bin/update-alternatives       # Verwaltet Links des Debian Alternativsystems
```

Dpkg ist ein hervorragend dokumentiertes, essentielles Paket von Debian.
Es lohnt sich uneingeschränkt, die Handbücher (Manuals) der aufgeführten Tools zu studieren.

### dpkg
```
# Liste alle installierten und teilinstallierten Pakete
dpkg --list
dpkg -l

# Liste alle installierten Pakete, die mit 'foo' beginnen
dpkg -l foo*

# Installiere Paket (Testlauf)
sudo dpkg --dry-run --install ./nano_3.2-3_amd64.deb

# Installiere Paket
sudo dpkg --install ./nano_3.2-3_amd64.deb
sudo dpkg -i ./nano_3.2-3_amd64.deb

# Deinstalliere Paket
sudo dpkg --remove nano
sudo dpkg -r nano

# Deinstalliere Paket und Konfigurationsdateien
sudo dpkg --remove nano
sudo dpkg -r nano

# Zeige Informationen zu einem installierten Paket
dpkg --status nano
dpkg -s nano

# Zeige Dateien, die ein Paket installiert hat
dpkg --listfiles nano
dpkg -L nano

# Zeige Pakete, die nicht vollständig installiert sind
dpkg --audit
dpkg -C
```
### dpkg-deb
```
# Zeige Informationen zu einem Paketarchiv
dpkg-deb --info ./nano_3.2-3_amd64.deb
dpkg-deb -I ./nano_3.2-3_amd64.deb

# Paketarchiv in einen Ordner entpacken
dpkg-deb --raw-extract nano_3.2-3_amd64.deb ./nano
dpkg-deb -R nano_3.2-3_amd64.deb ./nano
```
### dpkg-reconfigure
```
# Konfiguriere ein installiertes Paket neu
sudo dpkg-reconfigure nano

# Konfiguriere alle installierten Pakete neu
dpkg-reconfigure -a
```
Das Programm dpkg-reconfigure wird nicht, wie anzunehmen wäre, im Paket _dpkg_ sondern im Paket _debconf_ bereitgestellt.

## APT - Advanced Package Tool
APT wird vom Paket apt bereitgestellt und beinhaltet folgende Tools:
```
/usr/bin/apt              # Das Hauptprogram von APT
/usr/bin/apt-cache        # Debian-Pakete suchen und inspizieren
/usr/bin/apt-cdrom        # Datenträger als Paketquelle nutzbar machen
/usr/bin/apt-config       # Konfiguration von APT ermitteln
/usr/bin/apt-get          # Debian-Pakete installieren und verwalten
/usr/bin/apt-key          # Schlüsselbasierte Authentifizierung von Paketen
/usr/bin/apt-mark         # dpkg-Markierungen setzen
```

### apt
Das Programm _apt_ vereint nahezu alle Funktionalitäten von _apt-get_ und _apt-cache_.
Es wurde speziell für die befehlszeilenorientierte Arbeit am Paketverwaltungssystem entwickelt.
```
# Erneuere von allen konfigurierten Quellen die Paketinformationen
sudo apt update

# Installiere verfügbare Upgrades für alle auf dem System installierten Pakete
sudo apt upgrade

# Installiere Paket(e)
sudo apt install foo bar

# Deinstalliere Paket(e)
sudo apt remove foo bar

# Deinstalliere Paket(e) und Konfigurationsdateien
sudo apt purge foo bar

# Deinstalliere alle Pakete, die automatisch installiert wurden
sudo apt autoremove

# Suche in allen verfügbaren Paketinformationen nach einem Schlüsselbegriff
apt search networking
```
```
apt-get update             ->  apt update
apt-get upgrade            ->  apt upgrade
apt-get dist-upgrade       ->  apt full-upgrade
apt-get install package    ->  apt install package
apt-get remove package     ->  apt remove package
apt-get autoremove         ->  apt autoremove
apt-cache search string    ->  apt search string
apt-cache policy package   ->  apt list -a package
apt-cache show package     ->  apt show package
apt-cache showpkg package  ->  apt show -a package
```
### apt-get
Das Programm _apt-get_ installiert und verwaltet Debian Pakete.
```
apt-get install package
apt-get clean                  # Cache Bereinigen (nicht `apt-cache clean` !!!)
```
### apt-cache
Das Programm _apt-cache_ dient der Suche und Inspektion von Debian Paketen.
```
apt-cache stats                 # Informationen über die Paketdatenbank ausgeben
apt-cache search DHCP-Server    # Paketnamen und Beschreibungen durchsuchen
apt-cache policy package
apt-cache show package
```
### apt-mark
Das Programm _apt-mark_ kann als einheitliche Oberfläche zum Setzen verschiedener dpkg-Markierungen eines Pakets benutzt werden.
Markierungen können beispielsweise sein:
* `auto` (automatisch installiert)
* `manual` (manuell installiert)
* `hold` (halten)
* `install` (installieren)
* `deinstall` (entfernen)
* `purge` (vollständig entfernen)

### apt-key
Das Programm _apt-key_ verwaltet eine Liste von Schlüsseln, die APT benutzt, um Pakete zu authentifizieren.

### apt-config
Das Programm _apt-config_ dient als Schnittstelle zur Abfrage der Konfiguration der APT Paketverwaltung.

### apt-cdrom
Das Programm _apt-cdrom_ fügt CD-ROM-Datenträger zur Quelldatenbank der APT Paketverwaltung hinzu.

## aptitude
Der Funktionsumfang des Programms _aptitude_ ist vergleichbar mit _apt_, bietet aber zusätzlich folgendes:
* textbasierte Nutzeroberfläche
* Rückverfolgung von Programmabhängigkeiten
* Verwaltung von veralteten Paketen
* Verwaltung von lokalen Paketen
* Befehlszeilenoptionen kombinierbar

## wajig
Mit den Paketen _dpkg, debconf, apt und aptitude_
können Anwender wohl 90% aller jemals anfallenden Aufgaben im Debian-Paketmanagement erledigen.
In diese Bresche schlägt wajig.
Der Nutzer kann mit einheitlichen _wajig_ - Befehlen die jeweils richtigen Tools ansprechen,
ohne sich über die passenden Optionen oder gar das richtige Programm Gedanken machen zu müssen.

Eine kleine Erklärung zum Programmnamen wajig:
wa bedeutet im Japanischen soviel wie Harmonie oder Teamgeist und Einheit.
Jig kommt aus dem Englischen und hat mehrere Bedeutungen: 
Kleine Maschine,
Hilfswerkzeug (soetwas wie ein Schraubstock beispielweise),
ein traditioneller Tanz,
eine ausgelassene Versform und dessen Vortragsweise, ...

## tasksel
Tasks aus der Sicht von _tasksel_ sind Aufgaben,
die ein Nutzer mit dem Rechner erledigen möchte.
Dazu werden Pakete, die aus Sicht der Debian-Entickler prädestiniert sind für diesen Job,
zusammengefasst und mit Task-Namen und Beschreibung versehen.
Die Pakete eines Tasks können zu einem laufenden System hinzugefügt oder komplett deinstalliert werden.

## synaptic
Synaptic ist ein grafisch basiertes Verwaltungswerkzeug, dass auf apt aufbaut.
In der Grundansicht werden die Paketgruppen von Debian dargestellt und können wie in einem Datei-Explorer durchstöbert werden.
