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

Bei der Formel stellt sich vielleicht die Frage, warum eigentlich die Signallaufzeit halbiert wird. Wenn wir uns das Script genau anschauen, wird die Laufzeit des Signals direkt beim absenden des Ultraschall-Pulses angefangen zu messen und hört auf, sobald das Echo-Signal wieder am Sensor ankommt. Dies hat zwangsweise die Folge, dass ich die Zeit für den Hin- und Rückweg habe. Für die Berechnung des Abstandes brauche ich allerdings nur die Zeit, die für einen der beide Wege benötigt wird - also die hälfte des Weges. Um dies zu erreichen wird die Laufzeit einfach halbiert.

## Aufbau der Hardware:

### HC-SR04 Ultraschall-Sensor:

![Ultraschall Sensor](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/raw/master/Bilder/HC-SR04.jpg) 

#### Anschlüsse:
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


## Aufbau der Software

Für den RaspberryPi wird empfiehlt sich Raspbian als System zu verwenden, welches über z.B. über [NOOB](https://www.raspberrypi.org/downloads/) installiert werden kann. Mittels eines Python-Scripts  kann die Messung durchgeführt werden.

#### Python-Script "getDistance.py"

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Entfernungsmesser für RaspberryPi
# Version 1.0
#
# Copyright: tfnApps.de Jens Dutzi
# Datum: 27.06.2015
# Lizenz: MIT Lizenz (siehe LICENSE)
# -----------------------------------


# import required modules
import time
import RPi.GPIO as GPIO

# define GPIO pins
GPIOTrigger = 18
GPIOEcho    = 24

# Funktion zum messen der Entfernung
def MesseDistanz():
	# Trigger auf "high" setzen (Signal senden)
	GPIO.output(GPIOTrigger, True)

	# Signal für 10µs senden (ggf. je nach RaspberryPi auf 0.0005 setzen)
	time.sleep(0.00001)
	
	# Trigger auf "low setzen (Signal beenden)
	GPIO.output(GPIOTrigger, False)

	# Aktuelle Zeit setzen
	StartZeit = time.time()
	StopZeit = StartZeit

	# Warte bis "Echo" auf "low" gesetzt wird und setze danach Start-Zeit erneut
	while GPIO.input(GPIOEcho) == 0:
		StartZeit = time.time()

	# Warte bis "Echo" auf "high" wechselt (Signal wird empfangen) und setze End-Zeit
	while GPIO.input(GPIOEcho) == 1:
		StopZeit = time.time()

	# Abstand anhand der Signal-Laufzeit berechnen
	# Schallgeschwindigkeit: 343,50 m/s (bei 20°C Lufttemperatur)
	# Formel: /Signallaufzeit in Sekunden * Schallgeschwindigket in cm/s) / 2 (wg. Hin- und Rückweg des Signals)
	SignalLaufzeit = StopZeit - StartZeit
	Distanz = (SignalLaufzeit/2) * 34350

	return [Distanz, (SignalLaufzeit*1000/2)]

# main function
def main():
	try:
		while True:
			Ergebnis = MesseDistanz()
			print("Gemessene Entfernung: %.1f cm (Signallaufzeit: %.4fms)" % (Ergebnis[0], Ergebnis[1]))
			time.sleep(1)

	# reset GPIO settings if user pressed Ctrl+C
	except KeyboardInterrupt:
		print("Messung abgebrochen")
		GPIO.cleanup()

if __name__ == '__main__':
	# benutze GPIO Pin Nummerierung-Standard (Broadcom SOC channel)
	GPIO.setmode(GPIO.BCM)

	# Initialisiere GPIO Ports
	GPIO.setup(GPIOTrigger, GPIO.OUT)
	GPIO.setup(GPIOEcho, GPIO.IN)

	# Setze GPIO Trigger auf false
	GPIO.output(GPIOTrigger, False)

	# Main-Funktion starten
	main()
```

Download: [getDistance.py](https://raw.githubusercontent.com/Blog404DE/RasPiUltraschallEntfernungsmesser/master/Code/getDistance.py)

### Wie führe ich eine Messung aus?

Nachdem Sie das Script auf Ihren RaspberryPi geladen haben, können Sie es direkt mittels folgendem Befehl aufrufen. Bitte beachten Sie, dass der Zugriff auf die GPIO Ports nur als *root* möglich ist:

> root@raspberrypi:~# python getDistance.py

Alternativ können Sie getDistance.py auch direkt ausführbar machen und sich das "python" beim aufrufen sparen. Hierzu ist nur folgendes notwendig:

> root@raspberrypi:~# chmod +x getDistance.py

Die Ausgabe des Python Scripts sieht wie folgt aus:

```
root@raspberrypi:~# ./entfernung.py
Gemessene Entfernung: 22.5 cm (Signallaufzeit: 0.6559ms)
Gemessene Entfernung: 22.9 cm (Signallaufzeit: 0.6690ms)
Gemessene Entfernung: 22.9 cm (Signallaufzeit: 0.6690ms)
Gemessene Entfernung: 22.5 cm (Signallaufzeit: 0.6560ms)
Gemessene Entfernung: 22.9 cm (Signallaufzeit: 0.6670ms)
```

Das Programm ermittelt kontinuierlich den Abstand und kann mittels *STRG+C* beendet werden. 



___
**Lizenz-Information:**

Copyright Jens Dutzi 2015 / Stand: 28.06.2015

Dieses Werk ist lizenziert unter der [MIT Lizenz](https://github.com/Blog404DE/RasPiUltraschallEntfernungsmesser/blob/master/LICENSE).