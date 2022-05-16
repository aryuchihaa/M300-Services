# Dokumentation zur Leistungsbeurteilung 3 
## Inhalt 
**1. Aufgabenstellung**
   
**2. Vorbereitung**

**3. Umsetzung**
   
**4. Code**

**5. Reflexion**
   
**6. Quellenangaben**

## 1. Aufgabenstellung

Die Aufgabenstellung sieht wie folgt aus:
Sie erstellen ein selbst gewähltes Projekt, welches auf der Docker Container-Technologiebasiert. Dabei erstellen sie einen Service, der innerhalb eines oder mehrerer Container implementiert wird.Die Implementation des Container-Projektserfolgt als Einzelarbeit. Der erstellte Code sowie die gesamte Dokumentation müssenversioniertauf GitHubhinterlegt und der Lehrpersonzugänglich sein (Lese-Rechte).Das Internet ist eine wichtige Ressource für solche Projekte. Entsprechend dürfen sie auch Codebeispiele aus dem Internet verwenden.Sie müssen solchen Code immer mit der Angabe der Quelleversehen.Der verwendete Code muss von ihnen vollständigdokumentiert sein.Das gilt auch für Code,welchen sie aus fremden Quellen verwenden. 

## 2. Vorbereitung
Als Vorbereitung für die LB3, habe ich mich verschiedene Online-Kurse zu Docker auf Youtube angeschaut. Techworld with Nana ist eine sehr guter Kanal, um darin einzusteigen und sich eine Übersicht zu bilden. Dazu haben wir mehrere Einführungen von unserem Lehrer bekommen und dazu praktische Übungen gelöst. Dafür habe ich mich auf Docker Hub schlau gemacht und 
Als nächsten Schritt ging es zur Auswahl meines Services. Ich habe mich für ein Mediawiki entschieden die mit einer MariaDB verbunden wird. Ich hab mir dann mehr Informationsquellen gesucht.


## 3. Umsetzung
Zuerst habe ich mir die Installation vom Docker Desktop vorgenommen. Dann habe ich auf meinem Git-Repository einen "lb3" Ordner erstellt und darin mein docker-compose.yml File abgelegt. Nun ging es zur Hauptaufgabe und zwar dem Code. Auf diesen werde ich im Kapitel 4. detailliert eingehen. Der Code wurd auf dem docker-compose.yml File geschrieben. Der nächste Schritt war es auf GitBash in das Verzeichnis zu gehen und dort den Befehl **docker-compose up** einzugeben. Nach dem Ende vom Script konnte man sich durch die vergebene IP auf die Webübersicht von Mediawiki verbinden. Die Konfigurationen und Anbindung von MariaDB wurde manuell über das GUI erstellt.  
  
Auf dem Gui wählte man zuerst die Sprache. Dann kam der Schritt die Datenbank anzubinden. Dort musste man die zugewiesene IP eingeben, diese ist im Gitbash ersichtlich. Für das Login nimmt man den zuvor im Code definierten Wiki-Namen, Benutzername und Passwort. Danach kann man noch optionale Einstellungen treffen und kann dan abschliessen. Beim Abschlussscreen wird ein File für den Download zur Verfügung gestellt. Das ist ein Konfigurationsfile von den getätigten Einstellungen. Diese sollte unbedingt heruntergeladen werden. 

## 4. Code 
```version: '3'
services:
  mediawiki:
    container_name: wiki
    image: mediawiki
    restart: always
    ports:
      - 80:80
    links:
      - database
    volumes:
      - $HOME/volumes/mediawiki/images:/var/www/html/images
      # After initial setup, download LocalSettings.php to the same directory as
      # this yaml and uncomment the following line and use compose to restart
      # the mediawiki service
      #- $HOME/volumes/mediawiki/LocalSettings.php:/var/www/html/LocalSettings.php

  database:
    image: mariadb
    container_name: db
    restart: always
    ports:
        - 3306:3306
    environment:
      # @see https://phabricator.wikimedia.org/source/mediawiki/browse/master/includes/DefaultSettings.php
      MYSQL_RANDOM_ROOT_PASSWORD: 1
      MYSQL_DATABASE: m300_lb3
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin123
    volumes:
       - $HOME/volumes/mysql/data:/var/lib/mysql```

## 5. Reflexion
Bei der LB3 viel es mir viel leichter mit den Tools umzugehen, da wir durch Vagrant LB2 Erfahrungen gesammelt haben und wir noch tiefer die Themen angeschaut haben. Ich habe ebenfalls durch die LB2 gelernt, wie man das Layout des md-Files strukturieren kann und dadurch viel mir die Dokumentation wesentlich einfacher. Die Arbeit mit Docker hat mir sehr gut gefallen. Die Screenshots konnte ich irgendwie nicht einbinden und habe es deswegen in einem Ordner auf dem Repository abgelegt.

## 6. Quellenangaben
**Theorie** https://github.com/mbe99/M300  
**Layout GitHub** https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax  
**Images** https://hub.docker.com/  
**Code** https://www.mediawiki.org/wiki/Docker/Hub#Initial_docker-compose.yml  
