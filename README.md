Aufgabenstellung
Der Auftrag war ein Projekt basiert auf "Infrastructure as Code" wählen und damit einen Service oder Dienst zu automatisieren.

Code
 Um die Konfiguration für die Virtuelle Maschine zu definieren wurde dieser Code verwendet:

 Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1804"
 #portforwarding
  config.vm.network "forwarded_port", guest: 80, host: 2988
  config.ssh.forward_agent = true
#
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox",
    owner: "www-data", group: "www-data"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "vagrant_M300_pamuk"
    vb.memory = 1024
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provision "shell" do |s|
    s.args = [time_zone]
    s.inline = <<-SHELL

    Danach kommen die Updates und Pakete, die installiert werden müssen:
    TIME_ZONE=$1
# einfach instalieren und nicht auf y oder n warten
export DEBIAN_FRONTEND=noninteractive 
# Zeitzone updaten
timedatectl set-timezone "$TIME_ZONE"
# Alle Pakages updaten
apt-get update -q
# Vim und Git installieren für repositorys clonen und vim für danach in das WWW file schreiben
apt-get install -q -y vim git
# instalieren von Apache Mariadb PHP 
apt-get install -q -y apache2
apt-get install -q -y php7.2 libapache2-mod-php7.2
apt-get install -q -y php7.2-curl php7.2-gd php7.2-mbstring php7.2-mysql php7.2-xml php7.2-zip php7.2-bz2 php7.2-intl
apt-get install -q -y mariadb-server mariadb-client

Lokales Directory wird erstellt:
dir='/vagrant/www'
if [ ! -d "$dir" ]; then
  mkdir "$dir"
fi
if [ ! -L /var/www/html ]; then
  rm -rf /var/www/html
  ln -fs "$dir" /var/www/html
fi
cd "$dir"

Testing

Nach Vagrant Up kann man über den definierten Port, in meinem Fall 2988 auf die Seite zugreifen.

localhost:2988

Über das Gui gelangt man dann auf das Login von Adminer und gibt die Login Daten ein.

User: Admin
Passwort: rootpw

Quellenangaben

 Ein Teil vom Code: https://gist.github.com/aurmil/e346aec64c3f6b6ea17259f41e3b6ab0 

 Jorin hat mir geholfen, da ich ein Problem mit Vagrant hatte. Die Lösung war am Schluss das ganze zu löschen und neu zu installieren.

