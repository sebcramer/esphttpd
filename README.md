esphttpd
========
esp8266 httpd server with some extras

Ich habe die esphttpd software [3](http://git.spritesserver.nl/esphttpd.git/) als Grundlage für mein Projekt benutzt. Nach und nach werden eigene Anpassungen und hoffentlich verbesserungen dazu kommen.



SDK Erstellen
=============

Die Anleitung habe ich mit der Ubuntu Version 14.04 getestet.

Benötigte Pakete
----------------
Bevor wir das SDK herunter laden und installieren sollten die Pakete aus Link [2](https://github.com/esp8266/esp8266-wiki/wiki/Toolchain) installiert werden. Vielleicht fehlt das ein oder andere Paket noch, dass aber durch die Abhängigkeiten in der Regel direkt mit installiert werden sollte.

SDK
---
Zum erstellen des Codes für den esp8266 benötigen wir ein SDK. Ich nutze das esp-open-sdk [1](https://github.com/pfalcon/esp-open-sdk), weil es dort ein Makefile zum erstellen gibt, das einem die Einzelschritte aus dem esp8266 wiki [2](https://github.com/esp8266/esp8266-wiki/wiki/Toolchain) teilweise abnimmt.<br>
Daher das SDK [1](https://github.com/pfalcon/esp-open-sdk) einfach herunterladen (z.B. nach /usr/local/). Anschließend wird in das Verzeichnis gewechselt, alles aktualisiert und compiliert mit:
<pre>make</pre>


Esphttpd Erstellen
==================

Nach dem clonen sollte im repository noch folgendes ausgeführt werden:
<pre>
git submodule init
git submodule update
</pre>
<br>Damit wird sichergestellt, dass alle submodule auch geladen werden.

Makefile
--------

In der Makefile wird die Variable "ESPRESSIF_ROOT" (bei mir „/usr/local“) verwendet die man in der Datei "Makefile.uservars" anlegen muss. Dort ist der Pfad anzugeben in dem das esp-open-sdk liegt.
Z.B. mit dem Befehl:
<br>
<pre>echo "ESPRESSIF_ROOT = /usr/local" > Makefile.uservars</pre>
Oder einem Editor eurer wahl.
<br>
Anschließend kann das Programm mit
<pre>make</pre>
compiliert werden.<br>

flashen
-------
Zum flashen benutze ich ein FTDI Chip der zwei Vorteile gegenüber anderen USB zu UART Adapter besitzt. Der erste ist, dass die UART Seite des FTDI auch mir 3,3V laufen kann und somit kein Pegelwandler benötigt wird.<br>
Der andere Vorteil ist, dass neben den Datenleitungen noch andere zur Verfügung stehen die genutzt werden können um GPIO0 und CH_PD zu beschalten. Dies wird vom exptool.py direkt unterstützt und somit muss man beim flashen nicht immer Jumper setzen um den Chip in den Boot Modus [4](https://github.com/esp8266/esp8266-wiki/wiki/Uploading) zu versetzen.
<br>
Dazu muss der esp8266 wie unten stehend mit dem FTDI verbunden werden:<br>

<pre>
-----------
|         |
|         |
|         |
|         |
| 1 3 5 7 |
| 2 4 6 8 |
-----------
Ansicht von oben

1: GND
2: TXD
3: GPIO2
4: CH_PD (muss im Betrieb auf Vcc liegen)
5: GPIO0
6: RST
7: RXD
8: VCC

Booloader Mode:
CH_PD - RTS
GPIO0 - DTR
</pre>
<br>
Wenn das follendet ist, kann der code wie folgt übertragen werden:
- make flash : überträgt den code
- make htmlflash : überträgt die Dateien für den Webserver. Dies ist aber nur nötig, wenn man in den html Files etwas ändert.


LINKS
=====
- [1] https://github.com/pfalcon/esp-open-sdk
- [2] https://github.com/esp8266/esp8266-wiki/wiki/Toolchain
- [3] http://git.spritesserver.nl/esphttpd.git/
- [4] https://github.com/esp8266/esp8266-wiki/wiki/Uploading


LICENSE
=======

Der größte Teil der Software kommt von:
- http://www.esp8266.com/viewtopic.php?f=6&t=376
- http://git.spritesserver.nl/esphttpd.git/

Wenn nicht explizit anders angegeben gillt:<br>
Dieses Material steht unter der Creative-Commons-Lizenz Namensnennung 4.0 International. Um eine Kopie dieser Lizenz zu sehen, besuchen Sie http://creativecommons.org/licenses/by/4.0/.
<br>cc-by Pascal Gollor (http://www.pgollor.de)
