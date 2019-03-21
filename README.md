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
** Virtual Box
** Vagrant
** Visual Studio Code
** Git-Client
** SSH-Key für Client erstellt
* K2
* K3
* K4
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

Für die Einrichtung muss man sich nach den nachfolgenden Anweisungen orientieren:

 

### Repository hinzufügen & pushen
***
1. Visual Studio Code öffnen
2. Änderungen an entsprechenden Dateien des lokalen Repositorys vornehmen
3. In der linken Leiste das Symbol mit einer "1" aufrufen
4. Unter dem Abschnitt **Changes** die betroffenen Files bezüglich ihres Changes "stagen" (**Stage Changes**)
5. Nachricht hinterlegen (**Message**) und Haken (**Commit**) setzen
6. Bei den 3 Punkten (...) die Funktion **Push** aufrufen
7. Warten, bis Dateien vollständig gepusht wurden

## Git-Client
### Account erstellen
Als erster Schritt muss ein GitHub-Account eingerichtet werden. Dieser dient uns später als "Cloud-Speicher" unserer Dokumentation und weiteren Dateien.

Folgende Arbeiten müssen gemacht werden:


***
1. Auf www.github.com ein Benutzerkonto erstellen (Angabe von Username, E-Mail und Passwort)
2. E-Mail zur Verifizierung des Kontos bestätigen und anschliessend auf GitHub anmelden


### Repository erstellen
***
1. Anmelden unter www.github.com 
2. Innerhalb der Willkommens-Seite auf <strong>Start a project</strong> klicken
3. Unter <strong>Repository name</strong> einen Name definieren (z.B. M300)
4. Optional: kurze Beschreibung eingeben
5. Radio-Button bei <strong>Public</strong> belassen
6. Haken bei <strong>Initialize this repository with a README</strong> setzen
7. Auf <strong>Create repository</strong> klicken
   

### SSH-Key erstellen (lokal)
***
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

K4
=====


K5
=====

## Reflexion
Ich hatte noch nie etwas von Vagrant gehört. Jedoch als es Herr Kälin uns erklärte, fand ich grosses Interesse. Eine VM zu erstellen mit einem Text-File nach seinen Bedürfnissen? Das ist sehr praktisch und zeitsparend. Was mir besonders gefiel war das Erarbeiten der Dokumentation, da ich bis anhin den Funktionsumfang von GitHub in Kombination mit Markdown nicht kannte. Da für mich alles sehr neu war, musste ich mich in einer ersten Phase erst einmal in die einzelnen Bereiche einarbeiten und Schritt für Schritt die Anweisungen befolgen. Grösstenteils hatte ich dabei keine Mühe und ich konnte bereits in geraumer Zeit einen Grossteil der Aufgaben abschliessen. Am Schluss hatte ich ein bisschen Zeitdruck konnte jedoch alles noch gut erledigen. 

Ich konnte vieles lernen und bedanke mich beim Herrn Kälin für seine Unterstützung. In Zukunft werde ich Github auf jeden Fall für nachfolgende Projekte brauchen. 
