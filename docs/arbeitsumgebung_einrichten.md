# Arbeitsumgebung für den praktischen Teil
In der Lernphase zu den Prüfungen des LPIC-1 ist anzuraten, die Theorie auch in Praxis umzusetzen.
So bekommt der Aspirant eine Vorstellung davon, was terminalbasiertes Arbeiten an einem Linux-System bedeutet.

## Werkzeuge
Die eszenziellen Werkzeuge für den Praxisteil sind:

* Rechner mit Internetzugang
* Linux-Distribution passend zum jeweiligen Thema
* Bildschirm
* Tastatur

## Hardware vs. Virtual
Der Idealfall wäre wahrscheinlich, wenn für jede zu nutzende Distribution ein eigener Rechner samt Bilschirm und Tastatur vorhanden wäre. Der Schüler könnte sich je nach Bedarf an einen anderen Rechner setzen und dort distributionsabhängige Dinge tun.

Natürlich können auch mehrere Distributionen auf einem Rechner koexistieren. Heutige Bootmanager sind dafür geradezu prädestiniert. Der Nachteil liegt darin, dass die intensive Arbeit am Bootsystem zu den grundlegen Fähigkeiten eines LPIC-1 Administrators zählt und damit ein lebendes Multibootsystem für praktische Experimente am Selbigen eher ungeeignet wäre.

Betrachten wir die dritte Möglichkeit, die Virtualisierung des Betriebssystems.

Die wichtigsten zwei aktuell angewandten Techniken sind die Virtualisierung mit Hypervisoren und mit Virtualisungskontainern (bspw. Docker). Beide bieten vorinstallierte Images von aktuellen Distributionen - das dürfte nicht das Entscheidungskriterium sein. Viel wichtiger ist, wie oben schon angedeutet, das der Lernende immer wieder schreibend auf das Bootsystem zugreifen muss und damit für den praktischen Teil eigentlich nur die Virtualisierung durch einen Hypervisor angeraten werden kann.

## Distributionsauswahl
Der LPIC-1 Schüler sollte sich im Idealfall auf allen Distributionen zu Hause fühlen. Setze ihn an einen Debian-Rechner oder an einen SuSE-Rechner - er wird immer das richtige tun ;) Doch welche Distributionen soll er installieren? - es gibt doch tausende! Außerdem ist es wohl eher kontraproduktiv, mehrere Distributionen eines Haupstranges zu installieren, wenn das für die praktische Arbeit keinen praktischen Nutzen mit sich bringt. Eine grundlegende Frage muss für die richtige Auswahl vor der Installation aller Distributionen beantwortet werden: Welche (minimale) Auswahl deckt alle Themen des LPIC-1 ab?

### Themenfelder des LPIC-1 und entsprechende Distributionen
Die folgenden Tabellen sind soweit ausgedünnt, dass in der Spalte _Distribution_ nur noch jene aufgeführt sind, die die maximale Anzahl an Themenfeldern abdecken. Distributionsnamen ohne Versionsnummer bezeichnen die jeweils aktuelle Version.

#### Paketverwaltungstools
| Paketverwaltungstool | Distribution |
|:--|:--|
| dpkg     | Ubuntu |
| apt      | Ubuntu |
| aptitude | Ubuntu |
| dselect  | Ubuntu |
| rpm      | RHEL |
| yum      | RHEL |
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

### Zusammenfassung
Nach logischer Verknüpfung aller relevanten Prüfungsthemen über die verfügbaren Distributionen _genügen_ folgende Distributionen für die praktische Arbeit:

| Distribution | Themenfelder | Umfang |
|:--|:--|:--|
| aktuelles RHEL | rpm, yum | Minimal |
| aktuelles openSuSE | zypper | Minimal |
| aktuelles Fedora | dnf, Wayland | Desktop |
| aktuelles Devuan | SysVinit | Minimal |
| aktuelles Ubuntu | dpkg, apt, aptitude, dselect, Gnome | Desktop |
| aktuelles Kubuntu | KDE | Desktop |
| aktuelles Xubuntu | XFCE | Desktop |
| Ubuntu 12.04.5 | upstart | Minimal |
| Ubuntu 10.04.4 | module-init-tools | Minimal |
| Fedora 15 | legacy GRUB | Minimal |
