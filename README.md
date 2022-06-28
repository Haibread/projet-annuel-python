# Projet annuel : Développement autour de Python et Hashcat

## Présentation

Dans le cadre du projet annuel, j'ai fais le choix de déployer une application en Python permettant de lancer des processus de bruteforce Hashcat dans le cloud.

Le déploiement de la solution se déroulera en plusieurs étapes, mais il est relativement aisé grâce à la présence d'un fichier docker-compose.

Le résultat final permet ainsi de déployer, depuis une interface web, des instances hashcat sur Digital-Ocean

## Prérequis

### Prérequis logiciels

Afin de pouvoir lancer le logiciel, il faut que Docker et Docker compose soient installés sur le serveur.

Vous trouverez la documentation d'installation de Docker à [à cette adresse](https://docs.docker.com/get-docker/), ainsi que la documentation d'installation de Docker compose [à cette adresse](https://docs.docker.com/compose/install/).

### Récupération des identifiants nécessaires

Une fois les différents logiciels installés, il est nécessaire de récupérer son token Digital Ocean afin de pouvoir démarrer les instances dans le cloud.

La procédure de récupération du token est disponible [à cette adresse](https://docs.digitalocean.com/reference/api/create-personal-access-token/).

## Mise en place

Maintenant que les étapes de prérequis sont complétées, on peut commencer le partie déploiement.

### Configuration de nginx

Le premier outil à configurer est nginx.

Nginx nous sert de reverse proxy pour l'application, ainsi que pour la partie monitoring avec Prometheus, Loki et Grafana.

Le fichier `monitoring.domain.nginx` est disponible sur le repository, et après avoir modifié tous les champs `server_name`, `ssl_certificate_*` et `proxy_pass`, votre configuration nginx devrait être prête !

### Démarrage de l'application

Pour démarrer l'application, on commence d'abord par cloner le repository sur le serveur grâce à la commande `git clone git@github.com:Haibread/projet-annuel-python.git`.

Ensuite, il faut créer un fichier vide à l'emplacement prévue, car docker crée un dossier par défaut. Sous Linux, on peut exécuter la commande `mkdir web && touch web/db.sqlite` à la racine du projet.

Avant de créer les containeurs, il faut modifier le fichier `docker-compose.yml` pour remplir les variables d'environnement pour les containeurs `web` et `grafana`.

Une fois ces modifications effectuées, on peut exécuter la commande `docker compose up -d` pour démarrer le projet.

### Accès aux différentes consoles web

La console web du projet sera disponible à l'adresse fournie dans le `docker-compose.yml`, avec les identifiants fournis.

Dans mon cas, l'application est disponible à l'adresse `https://monitoring.basget.io/`.

Une interface web Grafana est aussi disponible dans le sous-menu `/grafana/`.

Ainsi dans mon cas, elle est disponible à l'adresse `https://monitoring.basget.io/grafana`.
