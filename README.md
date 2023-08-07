# docker-php

## php :  
Base : [PHP 8, FPM] + PHP Extensions (Redis, ODBC, ...) + Python & Pandas
Il y a une version prod et une version dev :
La version dev se base sur la php:prod-latest et rajoute XDebug.

Mettre à jour les images :
Si par exemple vous voulez modifier le Dockerfile de prod :

Une fois les modifications faites, il y a 4 étapes à faire :

rebuild l'image en précisant le tag de la nouvelle version mis à jour :  
```
docker build -t guillaumehanotel/php:8.2-prod-1.0.3 .
```

push l'image sur le hub : (docker login à faire au préalable)   
```
docker push guillaumehanotel/php:8.2-prod-1.0.3
```

rebuild l'image en précisant cette fois que c'est la latest version :  
```
docker build -t guillaumehanotel/php:8.2-prod-latest .
```
push l'image latest sur le hub :  
```
docker push guillaumehanotel/php:8.2-prod-latest
```
Et oui, il faut rebuild la dev puisque celle-ci se base sur prod, il faut aussi refaire la manip pour dev :)

## Outils

- nano
- ffmpeg
- git
- supervisor
- wget
- sudo
- default-mysql-client
- zsh
- cron


## Extensions

- zip
- pdo
- pdo_pgsql
- pdo_mysql
- curl
- imap
- gd
- intl
- dom
- redis
- xdebug
- imagick
- opcache