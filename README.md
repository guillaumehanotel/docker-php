# Images Docker PHP

Ce dépôt contient des images Docker pour les environnements de développement et de production PHP.

## Versions PHP disponibles

- PHP 8.2 (FPM)
- PHP 8.3 (FPM)
- PHP 8.4 (FPM)

Chaque version dispose de variantes pour le développement et la production.

## Structure des images

- **Base** : PHP-FPM avec les extensions essentielles
- **Production** : Optimisée pour les environnements de production
- **Développement** : Basée sur l'image de production avec des outils supplémentaires (XDebug)

## Reconstruction et publication des images

Lorsque vous devez mettre à jour une image (par exemple, ajouter des extensions ou modifier des configurations), suivez ces étapes :

### Étape 1 : Construire l'image de production versionnée

```bash
# Naviguer vers le répertoire de production pour la version PHP
cd 8.2/production

# Construire avec un tag de version spécifique
docker build -t guillaumehanotel/php:8.2-prod-1.0.3 .
```

### Étape 2 : Pousser l'image de production versionnée

```bash
# Se connecter à Docker Hub (si ce n'est pas déjà fait)
docker login

# Pousser l'image versionnée
docker push guillaumehanotel/php:8.2-prod-1.0.3
```

### Étape 3 : Construire et pousser l'image 'latest' de production

```bash
# Construire le tag latest
docker build -t guillaumehanotel/php:8.2-prod-latest .

# Pousser le tag latest
docker push guillaumehanotel/php:8.2-prod-latest
```

### Étape 4 : Reconstruire l'image de développement

Comme l'image de développement est basée sur l'image de production, vous devez la reconstruire après avoir mis à jour l'image de production :

```bash
# Naviguer vers le répertoire de développement
cd ../dev

# Construire avec un tag de version spécifique
docker build -t guillaumehanotel/php:8.2-dev-1.0.3 .

# Pousser l'image versionnée
docker push guillaumehanotel/php:8.2-dev-1.0.3

# Construire et pousser le tag latest
docker build -t guillaumehanotel/php:8.2-dev-latest .
docker push guillaumehanotel/php:8.2-dev-latest
```

Remplacez `8.2` par la version PHP souhaitée et `1.0.3` par votre numéro de version sémantique.

## Outils inclus

- nano
- ffmpeg
- git
- supervisor
- wget
- sudo
- default-mysql-client
- zsh
- cron

## Extensions PHP

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
- xdebug (images dev uniquement)
- imagick
- opcache

## Exemple d'utilisation

Pour utiliser ces images dans vos projets :

```yaml
# exemple de docker-compose.yml
services:
  php:
    image: guillaumehanotel/php:8.4-prod-latest
    volumes:
      - ./:/var/www/html
    working_dir: /var/www/html
```

Pour les environnements de développement :

```yaml
# exemple de docker-compose.yml
services:
  php:
    image: guillaumehanotel/php:8.4-dev-latest
    volumes:
      - ./:/var/www/html
    working_dir: /var/www/html
```
