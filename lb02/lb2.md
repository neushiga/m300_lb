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

Mit ```vagrant up```  kann man versuechen, die VM zu starten. Dabei wird das Ganze durchgelaufen:
![](Screenshot\vagrant_up.JPG) 


Oder man kann spontan im VirtualBox nachsehen, ob es diesen generiert hat:

![](Screenshot\VM.JPG)


Die Verbindung via SSH zum Github wird wie folgt aufgebaut:
```bash Codiger Code ist hier gecoded
vagrant ssh
```


Um zu sehen, ob es funktioniert hat, kann man commiten und pushen. Im Github kann man nachsehen, ob das Verzeichnis dort aktualisiert wurde!
```
git add -A  
git commit -m "Kommentar"
git push
```


Dieser Vagrantfile wird im Visual Studio bearbeitet.
Ich habe mich für die "ubuntu/xenial64" entschieden, weil bei deren Trusty und CentOS Probleme aufgetaucht sind, die ich nicht lösen konnte.
```bash Codiger Code ist hier gecoded
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/xenial64"
```


 Hier definiere ich das Network auf public, IP-Adresse, Bridge und den Hostnamen. Beim **.define** habe ich auch den Hostnamen eingegeben.
 ```bash Codiger Code ist hier gecoded
    config.vm.network "public_network", ip: "192.168.56.20", 
    bridge: "Intel(R) Dual Band Wireless-AC 7265"
    config.vm.hostname = "Samba-Client"
    config.vm.define "Samba-Client"
```


Hier lege ich den vm.provider als Virtualbox fest und ändere den vb.gui auf false, weil ich nur die Kommandozeile brauche. Memory lege ich auf 2048 fest und die cpus auf 2 Kerne. Schlussendlich lege ich hier noch den **vb.name** fest.
```bash Codiger Code ist hier gecoded
   config.vm.provider "virtualbox" do |vb|
   vb.gui = false
  #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
    vb.cpus = "2"
    vb.name = "Samba-Client"
   end
```


### Samba
Am Ende vom Vagrantfile gebe ich folgende Zeilen an, damit Samba installiert wird. Normalerweise fragt es an, ob ich noch ein Packet installieren will und mit **-y** gebe ich an, dass ich das will. Somit wird es während des instrallierens nicht nachgefragt.
```bash Codiger Code ist hier gecoded
    config.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y samba
    SHELL
end
```


Wir geben folgendes ein, um auf der SSH Verbindung zu arbeiten:
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
sudo mv /etc/samba/smb.conf /etc/samba/smbold.conf
```


Um zu sehen, ob es wirklich geändert hat, kann ich dies mit dem Befehl **ls** oder **ll** überprüfen.

![](Screenshot\smbold.JPG) 


Wir erstellen mit **mkdir** einen neuen Ordner im Verzeichnis /home/vagrant/.
```bash Codiger Code ist hier gecoded
mkdir /home/vagrant/tshare
```

Wenn wir im Verzeichnis /home/vagrant/tshare **ll** eingeben, sehen wir den **root** als den Standarduser. 

![](Screenshot\root.JPG) 

Wir wollen aber vagrant als den Standarduser und ändern es mit **chown**.
```bash Codiger Code ist hier gecoded
sudo chown vagrant:vagrant /home/vagrant/tshare
```

## 3. Korrekteheit der Angaben
![](Screenshot\GrafischeÜbersicht.JPG) 

## 4. Testen
 

## 5. Quellenangaben

### ubuntu/xenial64 Box Webseite 
https://app.vagrantup.com/boxes/search

### Markdown
https://www.markdownguide.org/basic-syntax/

### Samba
https://www.youtube.com/watch?v=oRHSrnQueak&ab_channel=ChrisTitusTech