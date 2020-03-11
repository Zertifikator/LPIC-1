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

### Themenfeld des LPIC-1 und entsprechende Distributionen

#### Paketverwaltung

| Paketverwaltungstools | Distribution |
|--|--|
| rpm | openSuSE |
| zypper | openSuSE |
| dpkg | Debian |
| apt | Debian |
| aptitude | Debian |
| pacman | Arch |

#### Init-System

| Init-System | Distribution |
|--|--|
| init    | Devuan |
| upstart | |
| systemd | Praktisch alle aktuellen Distributionen |

#### Bootmananger

| Bootmanager | Distribution |
|--|--|
| GRUB 0.97 | Ubuntu 18.04.3 Mini mit GRUB2 Downgrade |
| GRUB 2 | Praktisch alle aktuellen Distributionen |

#### Desktop-Umgebung
