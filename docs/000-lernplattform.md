# Lernplattform für den praktischen Teil
In der Lernphase zu den Prüfungen des LPIC-1 ist anzuraten, die Theorie auch in Praxis umzusetzen.
So bekommt der Aspirant eine Vorstellung davon, was terminalbasiertes Arbeiten an einem Linux-System bedeutet.
Dieses Dokument beschreibt die dafür notwendigen Werkzeuge und auch die grundlegende Einrichtung der selbigen.
Auch wenn das LPIC-1 eine gewisse Vorerfahrung voraussetzt,
kann sich so der einzelne doch die individuelle Mehrarbeit ersparen
und so früh als möglich mit dem Lernen auf LPIC-1 beginnen.

## Werkzeuge
Die essenziellen Werkzeuge für den Praxisteil sind:

* Rechner mit Internetzugang
* Linux-Distribution passend zum jeweiligen Thema
* Bildschirm
* Tastatur

## Hardware vs. Virtual
Der Idealfall wäre wahrscheinlich, wenn für jede zu nutzende Distribution ein eigener Rechner samt Bilschirm und Tastatur vorhanden wäre. Der Schüler könnte sich je nach Bedarf an einen anderen Rechner setzen und dort distributionsabhängige Dinge tun.

Natürlich können auch mehrere Distributionen auf einem Rechner koexistieren. Heutige Bootmanager sind dafür geradezu prädestiniert. Der Nachteil liegt darin, dass die intensives Arbeiten am Bootsystem zu den grundlegenden Fähigkeiten eines LPIC-1 Administrators zählt und damit ein produktives Multibootsystem für praktische Experimente am Selbigen eher ungeeignet wäre.

Betrachten wir die dritte Möglichkeit, die Virtualisierung des Betriebssystems.

Die wichtigsten zwei aktuell angewandten Techniken sind die Virtualisierung mit Hypervisoren und mit Virtualisungskontainern (bspw. Docker). Beide bieten vorinstallierte Images von aktuellen Distributionen - das dürfte nicht das Entscheidungskriterium sein. Viel wichtiger ist, wie oben schon angedeutet, das der Lernende immer wieder schreibend auf das Bootsystem zugreifen muss und damit für den praktischen Teil eigentlich nur die __Virtualisierung durch einen Hypervisor angeraten__ werden kann.

## Auswirkungen von Hypervisoring auf die Hardwareplattform

Durch die Nutzung eines Hypervisors auf der Hardware der Lernplattform ergeben sich geringfügig veränderte Vorgaben was die Leistungsfähigkeiten betrifft. Das trifft vorrangig für die benötigten Desktop-Betriebssysteme zu, die für den Inhaltsteil "Desktopumgebungen" genutzt werden sollen. Die wichtigsten Parameter seien hier angeführt:

* Mehrkernprozessor, der mindestens einen Kern an den Gast abtreten kann.
* Mindestens 1GB RAM (besser 2GB) die dem Gast zur Verfügung gestellt werden können (das bedeutet 4GB RAM insgesamt auf dem Rechner, besser 8GB)

## Distributionsauswahl
Der LPIC-1 Schüler sollte sich im Idealfall auf allen Distributionen der Welt zu Hause fühlen. Setze ihn an einen Debian-Rechner oder an einen SuSE-Rechner - er wird immer das richtige tun ;) Doch welche Distributionen soll er installieren? - es gibt doch tausende! Außerdem ist es wohl eher kontraproduktiv, mehrere Distributionen eines Haupstranges zu installieren, wenn das für die praktische Arbeit keinen differntiellen Nutzen mit sich bringt. Eine grundlegende Frage muss für die richtige Auswahl vor der Installation der Lern-Distributionen beantwortet werden: Welche (minimale) Auswahl deckt alle Themen des LPIC-1 ab?

### Themenfelder des LPIC-1 und entsprechende Distributionen
Die folgenden Tabellen sind soweit ausgedünnt, dass in der Spalte _Distribution_ nur noch jene aufgeführt sind, die die maximale Anzahl an Themenfeldern abdecken. Distributionsnamen ohne Versionsnummer bezeichnen die jeweils aktuelle Version.

#### Paketverwaltungstools
| Paketverwaltungstool | Distribution |
|:--|:--|
| dpkg     | Ubuntu |
| apt      | Ubuntu |
| aptitude | Ubuntu |
| dselect  | Ubuntu |
| rpm      | CentOS |
| yum      | CentOS |
| zypper   | openSuSE |
| dnf      | Fedora |

#### Init-Systeme
| Init-System | Distribution |
|:--|:--|
| SysVinit | Devuan |
| upstart  | Ubuntu 12.04.5 |
| systemd  | Praktisch alle aktuellen Distributionen |

#### Bootmananger
| Bootmanager | Distribution |
|:--|:--|
| GRUB 0.97 | Fedora 15 |
| GRUB 2 | Praktisch alle aktuellen Distributionen |

#### Komponenten grafikbasierter Desktops
| Komponente |  Distribution |
|:--|:--|
| Xorg/X11 | Praktisch alle aktuellen Distributionen |
| Wayland  | seit Debian 10, Fedora 25 |
| KWin/KDM/KDE | Kubuntu |
| Metacity/GDM/Gnome | Ubuntu |
| XFWM/XFCE | Xubuntu |

#### Kernel-Module verwalten
| Paket | Distribution |
|:--|:--|
| module-init-tools | Bis einschließlich Ubuntu 10.04.4 |
| kmod              | Praktisch alle aktuellen Distributionen |

* https://github.com/vadmium/module-init-tools
* https://github.com/lucasdemarchi/kmod

### Zusammenfassung der Lern-Distributionen
Nach Verknüpfung aller relevanten LPIC-1-Prüfungsthemen mit den Standardwerkzeugen der verfügbaren Distributionen _genügen_ folgende Distributionen für die praktische Arbeit:

| Distribution | Themenfelder | Geschmack | Download-Link |
|:--|:--|:--|:--|
| CentOS | rpm, yum | CLI | https://wiki.centos.org/Download |
| openSuSE | zypper | CLI | https://www.opensuse.org/ |
| Fedora | dnf, Wayland/Gnome | GUI | https://getfedora.org/de/workstation/download/ |
| Devuan | SysVinit | CLI | https://devuan.org/get-devuan |
| Ubuntu | dpkg, apt, aptitude, dselect, Xorg/Gnome | GUI | https://ubuntu.com/download |
| Kubuntu | KDE | GUI | https://kubuntu.org/getkubuntu/ |
| Xubuntu | XFCE | GUI | https://xubuntu.org/download |
| Ubuntu 12.04.5 | upstart | CLI | http://releases.ubuntu.com/12.04/ |
| Ubuntu 10.04.4 | module-init-tools | CLI | http://old-releases.ubuntu.com/releases/10.04.0/ |
| Fedora 15 | legacy GRUB | CLI | https://archives.fedoraproject.org/pub/archive/fedora/linux/releases/15/Fedora/x86_64/iso/ |

Anmerkung: ein echtes Debian fehlt in dieser Auflistung, weil die Paarung _Wayland/Gnome_ bereits durch Fedora abgedeckt ist und ein echtes Xorg/Gnome _noch_ durch ein aktuelles Ubunutu bereitgestellt wird. Sobald Wayland in aktuelle Ubuntus einfließt, wird diese Tabelle aktualisiert.
