M300 - LB1
======

Dieses Repository behandelt die Installation eines Multi-VM-Umgebung mit Vagrant und Virtual Box.

## Einleitung

Diese Dokumentation wurde von David Malic im Rahmen des Moduls M300 (Plattformübergreifende Dienste in ein Netzwerk integrieren)
erarbeitet und zeigt alle Schritte auf, die es zur Einrichtung einer vollständig funktionsfähigen Toolumgebung benötigt.


## Voraussetzungen
* VirtualBox 
* Vagrant
* Text-Editor (z.B. Visual Studio Code)
* Github Account
## Inhaltsverzeichnis
* K1
* K2
* K3

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


Für die Erstellung einer Multi-Maschinen-Umgebung mit Vagrant muss zuallererst das sogenannte Vagrantfile erstellt werden. Dies ist eine Datei, welche alle notwendigen Konfigurationselemente beherrbergt, die zur Erstellung der einzelnen VMs benötigt werden.

Die Datei erfüllt folgende Zwecke:
1. Das Vagrantfile legt fest, wo sich das Projekt befindet. Dieses "Root-Directory" ist für viele Konfigurationseinstellungen unabdingbar.
2. Mit dem Vagrantfile werden Anzahl Maschinen und Ressourcen (inkl. Software) definiert, die für den Betrieb benötigt werden.

## Vagrantfile erstellen
1. Terminal starten
2. Projektordner erstellen, wo das Projekt liegen soll:
    ```Shell
      $ mkdir MeinVagrantProjekt
      $ cd MeinVagrantProjekt
    ```
3. Vagrantfile erstellen:
    ```Shell
      $  vagrant init
    ```

Mit dem letzten Befehl wird das Vagrantfile im aktuellen Verzeichnis `MeinVagrantProjekt` erstellt. 

02 - Box hinzufügen
======

Als nächstes benötigen wir eine Vagrant Box, die uns eine Art "System-Image" (Abbild) liefert, auf welchem wir unsere eigene Konfiguration mit den Services aufbauen können.

Unter https://app.vagrantup.com/boxes/search findet man alle öffentlich verfügbaren Boxen, die frei verwendbar sind. In unserem Fall nutzen wir die `bento/ubuntu-16.04` Box, die [hier](https://app.vagrantup.com/bento/boxes/ubuntu-16.04) näher beschrieben ist.

Vagrant Boxen lassen sich nicht einfach so per Maus-Klick herunterladen. Sie werden direkt im Terminal dem lokalen "Repository" hinzugefügt. Dazu müssen folgende Schritte durchgeführt werden:

1. Terminal starten
2. Vagrant Box hinzufügen:
    ```Shell
      $  vagrant box add bento/ubuntu-16.04
    ```

Der letzte Befehl lädt die Box herunter und fügt sie Vagrant hinzu. Sobald wir nun im Vagrantfile diese Box mitgeben, wird die Anwendung eine VM mit diesem "Image" verwendet. 
 
> Falls die Box noch nicht mit den Befehl `vagrant box add` hinzugefügt wurde, ist dies nicht weiter schlimm. Vagrant sucht automatisch im [Katalog](https://app.vagrantup.com/boxes/search) nach der entsprechenden Box.

03 - SSH-Key
======

## SSH-Key erstellen mit Password und hinzufügen des SSH-Agents
![ssh key](https://user-images.githubusercontent.com/47855918/54729693-3ac17000-4b85-11e9-95b6-e2673ee3df48.png)
SSH-Key wurde erstellt mit dem git-bash. Zusätzlich wurde auch ein Passwort hinterlegt. 
Ausserdem wurde es dem SSH-Agent hinzugefügt


## SSH-Key in Github hinzufügen
![add key](https://user-images.githubusercontent.com/47855918/54729765-a277bb00-4b85-11e9-958d-9c5b299893ea.png)
Der SSH-Key wurde in Github implementiert.


## Repository klonen
![rep klonen](https://user-images.githubusercontent.com/47855918/54730216-f4b9db80-4b87-11e9-95c5-fa678e3d3a67.png)
Die Repository wird von Github auf den Client geklont.
