# Domo1
Domotique node

Le module de contrôle du système est un Raspberry Pi modèle 3 B. Il est alimenté par une alimentation externe 5v 3A.

## Installation du RaspberryPi
Réaliser un un update et un upgrade
```
sudo apt-get update && sudo apt-get upgrade -y
```

Installer Docker
```
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $USER
sudo reboot
```

Installer git
```
sudo apt install git -y
```

Cloner le repo git
```
git clone https://github.com/gregoryvanko/Domo1.git Docker
```

Créer et exécuter le container
```
cd Docker
docker compose up -d
```

## Mesure du port P1 du compteur d'energie
Pour récupérer les données du port P1, il faut s'y connecter via un port USB du Raspberry Pi et un cable USB RJ45.
Le port USB a utiliser doit etre configuré dans le fichier docker-compose.yml
```
- P1READER_PORT=/dev/ttyUSB0
```

## Mesure de l'onduleur
Pour récupérer les données de l'onduleur, il faut s'y connecter via un connextion modbus over tcp sur le port 502.
L'adresse IP de l'onduleur doit etre configurée dans le fichier docker-compose.yml
```
- ONDULEUR_IP=192.168.30.8
```

## Sauver les données dans une DB InfluxDB
Toutes les donnes sont envoyées toutes les 15 minutes dans une DB InfluxDb.
Il faut configurer la connexion vers la DB dans le fichier .env à sauver sur le Raspberry PI.
```
INFLUX_TOKEN=VKo5S05Tx6v7mJ-D0d...
INFLUX_URL=https://...
INFLUX_ORGA=...
INFLUX_BUCKET=...
```

## CloudFlared
Pour se connecter au Raspberry Pi depuis internet on va créer un tunnel via CloudFlare.

Il faut configurer un tunnel dans Claudflare et sauver le token communiqué par ClaudFlare dans le fichier .env à sauver sur le Raspberry PI.
```
CLOUDFLARED_TOKEN=djyugricbd...
```

## Le fichier .env a créer sur le Raspberry Pi:
```
CLOUDFLARED_TOKEN=djyugricbd...
INFLUX_TOKEN=VKo5S05Tx6v7mJ-D0d...
INFLUX_URL=https://...
INFLUX_ORGA=...
INFLUX_BUCKET=...
```

# Update
Ajouter le package NodeRed à installer dans le fichier NodeRed/Configuration/dockerfile en y copiant la ligne suivante: 
```
RUN npm install xxx
```
Copier le nouveau fichier flows.json sous NodeRed/Data
