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
