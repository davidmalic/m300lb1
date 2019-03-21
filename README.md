M300 - LB1
======

Diese Repository behandelt die Umsetzung von der LB1.

## Einleitung
Diese Dokumentation wurde von David Malic im Rahmen des Moduls M300 (Plattformübergreifende Dienste in ein Netzwerk integrieren)
erarbeitet und zeigt alle Schritte auf, die es braucht um die LB1-Kriterien zu erfüllen.

## Voraussetzungen
* VirtualBox 
* Vagrant
* Text-Editor (z.B. Visual Studio Code)
* Github Account


## Inhaltsverzeichnis
* K1
  * VirtualBox
  * Vagrant
  * Visualstudio-Code
  * Git-Client
  * SSH-Key für Client erstellt
* K2
  * Github oder Gitlab-Account ist erstellt
  * Git-Client wurde verwendet
  * Dokumentation ist als Mark Down vorhanden
  * Mark down-Editor ausgewählt und eingerichtet
  * Mark down ist strukturiert
  * Persönlicher Wissenstand
  * Wichtige Lernschritte sind dokumentiert
* K3
* K4
  * Firewall eingrichtet inkl. Rules
  * Reverse-Proxy eingerichet
  * Benutzer- und Rechtevergabe ist eingerichtet
  * Zugang mit SSH-Tunnel abgesichert
  * Sicherheitsmassnahmen sind dokumentiert
  * Projekt mit Git und Mark Down dokumentiert
* K5

___

K1
======

## Virtualbox

Nun widmen wir uns der Virtualisierung von Computersystemen. Für den Betrieb von solchen Maschinen bzw. Computern stehen zahlreiche Virtualisierungsanwendungen zur Verfügung. Eine davon ist VirtualBox. In diesem Kapitel richten wir eine einfache VM (Virtuelle Maschine) mit VirtualBox ein. Also ganz traditionell und wie sich im späteren Verlauf zeigt, auch eine sehr aufwendige Arbeit.

Folgende Arbeiten müssen gemacht werden:

### Software herunterladen & installieren
***
1. Zuerst muss die VirtualBox-Anwendung installiert werden. Der Installer lässt sich [hier](https://www.virtualbox.org"virtualbox.org") herunterladen.
2. Auf "Download VirtualBox 5.2" klicken und bei Abschnitt "VirtualBox 5.2.19 platform packages" dem OS X hosts Link folgen (Datei wird heruntergeladen)
3. Die Installation erfolgt GUI-basiert, jedoch standard (ohne speziellen Anpassungen). Daher wird an dieser Stelle auf eine Erklärung verzichtet.
4. Sobald der Vorgang abgeschlossen wurde, kann mit dem Herunterladen der ISO-Datei und der VM-Erstellung fortgefahren werden.

### ISO-Datei herunterladen
***
Für das weitere Vorgehen wird eine System-Abbild-Datei benötigt. Dazu laden wir in unserem Fall das Image von Ubuntu Desktop 16.04.05 herunter. Wie das genau funktioniert, wird nachfolgend beschrieben:

1. Das Systemabbild (ISO-Image) über [diesen Link](http://releases.ubuntu.com/16.04/ubuntu-16.04.5-desktop-amd64.iso.torrent"ubuntu.com") herunterladen
2. Datei im gewünschten Verzeichnis ablegen (damit das Image wiederverwendet werden kann)
3. Allen Anweisung in Abschnitt "VM erstellen" folgen

### VM erstellen
***
1. VirtualBox starten
2. Links oben, innerhalb der Anwendung, auf `Neu` klicken
3. Im neuen Fenster folgende Informationen eintragen:
   *  Name:           `M300_Ubuntu_16.04_Desktop`
   *  Typ:            `Linux`
   *  Version:        `Ubuntu (64-bit)`
   *  Speichergrösse: `2048 MB`
   *  Platte:         `[X] Festplatte erzeugen`
4. Auf `Erzeugen` klicken
5. Weiteres Fenster öffnet sich, folgende Informationen eintragen:
   *  Dateipfad:                       standard
   *  Dateigrösse:                     `10.00 GB`
   *  Dateityp der Festplatte:         `VMDK (Virtual Maschine Disk)`
   *  Storage on physical hard disk:   `dynamisch alloziert`
6. Ebenefalls auf `Erzeugen` klicken, dann im Hauptmenü die VM anwählen (blau markiert) und den Punkt `Ändern` aufrufen
7. Im Abschnitt `Massenspeicher` den SATA-Controller anwählen und auf das CD+Symbol klicken
8. Unter `Medium auswählen` das zuvor heruntergeladene Systemabbild (ISO-Datei) anwählen
9. Alle Änderungen speichern und die VM starten
10. Den Installationsanweisungen der OS-Installation folgen und anschliessend zu Abschnitt "VM einrichten" gehen


## Vagrant

Vagrant sollte uns zeigen, dass das Bereitstellen virtueller Systeme in der konventionellen Art lange dauert und umständlich sein kann.
Abhilfe bietet hier Vagrant. Vagrant ist eine freie Ruby-Anwendung zur Erstellung und Verwaltung virtueller Maschinen und ermöglicht einfache Softwareverteilung.

Nachfolgend sind einzelne Schritte zur Einrichtung von Vagrant dokumentiert:

### Software herunteladen & installieren
***
1. Die Anwendung in der Version 2.1.4 kann auf der [offiziellen Webseite](https://www.vagrantup.com/ "vagrantup.com") heruntergeladen werden.
2. Die Installation erfolgt, wie alle anderen Anwendungen, GUI-basiert, jedoch standard (ohne speziellen Anpassungen). Daher wird an dieser Stelle ebenfalls auf eine Erklärung verzichtet.
3. Sobald der Vorgang abgeschlossen wurde, kann mit dem Erstellen einer VM fortgefahren werden. 


### Virtuelle Maschine erstellen
***
1. Terminal öffnen
2. In gewünschtem Verzeichnis einen neuen Ordner für die VM anlegen:
    ```Shell
      $ cd Wohin\auch\immer
      $ mkdir MeineVagrantVM
      $ cd MeineVagrantVM
    ``` 
3. Vagrantfile erzeugen, VM erstellen und entsprechend starten:
    ```Shell
      $ vagrant init ubuntu/xenial64        #Vagrantfile erzeugen
      $ vagrant up --provider virtualbox    #Virtuelle Maschine erstellen & starten
    ``` 
4. Die VM ist nun in Betrieb (erscheint auch in der Übersicht innerhalb von VirtualBox) und kann via SSH-Zugriff bedient werden:
    ```Shell
      $ cd Pfad\zu\meiner\Vagrant-VM      #Zum Verzeichnis der VM wechseln
      $ vagrant ssh                       #SSH-Verbindung zur VM aufbauen

      #Anschliessend können ganz normale Bash-Befehle abgesetzt werden:

      $ ls -l /bin  #Bin-Verzeichnis anzeigen
      $ df -h       #Freier Festplattenspeicher
      $ free -m     #Freier Arbeitsspeicher
    ``` 
5. VM über VirtualBox-GUI ausschalten

Schlussfolgerung: Eine VM lässt sich mit Vagrant eindeutig schneller und unkomplizierter erstellen!


### Virtuelle Maschine erstellen (mit Vagrant-Box auf Netzwerkshare)
***
1. Terminal öffnen
2. In gewünschtem Verzeichnis einen neuen Ordner für die VM anlegen:
    ```Shell
      $ cd Wohin\auch\immer
      $ mkdir MeineVagrantVM
      $ cd MeineVagrantVM
    ``` 
3. Vagrantfile erzeugen, VM erstellen und entsprechend starten:
    ```Shell
      $ vagrant box add http://[HOST]/vagrant/ubuntu/xenial64.box --name ubuntu/xenial64  #Vagrant-Box vom Netzwerkshare hinzufügen
      $ vagrant init ubuntu/xenial64                                                      #Vagrantfile erzeugen
      $ vagrant up --provider virtualbox                                                  #Virtuelle Maschine erstellen & starten
    ``` 
4. Die VM ist nun in Betrieb (erscheint auch in der Übersicht innerhalb von VirtualBox) und kann via SSH-Zugriff bedient werden:
    ```Shell
      $ cd Pfad\zu\meiner\Vagrant-VM      #Zum Verzeichnis der VM wechseln
      $ vagrant ssh                       #SSH-Verbindung zur VM aufbauen

      #Anschliessend können ganz normale Bash-Befehle abgesetzt werden:

      $ ls -l /bin  #Bin-Verzeichnis anzeigen
      $ df -h       #Freier Festplattenspeicher
      $ free -m     #Freier Arbeitsspeicher
    ``` 
5. VM über VirtualBox-GUI ausschalten

Schlussfolgerung: Keine erheblichen Unterschiede zum ersten Teil (ohne Share) und daher auch nicht wirklich kompliziert.

## Visual Studio Code

Bis hierhin haben wir soweit alles aufgesetzt und installiert. Nun möchten wir für effizienteres Arbeiten eine "Entwicklungsumgebung" aufbauen, die es uns ermöglicht, alle lokalen Repositories an einem Ort zu verwalten und die dazugehörigen Dateien zu bearbeiten. Die Lösung hierzu ist: Visual Studio Code 
Dieser freie Quelltext-Editor von Microsoft, ermöglicht uns, unsere Workflows besser zu gestalten und damit die Arbeit um einiges leichter zu machen.

Ich habe jedoch nicht mit Visual Studio Code gearbeitet. Ich habe es direkt im Github Browser Code bearbeitet.

### Software herunteladen & installieren
***
1. Unter [dieser Webseite](https://code.visualstudio.com/"visualstudio.com") lässt sich der Installer (Version 1.26.1) herunterladen.
2. Auf "Download for Mac" klicken und warten, bis das Fenster zum Herunterladen erscheint. Anschliesend den Download des Installers starten
3. Die Installation erfolgt auch hier GUI-basiert. Wiederum aber standard (ohne speziellen Anpassungen), sodass an dieser Stelle auf eine Erklärung ebenfalls verzichtet wird .
4. Sobald der Vorgang abgeschlossen wurde, kann mit dem Herunterladen der ISO-Datei und der VM-Erstellung fortgefahren werden.



## Git-Client

### Account erstellen
***
Als erster Schritt muss ein GitHub-Account eingerichtet werden. Dieser dient uns später als "Cloud-Speicher" unserer Dokumentation und weiteren Dateien.

Folgende Arbeiten müssen gemacht werden:


***
1. Auf www.github.com ein Benutzerkonto erstellen (Angabe von Username, E-Mail und Passwort)
2. E-Mail zur Verifizierung des Kontos bestätigen und anschliessend auf GitHub anmelden


### Repository erstellen

1. Anmelden unter www.github.com 
2. Innerhalb der Willkommens-Seite auf <strong>Start a project</strong> klicken
3. Unter <strong>Repository name</strong> einen Name definieren (z.B. M300)
4. Optional: kurze Beschreibung eingeben
5. Radio-Button bei <strong>Public</strong> belassen
6. Haken bei <strong>Initialize this repository with a README</strong> setzen
7. Auf <strong>Create repository</strong> klicken
   

## SSH-Key für Client erstellt

![ssh key](https://user-images.githubusercontent.com/47855918/54729693-3ac17000-4b85-11e9-95b6-e2673ee3df48.png)
SSH-Key wurde erstellt mit dem git-bash. Zusätzlich wurde auch ein Passwort hinterlegt. 
Ausserdem wurde es dem SSH-Agent hinzugefügt

1.  Terminal öffnen
2.  Folgenden Befehl mit der Account-E-Mail von GitHub einfügen:
    ```Shell
      $  ssh-keygen -t rsa -b 4096 -C "beispiel@beispiel.com"
    ```
3. Neuer SSH-Key wird erstellt:
    ```Shell
      Generating public/private rsa key pair.
    ```
4. Bei der Abfrage, unter welchem Namen der Schlüssel gespeichert werden soll, die Enter-Taste drücken (für Standard):
    ```Shell
      Enter a file in which to save the key (~/.ssh/id_rsa): [Press enter]
    ```
5. Nun kann ein Passwort für den Key festgelegt werden. Ich empfehle dieses zu setzen und anschliessend dem SSH-Agent zu hinterlegen, sodass keine erneute Eingabe (z.B. beim Pushen) notwendig ist:
    ```Shell
      Enter passphrase (empty for no passphrase): [Passwort]
      Enter same passphrase again: [Passwort wiederholen]
    ```






### SSH-Key hinzufügen
***
1.  Anmelden unter www.github.com
2.  Auf Benutzerkonto klicken (oben rechts) und den Punkt <strong>Settings</strong> aufrufen
3.  Unter den Menübereichen auf der linken Seite zum Abschnitt <strong>SSH und GPG keys</strong> wechseln
4.  Auf <strong>New SSH key</strong> klicken
5.  Im Formular unter <strong>Title</strong> eine Bezeichnung vergeben (z.B. MB SSH-Key)
6.  Den zuvor kopierten Key mit <i>CTRL + V</i> einfügen und auf <strong>Add SSH key</strong> klicken
7.  Der Schlüssel (SSH-Key) sollte nun in der übergeordneten Liste auftauchen


### SSH-Key in Github hinzufügen
***
![add key](https://user-images.githubusercontent.com/47855918/54729765-a277bb00-4b85-11e9-958d-9c5b299893ea.png)
Der SSH-Key wurde in Github implementiert.


### Repository klonen
***
![rep klonen](https://user-images.githubusercontent.com/47855918/54730216-f4b9db80-4b87-11e9-95c5-fa678e3d3a67.png)
Die Repository wird von Github auf den Client geklont.

### Repository hochladen (Push)
***
1.  Terminal öffnen (nachdem Teile bzw. Dateien des lokalen Repositorys geändert wurden)
2.  In das entsprechende Verzeichnis des Repository gehen: 
    ```Shell
      $ cd Pfad\zu\meinem\Repository  
    ```  
3.  Dateien dem Upload hinzufügen:
    ```Shell
      $ git add -a.
    ``` 
4.  Den Upload commiten:
    ```Shell
      $ git commit -m "Mein Kommentar"
    ``` 
5.  Schliesslich den Upload pushen:
    ```Shell
      $ git push
    ```
6.  Nun sollte der Master-Branch des Repositorys ebenfalls aktualisiert sein

### Übersicht "How to Push"
***

Dieser Abschnitt zeigt die Handhabung von Git-Befehlen auf. Mit den nachfolgenden Kommandos pusht man das (geänderte) Repository zu seinem GitHub-Repository.

Wichtig: Die Befehle müssen innerhalb des lokalen Repositorys ausgeführt werden!

```Shell 
$  cd Pfad\zu\meinem\Repository    # Zum lokalen GitHub-Repository wechseln

$  git status                      # Geänderte Datei(en) werden rot aufgelistet
$  git add -a                      # Fügt alle Dateien zum "Upload" hinzu
$  git status                      # Der Status ist nun grün > Dateien sind Upload-bereit (Optional) 
$  git commit -m "Mein Kommentar"  # Upload wird "commited" > Kommentar zu Dokumentationszwecken ist dafür notwendig
$  git status                      # Dateien werden nun als "zum Pushen bereit" angezeigt
$  git push                        #Upload bzw. Push wird durchgeführt
```

K2
=====

## Github oder Gitlab-Account ist erstellt


Ein GitHub-Account konnte ich problemlos erstellen.



## Git-Client wurde verwendet
Mein Git-Client ist aktiv und mein repo wurde auch geklont.


## Dokumentation ist als Markdown vorhanden, Editor ausgewählt und eingerichtet, strukturiert

Die ganze Dokumentation habe ich im Github Code Browser geschrieben.



## Persönlicher Wissenstand im Bezug auf die wichtigsten Themen sind dokumentiert (Linux, VM, Vagrant, Git, .md, Sicherheit)

- Linux: 

  Ich habe im Geschäft schon mit Linux zu tun gehabt. Daher hatte ich vor dem Projekt grundlegendes Basiswissen über Linux

- Virtualisierung: 

  In ÜKs und im Geschäft hatte ich sehr viel zu tun mit Virtualisierung, deswegen kenne ich mich mit VMs einigermassen gut aus.

- Vagrant: 

  Vagrant kannte ich vorher nicht. Ich arbeite zum ersten Mal mit Vagrant. 

- Versionsverwaltung:

  Die Versionsverwaltung habe ich bis jetzt eigentlich garnicht verwendet. 
  
- Git & Markdown 

  Mit Git und Markdown hatte ich bis jetzt nichts am Hut, das ist alles neu für mich.

- Systemsicherheit

  Mit Systemsicherheit hatte ich in der Vergangenheit ab und zu zu tun. Kenne mich mässig aus.
  

K3
=====

## Bestehende vm aus Vagrant-Cloud einrichten. 

```# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.define "database" do |db|
    db.vm.box = "ubuntu/xenial64"
	db.vm.provider "virtualbox" do |vb|
	  vb.memory = "512"  
	end
    db.vm.hostname = "db01"
    db.vm.network "private_network", ip: "192.168.55.100"
    # MySQL Port nur im Private Network sichtbar
	# db.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: false
  	db.vm.provision "shell", path: "db.sh"
  end
  
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/xenial64"
    web.vm.hostname = "web01"
    web.vm.network "private_network", ip:"192.168.55.101" 
	web.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
	web.vm.provider "virtualbox" do |vb|
	  vb.memory = "512"  
	end     
  	web.vm.synced_folder ".", "/var/www/html"  
	web.vm.provision "shell", inline: <<-SHELL
		sudo apt-get update
		sudo apt-get -y install debconf-utils apache2 nmap
		sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password admin'
		sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password admin'
		sudo apt-get -y install php libapache2-mod-php php-curl php-cli php-mysql php-gd mysql-client  
		# Admininer SQL UI 
		sudo mkdir /usr/share/adminer
		sudo wget "http://www.adminer.org/latest.php" -O /usr/share/adminer/latest.php
		sudo ln -s /usr/share/adminer/latest.php /usr/share/adminer/adminer.php
		echo "Alias /adminer.php /usr/share/adminer/adminer.php" | sudo tee /etc/apache2/conf-available/adminer.conf
		sudo a2enconf adminer.conf 
		sudo service apache2 restart 
	  echo '127.0.0.1 localhost web01\n192.168.55.100 db01' > /etc/hosts
SHELL
	end  
 end
```

K4
=====

## Firewall eingerichtet inkl. Rules
Eine lokale Firewall wird installiert und anschliessend auch konfiguriert. Dabei öffnen man den Port 22 um via SSH darauf zuzugreifen. INFO-INPUT SSH Port 22 | TELNET Port 23
```
#Local Firewall installieren
sudo apt-get -y install ufw gufw 
sudo ufw allow from 10.0.2.2 to any port 22
sudo ufw --force enable    
```
Nun ist die DHCP-Part abgeschlossen. Man kann jetzt Clients VM erstellen und mit dem DHCP-Server innerhalb IP-Range IP-Adresse bekommen.

## Reverse-Proxy eingerichtet

## Benutzer- und Rechtevergabe ist eingerichtet
Im nächsten Schritt wird eine Gruppe, inklusive Benutzer mit Passwort erstellt. Dies geht folgendermassen: 
```
#Gruppe Myadmin erstellen
sudo groupadd myadmin
#User erstellen
sudo useradd admin -g myadmin -m -s /bin/bash
sudo useradd test -g myadmin -m -s /bin/bash
#Password festlegen
sudo chpasswd <<<admin:admin
sudo chpasswd <<<test:test
```

### DHCP-Server
***
Die DHCP VM hat folgende Spezifikationen (Die Änderungen kann man im Vagrant-File vornehmen):
* IP: 192.168.6.5
* Hostname: dhcp
* RAM: 1024 MB
* VM Box: Ubuntu

```
config.vm.define "dhcp" do |dhcp|
  dhcp.vm.box = "ubuntu/xenial64"
  dhcp.vm.hostname = "dhcp"
  dhcp.vm.network "private_network", ip:"192.168.6.5" 
	dhcp.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"  
end  
```
Der erstes Schritt ist die Paketverzeichnis aktualisiert wird. 

```
sudo apt-get update
```

Bei nächsten Schritt wird der DHCP Server installiert. Das Paket lautet: ISC-DHCP-SERVER.
```
sudo apt-get -y install isc-dhcp-server
```
Nach der Installation von DHCP-Server muss man die Konfiguration anpassen. Das Konfigurationfile vom DHCP Server befindet sich im Pfad /etc/dhcp/dhcpd.conf. Bei dem Konfigurationfile wird folgendes geändert:
* Domainname
* DNS
* DHCP Scope

Der Domainname lautet Standardmässig example.org und will neu labor.local umändern. 
```
#Domainanme konfigurieren
sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
```
Der DNS wird nun auf 8.8.8.8 (Google DNS) konfiguriert.
```
#DNS konfigurieren
sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
```
Im Bereich beim Scope hat folgende Parameter:
* Subnet --> 192.168.6.0/24
* Range --> 192.168.6.100 - 130
* Gateway --> 192.168.6.1
```
#DHCP Autorisierung aktiviert
sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
#DHCP Subnetz & Maske konfigurieren
sudo sed -i '$asubnet 192.168.6.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
#DHCP Range konfigurieren
sudo sed -i '$arange 192.168.6.100 192.168.6.130;' /etc/dhcp/dhcpd.conf
#DHCP Gateway konfigurieren
sudo sed -i '$aoption routers 192.168.6.1;' /etc/dhcp/dhcpd.conf
sudo sed -i '$a}' /etc/dhcp/dhcpd.conf
```
Nach der Änderung des Konfigurationsfile wird der DHCP Service neu gestartet.
```
#DHCP Server neustarten
sudo service isc-dhcp-server restart
```
Die Tastaturlayout muss man noch auf Deutsch Schweiz anpassen.
```
#Tastaturlayout anpassen
sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
```


FTP-Server
----------
Die FTP VM hat folgende Spezifikationen (Die Änderungen kann man im Vagrant-File vornehmen):
* IP: 192.168.6.6
* Hostname: ftp
* RAM: 1024 MB
* VM Box: Ubuntu

```
config.vm.define "ftp" do |ftp|
  ftp.vm.box = "ubuntu/xenial64"
  ftp.vm.hostname = "ftp"
  ftp.vm.network "private_network", ip: "192.168.6.6"
  ftp.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: true
  ftp.vm.provider "virtualbox" do |vb|
	 vb.memory = "1024"  
end
```
Der erstes Schritt ist erneut die Paketverzeichnis zu akualisieren. 

```
sudo apt-get update
```
Bei nächsten Schritt wird der FTP Server installiert. Das Paket lautet: pure-ftpd
```
#FTP Server installieren
sudo apt-get -y install pure-ftpd-common pure-ftpd
```
Nach der Installation kann ich die FTP-Server konfigurieren. Bei meinem Fall sieht es so aus: 
```
sudo groupadd ftpgroup
sudo useradd -g ftpgroup -d /dev/null -s /etc ftpuser
sudo pure-pw useradd david -u ftpuser -g ftpgroup -d /home/pubftp/david -N 10
```
Nach der Änderung wird der FTP Service neu gestartet.
```
#FTP Server neustarten
sudo service pure-ftpd-common pure-ftpd restart
#sudo /home/pubftp/david restart
```
Die Tastaturlayout muss man noch auf Deutsch Schweiz anpassen.
```
#Tastaturlayout anpassen
sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
```
Am Ende wird noch eine lokale Firewall installiert und anschliessend auch konfiguriert. Dabei öffnen man den Port 22 um via SSH darauf zuzugreifen. INFO-INPUT SSH Port 22 | TELNET Port 23
```
#Local Firewall installieren
sudo apt-get -y install ufw gufw 
sudo ufw allow from 10.0.2.2 to any port 22
sudo ufw --force enable    
```
Nun ist die FTP-Part abgeschlossen.

Apache-Server
----------
Die Apache VM hat folgende Spezifikationen (Die Änderungen kann man im Vagrant-File vornehmen):
* IP: 192.168.6.7
* Hostname: apache
* RAM: 1024 MB
* VM Box: Ubuntu

```
config.vm.define "apache" do |apache|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "apache"
  config.vm.network "private_network",ip:"192.168.6.7",netmask:"255.255.255.0",default_gateway:"192.168.6.1"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"
  config.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
end
```
Der erstes Schritt ist ebenfalls erneut die Paketverzeichnis zu aktualisieren. 

```
sudo apt-get update
```
Im nächsten Schritt wird eine Gruppe mit inklusvie Benutzer mit Passwort erstellt. Es sieht so aus: 
```
#Gruppe Myadmin erstellen
sudo groupadd myadmin
#User erstellen
sudo useradd admin -g myadmin -m -s /bin/bash
sudo useradd test -g myadmin -m -s /bin/bash
#Password festlegen
sudo chpasswd <<<admin:admin
sudo chpasswd <<<test:test
```
Bei nächsten Schritt wird der Apache Server installiert und die Service wird neugestartet. Das Paket lautet: apche2
```
#Installation Apache2
sudo apt-get -y install apache2
#Apache2 Service neustarten
sudo service apache2 restart
```
Die Tastaturlayout muss man noch auf Deutsch Schweiz anpassen.
```
#Tastaturlayout anpassen
sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
```


K5
=====

## Vergleich Vorwissen - Wissenszuwachs
Ich habe gelernt wie man mit einem Vagrant File eine VM erstellt nach seinen Bedürfnissen. Ausserdem habe ich mir sehr viel Fachwissen bezüglich Git, Markdown , Linux und Virtualisierung angeignet. Am Anfang hatte ich nur Basis Wissen in den meisten Bereichen. Durch dieses Modul konnte ich mein Wissen sehr vertiefen. 

## Reflexion
Ich hatte noch nie etwas von Vagrant gehört. Jedoch als es Herr Kälin uns erklärte, fand ich grosses Interesse. Eine VM zu erstellen mit einem Text-File nach seinen Bedürfnissen? Das ist sehr praktisch und zeitsparend. Was mir besonders gefiel war das Erarbeiten der Dokumentation, da ich bis anhin den Funktionsumfang von GitHub in Kombination mit Markdown nicht kannte. Da für mich alles sehr neu war, musste ich mich in einer ersten Phase erst einmal in die einzelnen Bereiche einarbeiten und Schritt für Schritt die Anweisungen befolgen. Grösstenteils hatte ich dabei keine Mühe und ich konnte bereits in geraumer Zeit einen Grossteil der Aufgaben abschliessen. Am Schluss hatte ich ein bisschen Zeitdruck konnte jedoch alles noch gut erledigen. 

Ich konnte vieles lernen und bedanke mich beim Herrn Kälin für seine Unterstützung. In Zukunft werde ich Github auf jeden Fall für nachfolgende Projekte brauchen. 
