M300 - LB1
======

Dieses Repository behandelt die Installation eines Multi-VM-Umgebung mit Vagrant und Virtual Box.

#### Einleitung

Diese Dokumentation wurde von David Malic im Rahmen des Moduls M300 (Plattformübergreifende Dienste in ein Netzwerk integrieren)
erarbeitet und zeigt alle Schritte auf, die es zur Einrichtung einer vollständig funktionsfähigen Toolumgebung benötigt.


#### Voraussetzungen
* VirtualBox 
* Vagrant (
* Text-Editor (z.B. Visual Studio Code)

#### Inhaltsverzeichnis
* 01 - Vorbereitungen
* 02 - Box hinzufügen
* 03 - SSH-Key

___

01 - Vorbereitungen
======

Für die Erstellung einer Multi-Maschinen-Umgebung mit Vagrant muss zuallererst das sogenannte Vagrantfile erstellt werden. Dies ist eine Datei, welche alle notwendigen Konfigurationselemente beherrbergt, die zur Erstellung der einzelnen VMs benötigt werden.

Die Datei erfüllt folgende Zwecke:
1. Das Vagrantfile legt fest, wo sich das Projekt befindet. Dieses "Root-Directory" ist für viele Konfigurationseinstellungen unabdingbar.
2. Mit dem Vagrantfile werden Anzahl Maschinen und Ressourcen (inkl. Software) definiert, die für den Betrieb benötigt werden.

### Vagrantfile erstellen
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

<p><img src="https://onedrive.live.com/view.aspx?resid=492F7CD056F0D2BB%216068&id=documents&wd=target%28m300.one%7CB3045686-7C3C-4C06-981F-9802D0AC178B%2Fad%7CB4C078A5-06F6-421A-8536-F312CA26A2FF%2F%29
onenote:https://d.docs.live.net/492f7cd056f0d2bb/Dokumente/Notizbuch%20von%20David/m300.one#ad&section-id={B3045686-7C3C-4C06-981F-9802D0AC178B}&page-id={B4C078A5-06F6-421A-8536-F312CA26A2FF}&object-id={2C215ADB-3EB0-4F15-9AB9-D69EC621A5A3}&13" alt="SSH"></p>
  
