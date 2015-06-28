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

Ziel des Aufbaus ist es, die Entfernung zu einem Gegenstand mittels Ultraschall-Impuls zu ermitteln. Verwendet wird hierfür das Ultraschall-Messmodul [HC-SR04](http://www.mikrocontroller.net/attachment/218122/HC-SR04_ultraschallmodul_beschreibung_3.pdf). Das Modul ist dabei in der Lage eine Entfernungsmessung im Bereich 2cm bis ~3 Meter durchzuführen mit einer Genauigkeit von ±3mm. 

Da Ultraschall durch Gegenstände reflektiert wird, kann mittels der Zeit zwischen absenden und empfangen des Schall die Entfernung berechnet werden. Bei einer Lufttemperatur von 20°C breitet sich Ultraschall 343,5 m/s aus, sodass mittels folgender Formel die Entfernung berechnet werden kann:

> Distanz [cm] = (Signallaufzeit [Sek] / 2) * 34350 [cm/sek]

## Aufbau der Hardware:

### HC-SR04 Ultraschall-Sensor:

![Ultraschall Sensor](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/raw/master/Bilder/HC-SR04.jpg) 

###### Anschlüsse:
1. Vcc (+5V)
2. Trigger
3. Echo
4. Gnd

Auf Vcc liegt die +5V Versorgungsspannung und bei Gnd die Masse an. Über "Trigger" wird der Ultraschall-Impuls ausgelöst und über Echo erfolgt die Signalisierung des erfolgreichen Empfang des Ultraschall-Echos. 

### Aufbau der Schaltung:

Die 4 PINs des Ultraschall-Moduls müssen wie folgt mit dem RaspberryPi verbunden werden:

- Vcc an Pin 2 (Vcc +5V)
- GND an Pin 6 (GND) 
- TRIG an Pin 12 (GPIO 18)
- An ECHO wird der 330Ω Widerstand angeschlossen. Vom Ende des Widerstands geht eine Verbindung zu Pin 18 (GPIO 24) und über den 470Ω eine Verbindung zu Pin 6 (GND).

##### Bitte unbedingt beachten:
Das HC-SR04 Modul darf auf keinen Fall direkt mit dem RaspberryPi verbunden werden. Das Ultraschall-Modul arbeitet mit einer Spannung von 5v, während die GPIO Ports des RaspberryPi nur 3.3V vertragen. Ohne einen Spannungsteiler am ECHO-Ausgang wird beim RaspberryPi daher der GPIO Eingang zerstört. Aus diesem Grund ist unbedingt der Widerstand R1 mit 330Ω notwendig. 

### Schaltbild auf dem Steckboard (Breadboard):

![Breadboard](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/raw/master/Schaltplan/Schaltplan.png) 

Downloads-URLs für den Schaltplan in verschiedene Formate: 

- [PDF Datei](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/raw/master/Schaltplan/Schaltplan.pdf)
- [Fritzing](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/raw/master/Schaltplan/Schaltplan.fzz)


## Script

still to come


___
**Lizenz-Information:**

Copyright Jens Dutzi 2015 / Stand: 28.06.2015

![Lizenz](http://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png).

Dieses Werk ist lizenziert unter einer [Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 4.0 International Lizenz.](http://creativecommons.org/licenses/by-nc-sa/4.0/)