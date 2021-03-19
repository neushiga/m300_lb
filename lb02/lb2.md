# Samba Dokumentation 

## Inhaltsverzeichnis
1. Einleitung
2. Code Beschreibung
3. Korrekteheit der Angaben
4. Testen
5. Quellenangaben

## 1. Einleitung
- Client wird mit Ubuntu aufgesetzt
- Beim Aufstarten des Ubuntu Clients wird ein Samba Dienst gestartet und ein Samba Share zur Verfügung gestellt
- Kann: "bei jedem Start wird das vagrant-logfile automatisch in einen Samba-Share kopiert, um den Status von vagrant schnell überprüfen zu können."


## 2. Code Beschreibung

Ordner m300_lb und Unterordner lb02 im Verzeichnis erstellen. 
```bash Codiger Code ist hier gecoded
mkdir m300_lb
cd m300_lb
mkdir lb02
vagrant init
```

Vagrantfile im Ordner lb02 generieren
```bash Codiger Code ist hier gecoded
vagrant init
```

Die Verbindung via SSH zum Github wird wie folgt aufgebaut:
```bash Codiger Code ist hier gecoded
vagrant ssh
```

Dieser Vagrantfile wird im Visual Studio bearbeitet.
Ich habe mich für die "ubuntu/xenial64" entschieden, weil bei deren Trusty und CentOS Probleme aufgetaucht sind, die ich nicht lösen konnte.
```bash Codiger Code ist hier gecoded
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/xenial64"
```

 Hier defnieire ich die IP-Adresse, Network auf public, Bridge und den Hostnamen. Den .define habe ich auch den Hostnamen eingegeben.
 ```bash Codiger Code ist hier gecoded
    config.vm.network "public_network", ip: "192.168.56.20", 
    bridge: "Intel(R) Dual Band Wireless-AC 7265"
    config.vm.hostname = "Samba-Client"
    config.vm.define "Samba-Client"
```

Hier lege ich den vm.provider als Virtualbox fest und ändere den vb.gui auf false. Memory lege ich auf 2048 fest und die vb.cpus auf 2 Kerne. Schlussendlich lege ich hier den vb.name fest.
```bash Codiger Code ist hier gecoded
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
    vb.gui = false
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
    vb.cpus = "2"
    vb.name = "Samba-Client"
   end
```

### Samba
Am Ende vom Vagrantfile gebe ich folgende Zeilen an, damit Samba installiert wird. 
```bash Codiger Code ist hier gecoded
    config.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y samba
    SHELL
end
```

Wir geben folgendes ein:
```bash Codiger Code ist hier gecoded
vagrant ssh
```

Mit folgendem Befehl sehen wir den Status von smbd
```
systemctl status smbd
```
Hier grün zeigt es an, dass der Service läuft. 
![](Screenshot\Status_smbd.JPG) 

Wir gehen in das Verzeichnis und /etc/samba und sehen das smb.conf File. 
```
cd /etc/samba
```
![](Screenshot\smbconfig_file.JPG) 

Wir nennen das File um zu "smbold.conf"
```bash Codiger Code ist hier gecoded
sudo mv smb.conf smbold.conf
```
Um zu sehen, ob es wirklich geändert hat, kann ich dies mit dem Befehl "ls" überprüefen.
![](Screenshot\smbold.JPG) 

Samba hat jetzt kein Configurationsfile. Nun wird der Samba Service gestoppt. 





















## 3. Korrekteheit der Angaben

## 4. Testen
 
Vagrantfile

VM
Mit ```vagrant up```  kann man versuechen, die VM zu starten, dabei wird das Ganze durchgelaufen:
![](Screenshot\vagrant_up.JPG) 

Oder man kann spontan im VirtualBox nachsehen, ob es diesen generiert hat:
![](Screenshot\VM.JPG)

### Verbindung SSH
Commiten und pushen und im GitHub nachsehen!.
```
git add A . 
git commit -m "Kommentar"
git push
```
## 5. Quellenangaben

Die ubuntu/xenial64 Box habe ich von der Folgender Webseite:
https://app.vagrantup.com/boxes/search