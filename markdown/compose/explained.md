Docker-compose
==============

L'outil `docker-compose` va vous permettre de pouvoir orchestrer le lancement de plusieurs conteneurs en définissant leurs dépendances

```yaml
version: '3'
services:
  web:
    build: .
    ports:
    - "8080:8080"
    volumes:
    - .:/app
    - logs:/var/log
    links:
    - mysql
  mysql:
    image: mysql
volumes:
  logs:
  ```

* `version` définit la version de votre fichier docker-compose
* `services` définit tous les services que docker-compose va gérer
    * `web` est le nom de votre service
        * `build` définit le chemin vers le Dockerfile pour le service `web`
        * `image` définit l'image sur laquelle vous vous basez pour votre service ()
        * `ports` définit la liste des ports exposé pour le service
        * `volumes` définit la liste des volumes disponibles dans votre service
        * `links` (déprécié) permet de définir les liens entre les différents services (vous pourrez ainsi contacter le conteneur cible grace a son nom défini dans ce fichier)
* `volumes` définit les volumes qui vont exister dans votre environnement orchestré
    * `logs` est le nom du volume qui sera crée par docker-compose


[[En savoir plus]](https://docs.docker.com/compose/)