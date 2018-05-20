---
layout: post
title:  "Docker Knowlegebase"
date:   2018-05-13 19:41:05 +0000
categories: jekyll update
---

#Table of Content
1.  [Introduction](#introduction)
    1. [Basic Docker Commands](#basiccommands)
    2. [Container untersuchen](#inspect)
2.  [Was ist ein Container/Image](#whatiscontainer)
    1. [Layer](#layer)
3.  [Prüfung der Engine](#engine)
4.  [Netzwerk](#network)
5.  [Docker DNS](#dns)
6.  [Was ist ein Image](#image)
7.  [Docker Hub](#dockerhub)
8.  [Dockerfile](#dockerfile)
9.  [Persistant Data](#persistant_data)
    1. [Volumes](#volumes)
    2. [Bindmounts](#bindmounts)
10. [Docker-Compose](#docker-compose)
11. [Docker-Swarm](#swarm)
    1. [3-Tier-Service](#3tierservice)

<h2 id="introduction">Introduction</h2>
Der Aufbau der Docker Kommandozeile ist sehr stukturiert 

`docker command subcommand options`


<h3 id="basiccommands">Basic Docker Commands</h3>
```
docker container stop
docker image ls                              # zeigt mir alle heruntergeladenen Images
docker container ls -a
docker container rm
docker container log name
docker container exec -it database /bin/bash # gut zum debuggen im Container
```
Shell in einem Container öffnen
```
docker container run   - it <container> bash # to get a shell in a new Container
docker container exec  - it <container>      # Verbindung zum bestehenden Container aufnehmen
docker container start - ai <container>      # Einen bereits verwendeten Container wieder starten und verbinden
```
Wass passiert wenn wir einen Container starten?:

```
Docker container run --publish 80:80 image
1. Prüft intern im image Cache ob das Image schon vorliegt
2. Nächster Schritt im Docker Hub danach suchen
3. Lädt die angebene Version herunter(Default latest)
4. Stellt einen neuen Container basieren auf dem Image bereit
5. Gibt dem Container im privaten Netwerk der Container Engine eine IP
6. Öffnet Port 80 auf dem Host und leitet den Traffic auf den Port 80 im Container 
7. Startet Container indem es den Command im Image nutzt
```

<h3 id="inspect">Container untersuchen</h3>
Kommandos zum Untersuchen von Container
```
docker container top     - process list in one container
docker container inspect - details of one container config; zeigt Metainformation
docker container stats   - performance details of a container

```
<h2 id="whatiscontainer">Was ist ein Container/Image</h2>

Ein Image sind die App Binaries und Abhängigkeiten
Metainformation über das Image und wie es zu nutzen ist
--> Es ist kein Betriebssystem. Es fehlt der Kernel, die Module etc. Diese dinge kommen vom Host
* Ein Image ist eine Applikation welche wir starten möchten.
* Ein Container ist eine Instanz dieses Image welches einen Prozess startet
* Man kann beliebig viele Container eines Images starten
* Images werden in Registries gesichert, der Standard für Docker ist Docker HUB

<h3 id="Layer">Layers</h3>
Images bestehen aus Änderungen am Filesystem und den Metadaten.
Jeder Layer hat eine Eindeuting Sha-Signatur. Auf einem Pc wird jeder Layer nur einmal gesichert. So werden Dopplungen bei Images welche sich nur in ihrem Appdaten unterscheiden aber auf dem gleichen Image basieren nur 1x geladen

<h2 id="engine">Prüfung der Engine</h2>

{% highlight bash %}
docker version
# Prüft ob die CLI mit der Engine kommuinizieren kann
docker info
# zeigt die Einstellungen der Engine
{% endhighlight %}


<h2 id="network">Netzwerk</h2>

<h2 id="dockerhub">Docker Hub</h2>
DockerHub ist ei Repository für Docker Images. Ist ziemlich einfach zu nutzen. Repos müssen existieren bevor der Upload stattfindet.
Image ist immer via Nutzer/Image zu erreichen. Tags sind einfach nur Pointer welche auf eine Version zeigen.

Wenn ich einen Tag setzen will, und mein Image veröffentlichen möchte:
``` 

docker image tag nginx phpe/nginx  <-- ich hatte noch ein nginx Image was ich als Basis verwendet habe.

docker image push phpe/nginx --> lädt mein Image hoch
docker login                 --> falls der Nutzer nicht hinterlegt ist
docker logout                --> falls es eine geteileter Server ist
```

<h2 id="dockerfile">Dockerfile</h2>

Ein Dockerfile ist ein Textfile mit Anweisungen wie das Image durch die Dockerengine gebaut werden soll, welche Images als Basis dient etc. Bretfischer repo auf Dockerhub hat eine gut erklärtes Dockerfile, es ist auch in der Nextcloud hinterlegt.
Es gibt einige Do's and dont's bei Dockerfiles [best-practises][best-practises]
docker image bauen:

docker image build -f docker-file -t customnginx .

<h2 id="swarm">Docker Swarm</h2>
Swarm ist eine Cluster Lösung welche 2016 veröffentlicht wurde und in der Docker Engine bereits enthalten ist.

<h3 id="swarm-architektur">Swarm Architektur</h3>
Ähnlich wie andere Container Orchestrierer gitb es bei Swarm 2 verschieden Aufgaben. Das ist zum einen der Manager und zum anderen der Worker. Die spezifischen Aufgaben sind anhand der Namen zu erkennen.

![DockerSwarm-Architektur]({{ "/assets/Docker-Swarm-Architektur.png" | baseurl }})

Der oder die Manager teilen sich eine Datenbank (die Raft Datenbank) und kommunizieren mit den Workern. Die gesamte Kommunikation findet verschlüsselt statt. Sobald ein Cluster aufgesetzt wird legt dieser ein Root Certifikat an, welches für die Verschlüsselung verwendet wird.
![DockerSwarm-Detail-Architektur]({{ "/assets/swarm-mode-complex-architekture.png" | baseurl }})

<h3 id="swarm-services">Swarm Services</h3>

Bevor der erste Swarm Service aktiviert werden kann muss zuerst der Swarm initialisiert werden, dazu einfach ein:
`docker swarm init`

Im Anschluss stehen allen Swarm Features zur Verfügung.

<h4> Swarm Service erstellen</h4>

![Docker Service Architektur]({{ "/assets/Service-architektur.png" | baseurl }})

Ein Service besteht aus mehreren Tasks welche abgearbeitet werden. Diese Tasks werden in Container abgearbeitet welche auf den verfügbaren Nodes ausgeführt werden. Mittels des Replica-Sets gibt man an wie breit ein Service verfügbar sein soll.

Docker Service erstellen der den Google DNS pinged

`docker service create alpine ping 8.8.8.8`

Alle Service anzeigen:
```
root@Desktop-PC:~# docker service ls
ID                  NAME                   MODE                REPLICAS            IMAGE               PORTS
wvaxhgetfunn        musing_proskuriakova   replicated          1/1                 alpine:latest       
```
Detials zu einem speziellem Service ansehen (den Namen habe ich aus dem vorherigen Befehl bezogen):
```
root@Desktop-PC:~# docker service ps musing_proskuriakova
ID                  NAME                     IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
ad9a4exeqlpd        musing_proskuriakova.1   alpine:latest       Desktop-PC          Running             Running 2 minutes ago
```
<h4>Einen Service aktualisieren um zum Beispiel sein Replikaset zu erweitern</h4>

```
docker service update quirky_shtern --replicas 3
```

Docker Swarm Ports:
[Docker Swarm Ports][docker-swarm-ports]

<h4> Funktionsweise der Replikasets testen</h4>

Man kann mittels `docker container rm -f` einen dieser Container stoppen und dann mittels `docker service ps` beobachten wie er durch swarm einfach wieder neu erschaffen wird
Um das im Detail zu beobachten kan man mittels `watch -d docker service ps` sehr gut sehen wie das Replikaset wieder auf den Soll-Zusatnd aufgefüllt wird.

<h5> Einen Service wieder stoppen</h5>

Dadurch das die Container auf irgendwo laufen können und die Container immer wieder neu gestartet werden, muss en Service immer explizit entfernt werden, dies macht man auch über den Service Command: `docker service rm service-name`.

<h3 id="3tierservice">3-Tier-Service</h3>

Um die folgenden Schritte nachvollziehen zu können habe ich [play-with-docker][play-with-docker] verwendet. Man kann aber auch [docker-machine][docker-machine] verwenden(auf Windows und Mac mit dem Docker-Toolset bereits installiert), und auf diesen docker installieren. am besten mittels [get-docker][get.docker.com]




[docker-certification][docker-certification]

[docker-devops-prep-guide][devops-prep-guide]

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].


<h2 id="introduction">Introduction</h2>
[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
[best-practises]: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
[play-with-docker]: play-with-docker.Commands
[docker-machine]: https://docs.docker.com/machine/
[get.docker.com]: https://get.docker.com/
[docker-swarm-ports]:https://www.bretfisher.com/docker-swarm-firewall-ports/
[devops-prep-guide]: https://github.com/DevOps-Academy-Org/dca-prep-guide
[docker-certification]:https://www.bretfisher.com/docker-certified-associate/
