# Ender-5-Plus-SKR-1.4-Turbo-TMC2209-Marlin-2.0.5.3
Firmware und Umbauanleitung zum Upgrade des Ender 5 Plus auf das Bigtreetech SKR 1.4 Turbo mit TMC2209 Stepstick Treibern, sensorless Homing für X- und Y-Achse, BLTouch, Bigtreetech TFT35 Touchscreen und einem Raspberry Pi 4 mit Octoprint.

In diesem Repository stelle ich das Upgrade meines Ender 5 Plus vor. Dazu habe ich eine angepasste Marlin Firmware in der Version 2.0.5.3 für das Bigtreetech SKR 1.4 Turbo Board erstellt. Die Grundlage für die Konfigurationsdatei wurde vom Ender 5 Pro übernommen.
Bei der Modifikation war es mir wichtig, dass keine mechanischen Teile des Druckers oder des Gehäuses verändert werden müssen.<br>
<br>
**Wichtiger Hinweis:** <br>
Jeglicher Umbau erfolgt auf eigenes Risiko. Für eventuelle Schäden übernehme ich keine Haftung.

Verwendete Quellen: <br>
https://github.com/MarlinFirmware/Marlin <br>
https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3 <br>
https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware <br>
https://github.com/bigtreetech/BIGTREETECH-TFT35-V3.0 <br>

# Übersicht
## 1. Vorstellung des Projekts
Folgende Upgrades werden hier vorgestellt:
- 500W Meanwell Netzteil
- Einbau des SKR 1.4 Turbo mit TMC2209 Stepstick Treibern und des Raspberry Pi
- Anpassung und Anschluss der Leitungen
- Installation des TFT35 Displays
- Anpassung der Marlin Firmware für das SKR 1.4 Board
- Anpassung der Bigtreetech Firmware für das TFT35 Display

## 2. 3D Modelle
Für den Einbau der Kompoenten sind folgende 3D Modelle notwendig gewesen.
### Halterung für das Meanwell 500W Netzteil
Zur Reduzierung der Lautstärke des Druckers wurde ein 500W 24V Netzteil von Meanwell verbaut. Da ich keine Veränderungen am Gehäuse vornehmen wollte, musste ich aufgrund anderer Lochabstände einen Adapter zum Einbau des Netzteils drucken. Hierfür habe ich folgendes Modell auf Thingiverse von jetpilotFX1 verwendet: https://www.thingiverse.com/thing:4212325

### Halterung für das SKR 1.4 Turbo, Raspberry Pi und Mosfet
Da die Lochabstände des SKR Boards nicht mit dem originalen Board von Creality zusammenpassen und zusätzlich noch ein Raspberry Pi 4 in die Elektronik-Box des Druckers gebaut werden sollte, habe ich nachfolgende Halterung auf Thingiverse von apalma verwendet: https://www.thingiverse.com/thing:4328907. Leider scheint sich das Format des Mosfet Treibers für das Heizbett verändert zu haben, sodass die Lochabstände der Platine nicht mehr zur Halterung passen. Dafür habe ich zwei kleine Adapter erstellt und ebenfalls auf Thingiverse hochgeladen: https://www.thingiverse.com/thing:4369583. Wichtiger Hinweis: durch die Verwendung der Halterung ist kein Zugriff mehr von außen auf den USB-Anschluss und die MicroSD Karte mehr möglich.

### Gehäuse für TFT35 Display
Das TFT35 Display von Bigtreetech hat neben dem Touchscreen noch einen Drehgeber für die "Marlin-Bedienung". Dadurch passt das Display nicht ohne mechanische Bearbeitung in den vorgesehenen Ausschnitt des Ender 5 Plus Gehäuses. Um dies zu vermeiden, habe ich auf Thingiverse ein Gehäuse von mister2212 verwenden wollen (https://www.thingiverse.com/thing:3862925). Das Gehäuse wird mit Hilfe von 4 Neodymmagneten an der Gehäusefront des Ender 5 Plus befestigt. Da ich aber leider nur Magnete im Format 10x2mm zuhause hatte, habe ich ein Remix des Gehäuses auf Thingiverse gestellt: https://www.thingiverse.com/thing:4369558.

## 3. Einbau und Anpassung der Leitungen
Von der Reihenfolge bin ich folgendermaßen vorgegangen:
1 Netzteil mit Adapter
2 Halterung für Platinen
2.1 SKR 1.4 Turbo
2.2 Raspberry Pi
2.3 Mosfet für das Druckbett
2.4 TMC2209 Treiber
3 Verkabelung anpassen und installieren

Es werden lediglich 4 Stepstick Treiber für die 5 Motoren benötigt, da sich die Z-Achsen Motoren beim SKR 1.4 Turbo Board einen Treiber teilen. Wenn die TMC2209 Treiber verbaut werden, muss ggf. ein Pin auf der Unterseite des Treibers entfernt werden. Dies hängt davon ab, ob ein Endschalter für die Achse verwendet wird, oder ob das sogenannte sensorless homing genutzt werden soll. In meinem Fall verwende ich für die Achsen X und Y das sensorless homing und für die Z-Achse den BLTouch des Ender 5 Plus. Im YouTube Video https://www.youtube.com/watch?v=oHMZ0ocTYvM von Chris Riley wird ab Minute 7 sehr schön gezeigt, welcher Pin bei der Verwendung eines Endschalters mit Hilfe eines Seitenschneiders vom TMC2209 entfernt werden muss. Wichtig: wenn ihr sensorless homing verwendet, darf der Pin des dazugehörigen Treibers auf keinen Fall entfernt werden! Außerdem müssen beim TMC2209 im UART Modus alle Jumper unter den Steppsticks, bis auf den UART-Jumper entfernt werden (siehe https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/BTT%20SKR%20V1.4%20Instruction%20Manual.pdf, Seite 7/12).

Die meisten Leitungen können direkt vom Creality Mainboard auf das SKR 1.4 umgesteckt werden. Für den BLTouch gibt es bei diesem Board 2 eigene Anschlüsse. Hierbei ist jedoch zu beachten, dass die Steckerbelegung beider BLTouch Leitungen verändert werden muss. An dem 2-poligen JST Stecker muss die schwarze und mit der weißen Ader vertauscht werden. Bei dem 3-poligen Dupont Stecker muss die braune mit der schwarzen Ader vertauscht werden. Hierzu sollte ebenfalls ein Blick in die offizielle Dokumentation von Bigtreetech geworfen werden (https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/BTT%20SKR%20V1.4%20Instruction%20Manual.pdf, Seite 8/12).

## 4. Anpassung der Firmware
Für das Kompilieren der Firmware wurde Platformio unter Microsoft Visual Studio Code verwendet.
Als Grundlage wurde die Marlin Firmware in der Version 2.0.5.3 von GitHub und die dazugehörige Konfiguratonsdatei für den Ender 5 Pro verwendet. Leider existierte noch keine fertige Konfiguration für den Ender 5 Plus.
Alle Anpassungen wurden in den folgenden Dateien vorgenommen:
- platformio.ini
- Configuration.h
- Configuration_adv.h
- Conditionals_LCD.h

In der Conditionals_LCD.h wurde nur in Zeile 586 der zweite Teil der if-Bedingung auskommentiert: <br>
#if Z_HOME_DIR < 0 && !HAS_CUSTOM_PROBE_PIN wurde geändert zu <br>
#if Z_HOME_DIR < 0 //&& !HAS_CUSTOM_PROBE_PIN <br>
Ohne diese Änderung hatte das Homing der Z-Achse nicht funktioniert.

Wenn die Firmware direkt verwendet werden soll, muss einfach nur die Datei *firmware.bin* aus dem Verzeichnis *Firmware* auf eine MicroSD Karte kopiert und in das SKR 1.4 gesteckt werden. Durch das Einschalten des Boards installiert sich die Firmware anschließend automatisch. Auf der Karte befindet sich nach erfolgreicher Installation der Firmware jetzt eine *FIRMWARE.CUR* Datei. Die Installation per Octoprint über einen Raspberry Pi wird im folgenden Abschnitt erklärt.

Nach der Übertragung der Firmware kann es zu einer EEPROM Fehlermeldung kommen. In diesem Fall einfach per Terminal (z.B. über Octoprint) die Befehle **M502**, **M500**, **M501** an das Board senden.

Die Firmware für das TFT35 wurde gemäß der Anleitung von Bigtreetech (https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware) kompiliert und mit Hilfe einer SD Karte übertragen. Bite neben der *BIGTREE_TFT35_V3.0.25.4.bin* auch den Unterordner *TFT35* aus dem Ordner *Copy to SD Card root directory to update - Unified Menu Material theme* mit auf die SD Karte kopieren.

## 5. Übertragung der Firmware per Octoprint
Da Übertragung einer neuen Firmware auf das SKR 1.4 ohne Öffnung des Gehäuses nicht mehr möglich ist, verwende ich den ebenfalls verbauten Raspberry Pi 4 mit Octoprint und WinSCP um eine neue vorkompilierte Firmware auf das Board zu übertragen.
Dazu muss auf dem Raspberry Pi *usbmount* gemäß dieser Anleitung installiert werden: https://github.com/OctoPrint/OctoPrint-FirmwareUpdater/blob/master/README.md#usbmount-installation. Die SD Karte des SKR 1.4 ist dann nach einem Neustart auf dem Raspberry Pi unter */media/usb0* gemountet. Bevor sich aber Dateien per WinSCP darauf übertagen lassen, muss Octoprint die Karte mit dem Befehl **M22** freigeben.
