Volumes
=======

Les volumes sont des espaces de stockage gérés par Docker.
Ils vont vous permettre de :
* Sauvegarder des données indépendantes de votre conteneur, si celui-ci disparait ou évolue, les données stockées dans le volume persistent
* Partager des données entre plusieurs conteneurs


#### Créer un volume
```bash
$ docker volume create mon-volume
```

#### Utiliser un volume sur un conteneur
```bash
$ docker run --volume mon-volume:/var/www
```

#### Supprimer un volume
```bash
$ docker volume rm mon-volume
```

[[En savoir plus]](https://docs.docker.com/storage/volumes/)