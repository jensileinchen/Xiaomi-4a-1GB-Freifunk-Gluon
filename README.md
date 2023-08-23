Xiaomi Mi Router 4A Gigabit Edition
Benötigt werden: PC mit aktuellen Windows-Betriebssystem, Internetverbindung, Xiaomi Router und 2 Netzwerkkabel (einmal Internet-Verbindung für den Xiaomi-Router und LAN Verbindung zum Laptop, mit dem der Router geflasht werden soll. 
Diese Anleitung ist für Microsoft Windows-Betriebssysteme!

Vorbereitung – wir starten mit einigen Downloads, die wir später benötigen:
•	Aktuelles Freifunk-Image der jeweiligen Community herunterladen (*.bin Datei) 
•	OpenWRT Image für den Xiaomi-Router herunterladen von der Webseite, am besten nach dem Download umbenennen zu openwrt.bin 
Link: https://openwrt.org/inbox/toh/xiaomi/xiaomi_mi_router_4a_gigabit_edition
•	Programm „VirtualBox“ herunterladen und installieren https://www.virtualbox.org/ alternativ auch „VMWare Workstation“ herunterladen.
•	Linux Ubuntu herunterladen als ISO Image: https://ubuntu.com/desktop
•	FTP-Client Programm z.B. „Cyberduck“ https://cyberduck.io/download/  
Alternativ auch „Filezilla“ https://filezilla-project.org/download.php?type=client

Inbetriebnahme Xiaomi-Router
•	Router mit Netzwerkkabel zum Heimnetzwerk (Fritzbox o.ä.) verbinden, hierzu die blaue RJ45-Buchse auf der Rückseite verwenden. Beschriftung: „Internet“
•	PC mit LAN-Kabel an eine der beiden freien RJ45-Buchsen verbinden.
•	Router mit Spannungsversorgung einschalten (mitgeliefertes Netzteil) 
•	Die LAN-Konfiguration vom Windows-PC wird auf DHCP ( = automatisch ) eingestellt
•	Per Browser wird die Startseite vom Router aufgerufen: 192.168.31.1
•	Auf der Seite wird Germany als Aufstellungsland ausgewählt und die allg. Geschäftsbedingungen müssen akzeptiert werden (Checkbox)
•	Ein sicheres Geräte- und WLAN Passwort wird vergeben, beispielsweise XiaomiMI
•	Die Stok-Nummer in der URL vom Router wird in eine Text-Datei geschrieben oder einfach das Browser-Fenster offen gelassen (Diese Nummer benötigen wir ggf. später noch) Browser Fenster kann erstmal in die Taskleiste minimiert werden.

(wenn die Stok nicht in der URL angezeigt wird einfach die Router Startseite per Eingabe der IP 192.168.31.1 nochmals mit einem anderen Browser zb Firefox öffnen)


Nun kann mit der Installation der Software begonnen werden:
Im ersten Schritt wird Virtualbox installiert um darin später die Linux-VM zu installieren. Dann kann eine neue virtuelle Maschine in der Virtualbox angelegt werden und vom heruntergeladenen ISO-Image installiert werden.
Die Installation vom Ubuntu ist selbsterklärend. Nach dem ersten Start vom Ubuntu-Betriebssystem wird ein Benutzerkonto angelegt und ein Passwort hinterlegt. Dieses benötigen wir später mehrmals.
Nach dem Neustart des Betriebssystems (in der virtuellen Maschine) melden wir uns am Linux an und können erstmal über die Konsole alle notwendigen Aktualisierungen vom Betriebssystem durchführen. (unten links auf die 9 Quadrate klicken „Anwendungen anzeigen“ und das Terminal starten. (ggf. im Suchfeld Ter eingeben und Terminal auswählen)
Eine schöne Alternative zur Virtualbox ist auch, im Windows 10 oder Windows 11 enthaltene WSL „Windows-Subsystem for Linux“ zu aktivieren. Dazu werden zuerst über die Windows-Update Funktion alle Windows-Komponenten auf den aktuellen Stand gebracht, eventuell muss der PC mehrfach neu gestartet werden.

Schritt 1: WSL installieren
In der Windows-Konsole (Als Administrator aufrufen im Menü mit CMD oder Eingabeaufforderung) 
wsl --install
nach erfolgreicher WSL Installation sollte der PC neu gestartet werden. Anschließend startet man erneut die Windows-Konsole (wieder mit Administrator-Rechten), um Ubuntu zu installieren:

Schritt 2: Ubuntu in das WSL installieren
wsl –list –online
jetzt erscheint eine Liste aller Linux-Distributionen, wir installieren die neueste Ausgabe von Ubuntu
wsl –install -d Ubuntu-20.04
der Prozess läuft automatisch ab. Anschließend muss noch ein Benutzerkonto angelegt werden, ebenso ein Passwort hinterlegt werden. Dieses kann aber unabhängig vom aktuellen Windows-Benutzer erstellt werden. Zu guter Letzt dieser Installation wird das Linux-System noch auf aktuellen Stand gebracht und die Unterstützung für Python installiert:
sudo apt-get update
(es muss einmal das Administrator Passwort eingegeben werden, damit wir den root Status als Benutzer erreichen)
sudo apt install update
sudo apt dist-upgrade
je nach Umfang und Alter der Ubuntu-Installation kann das ein paar Minuten dauern; nach den Updates müssen wir uns noch die Unterstützung für Python-Programmiersprache sowie Telnet-Verbindungen installieren, dazu nutzen wir folgenden Befehl:
sudo apt install git pip python3
sudo apt update && sudo apt upgrade
sudo apt install python3-pip git telnet 

Die Vorbereitungen sind abgeschlossen – nun geht’s an den eigentlichen Vorgang:
Nacheinander führen wir im Terminal-Fenster folgende Befehle aus:

(Hinweis: Im Gegensatz zu der Windows-Konsole ist die Linux-Konsole case-sensitive, also auf die entsprechende Groß- oder Kleinschreibung muss zwingend geachtet werden !)

Um später den Router zu übernehmen und Vollzugriff zu gelangen holen wir uns jetzt das entsprechende Github-Repository mit diesem Befehl:
 	git clone https://github.com/acecilia/OpenWRTInvasion.git
nachdem das Repo heruntergeladen wurde wechseln wir in den Ordner \OpenWRTInvasion mit dem Konsolenbefehl 
cd OpenWRTInvasion
(Hier noch ein Hinweis für nicht so erfahrene Konsolen-Benutzer: Wenn ihr „cd Leerzeichen O“ geschrieben habt drückt einfach die TAB Taste um die Autovervollständigung zu nutzen -- drückt dann ENTER um in den ausgewählten Ordner zu gelangen.)
Jetzt müssen wir noch
pip3 install -r requirements.txt
ausführen um anschließend mit
python3 remote_command_execution_vulnerability.py
das Angriffs-Script in Python-Programmiersprache ausführen zu können. Das Script fragt nach der Ziel-IP vom Router, die wir mit 192.168.31.1 eintragen, bei der dann folgenden Passwort-Abfrage geben wir das vorhin bei der Ersteinrichtung hinterlegte Passwort „XiaomiMI“ ein und bestätigen mit ENTER-Taste. Bei erfolgreicher Authentifizierung am Gerät kommt die Abfrage der Methode, wo wir „2“ eintragen um die Online-Methode auszuführen. Der Angriff auf den Router sollte nun vollständig durchlaufen und der Exploit ist damit erfolgreich abgeschlossen.
Im nächsten Schritt müssen wir die OpenWRT auf den Router kopieren, um diese dann dort installieren zu können. Wir wechseln im Windows-Betriebssystem in unseren lokalen Download-Ordner und installieren von dort das bereits heruntergeladene FTP Programm „Cyberduck“. Das Programm wird gestartet und eine FTP Verbindung zum Router eingerichtet. (Wie am Anfang bereits gesagt kann man auch Filezilla als FTP-Programm verwenden)Das erste Icon in der Menü-Leiste „neue Verbindung“ anklicken und dann als Server-Adresse 192.168.31.1 eintragen. Port ist standardmäßig 21 und die Authentifizierung erfolgt mit dem Benutzer „root“ und dem Passwort „root“ – nun kann auf „verbinden“ geklickt werden und es sollte sich nach kurzer Zeit der Hauptordner auf dem Router öffnen. Hier wechseln wir in den Ordner /tmp  um jetzt das OpenWRT-Image dort per d&d hereinzuziehen. (dazu einfach mit dem normalen Windows-Explorer in den Downloads-Ordner wechseln, die umbenannte Datei openwrt.bin auswählen und per drag&drop in das FTP Programm in den Ordner /tmp ziehen. Der Übertragungsstatus wird angezeigt und der erfolgreiche Upload der Datei auf den Router bestätigt. (Hinweis: natürlich kann man auch jedes andere FTP-Programm nutzen, Filezilla zum Beispiel wäre ebenso eine gute Alternative) Nun können wir Cyberduck schließen und uns wieder der Linux-Konsole zuwenden:
Wir stellen per telnet-Protokoll eine Verbindung zum nun übernommenen Router her:
telnet 192.168.31.1
Die Authentifizierung wird erneut mit dem Benutzer „root“ und dem Passwort „root“ ausgeführt. Bei erfolgreicher Verbindung wechseln wir mit dem Befehl
cd /tmp
in den Ordner, wo wir vorhin unter Windows mit dem FTP-Clienten unser OpenWRT image abgelegt haben. Der Befehl
ls
zeigt alle im Ordner vorhandenen Dateien an, natürlich auch unser Image, was wir nun zum flashen des Routers verwenden wollen. Dazu starten wir den Befehl
mtd -e OS1 -r write openwrt.bin OS1

(ACHTUNG wenn man umbenennen nach Download des Images vergessen hat sieht der Befehl so aus: mtd -e OS1 -r write openwrt-22.03.5-ramips-mt7621-xiaomi_mi-router-4a-gigabit-squashfs-sysupgrade.bin OS1   )
und können zusehen, wie nun die Firmware vom Router mit unserem openwrt-Image überschrieben, sprich „geflasht“ wird. Nach kurzer Zeit ist die Installation abgeschlossen und der Router wird selbständig neu starten. Nun werden wir mit der ursprünglichen Verbindung den Router nicht mehr erreichen, da OpenWRT mit der lokalen IP-Adresse 192.168.1.1 gestartet wird.  – Die Linux-Konsole können wir erstmal schließen.
Wir wechseln daher erneut zurück in unsere gewohnte Windows-Umgebung und öffnen dort im beliebigen Browser die das neue Web-Interface vom Xiaomi-Router. Dazu geben wir in der Adress-Zeile einfach die IP Adresse 192.168.1.1 ein. Eine mögliche Sicherheitswarnung können wir hier ignorieren und einfach fortfahren die Seite zu öffnen. Auch ist es im nächsten Schritt nicht nötig, für den Router ein neues Admin-Passwort zu hinterlegen.
Sollte sich diese Seite nicht öffnen können wir relativ einfach überprüfen, ob die Installation vom OpenWRT erfolgreich gewesen ist. Dazu öffnen wir erneut unsere virtuelle Linux-Umgebung und starten nochmals ein Terminal. Hier setzen wir den Befehl
ssh root@192.168.1.1 
ab und sollten als Ergebnis einen Begrüßungstext von OpenWRT bekommen. Wenn auch das nicht funktioniert ist eine detaillierte Fehlersuche nötig. (Die Frage nach einem Verbindunskey wird einfach mit YES beantwortet) Mit dem Befehl
Exit
Schließen wir die SSH-Verbindung zum Router und wechseln zurück in die Windows-Umgebung. Hier gehen wir zurück in den Browser und das User-Interface vom Router, welches wir mit 192.168.1.1 aufgerufen haben.  (Je nach OpenWRT heisst das Menü ein bisschen unterschiedlich)
Nun wechseln wir im Menü zur Update-Möglichkeit der Firmware: Verwalten / Wartung / Firmware – ggf auch unter „System“ dann „Backup / Flash Firmware“ und wählen dort unsere vorher heruntergeladene Freifunk-Firmware der lokalen Freifunk-Community aus, die im Download-Ordner liegen sollte. Diese Datei wählen wir also aus, WICHTIG: >>> leeren die Checkbox „Basiseinstellungen behalten“ und starten den Update-Vorgang für den Router, um statt OpenWRT das Gluon-basierte Freifunk-Image zu laden. Wiederum dauert es ggf. ein paar Minuten und der Router wird wieder von selbst neu gestartet.  Wenn beide LEDs auf der Oberseite vom Router blau flackern sollte der Neustart abgeschlossen sein. 
Nun ist der Router erstmalig zur Konfiguration der Freifunk-Software unter der IP-Adresse 192.168.1.1 erreichbar. ACHTUNG: Wenn der Verbindungsversuch zum Router nicht direkt gelingt machen wir einen manuellen Neustart wie folgt:
Reset-Taster auf der Rückseite vom Gerät gute 10 Sekunden gedrückt halten, bis die Power-LED orange aufleuchtet, dann Taster los lassen. Der Router startet in den Config-Modus und ist nach einigen Sekunden unter der http://192.168.1.1/cgi-bin/config/wizard   erreichbar. 
Achtung, manche Browser versuchen zuerst eine gesicherte https-Verbindung aufzubauen, das wird nicht funktionieren.  Nun kann der Freifunk-Router wie gewohnt eingestellt und angepasst werden.  Manchmal hilft bei Verbindungsschwierigkeiten auch einen anderen Browser zu benutzen, gerade wenn im Cache bereits unterschiedliche FF-Router eingerichtet wurden.

Nun kann wie gewohnt der neue Freifunk-Router eingerichtet werden. Viel Spaß !



Quellen:
•	https://hoddysguides.com/installing-openwrt-on-the-xiaomi-4a-4c-3gv2-4q-miwifi-3c-and-debrick-method-new-2022/
•	https://www.youtube.com/watch?v=SLbkce-M2nE&t=0s
•	https://wiki.freifunk-dresden.de/index.php/Router_einrichten_Xiaomi

Zusammengefasst und dokumentiert dasdigidings e.V. Jens Nowak im August 2023
