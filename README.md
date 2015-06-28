# Entfernung messen mit RaspberryPi


## Einleitung:

Diese Anleitung beschreibt, wie man mit wenigen Bauteilen und einem RaspberryPi einen Ultraschall-basierenden Entfernungsmesser baut.

### Bauteil-Liste:

* 1x RaspberryPi B, B+ oder 2 (inkl. Netzteil, SD bzw. MicroSD Karte, Tastatur) - ca. 60 Eur 
* 1x Steckplatine (Breadboard small+ oder full+) - ca. 8 Eur
* 1x Steckbrücken-Set für die Steckplatine - ca. 5 Eur
* 1x Set mit Male-Female Steckbrücken (Jumper Wires)¹ - ca. 5 Eur
* 1x Ultraschall Messmodul HC-SR04 - ca. 5 Eur
* 1x Widerstand mit 330Ω - ca. 2 Eur
* 1x Widerstand mit 470Ω - ca. 2 Eur

¹ Alternativ zu den Male-Female Steckbrücken kann auch ein Breadboard-Adapter für die Verbindung zum RaspberryPi verwendet werden - ca. 8 Eur

**Gesamtkosten:** Die Kosten für alle benötigten Bauteile betragen ca. 80 bis 90 Eur. 

Bei den angegebenen Preise handelt es sich um Durchschnittspreise für Deutschland zum Zeitpunkt der Erstellung des Tutorials. Die benötigten Teile sollten sich bei den meisten Elektronik-Händler, die auch ein RaspberryPi im Sortiment führen, zu finden sein.


### Funktionsweise:

Ziel des Aufbaus ist es, die Entfernung zu einem Gegenstand mittels Ultraschallpuls zu ermitteln. Verwendet wird hierfür das Ultraschall-Messmodul [HC-SR04](http://www.mikrocontroller.net/attachment/218122/HC-SR04_ultraschallmodul_beschreibung_3.pdf). Das Modul ist dabei in der Lage eine Entfernungsmessung im Bereich 2cm bis ~3 Meter durchzuführen mit einer Genauigkeit von ±3mm. 

Da Ultraschall durch Gegenstände reflektiert wird, kann mittels der Zeit zwischen absenden und empfangen des Schall die Entfernung berechnet werden. Bei einer Lufttemperatur von 20°C breitet sich Ultraschall 343,5 m/s aus, sodass mittels folgender Formel die Entfernung berechnet werden kann:

> Distanz [cm] = (Signallaufzeit [Sek] / 2) * 34350 [cm/sek]

## Aufbau:

![](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/raw/master/Schaltplan/Schaltplan.png =250x) 

still to come


## Script

still to come


___
**Lizenz-Information:**

Copyright Jens Dutzi 2015 / Stand: 28.06.2015

![Lizenz](http://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png).

Dieses Werk ist lizenziert unter einer [Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 4.0 International Lizenz.](http://creativecommons.org/licenses/by-nc-sa/4.0/)