# Docker

D'abord:
-  [Containers](#containers)
-  [Images et Dockerfile](#images-et-dockerfile)

Ensuite, nous traiterons deux autres sujets :
- [Volumes](#volumes)
- [Dockerfile](#dockerfile)
- [Docker-compose](#docker-compose)

Containers
--------------------
> Execute une image dans un conteneur
```sh
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
````

[[En savoir plus]](markdown/containers/explained.md)


Images et Dockerfile
--------------------
> Une image, c\'est un environnement prédéfini prêt a  l\'emploi

Récuperer une image
```sh
$ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest
```

<details>
    <summary>Node</summary>

```dockerfile
# Utilisation de l'image php officielle (tout son environnement nous est disponible)
FROM node

# Le dossier dans lequel les commandes s'executeront
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Creation du fichier node qui sera executé 
RUN echo 'console.log(process.env.MESSAGE); if (process.argv.length > 2) { console.log(process.argv[2]); }' > run.js

# Nous définissons une variable d'environnement qui sera constamment utilisée
ENV MESSAGE Docker is working

# L'application sera executée dès que nous démarrerons le container
ENTRYPOINT ["node", "run.js"]
```

> Dans le dossier contenant votre fichier `Dockerfile`
```sh
$ docker build -t docker-run .

# Puis
$ docker run docker-run
Docker is working

# Avec un paramètre
$ docker run docker-run test
Docker is working
test
```

</details>

<details>
    <summary>PHP</summary>

```dockerfile
# Utilisation de l'image php officielle (tout son environnement nous est disponible)
FROM php

# Le dossier dans lequel les commandes s'executeront
WORKDIR /app

# Le contenu du dossier en cours sera copié dans le dossier /app du container
COPY . /app

# Install any needed packages specified in requirements.txt
RUN echo '<? echo $_ENV["MESSAGE"] . " " . $argv[1]; ?>' > run.php

# Nous définissons une variable d'environnement
ENV MESSAGE Docker is working

# L'application sera executée dès que nous démarrerons le container
CMD ["php", "run.php"]
```

> Dans le dossier contenant votre fichier `Dockerfile`
```sh
$ docker build -t docker-run .

# Puis
$ docker run docker-run
Docker is working

# Avec un paramètre
$ docker run docker-run test
Docker is working test
```

</details>


[[En savoir plus]](markdown/images/explained.md)

Volumes
--------------------
> Créer un volume (nommé **data**) détaché du container et de votre système hôte
```sh
$ docker volume create data
data
```

> Utiliser le volume précedemment crée sur un volume
```sh
$ docker run -v data:/var/data busybox ls /data
```
[[En savoir plus]](markdown/volumes/explained.md)

Docker-compose
--------------------

```yaml
version: '3'
services:
    store:
        image: busybox
        working_dir: /tmp        
        command: "touch Docker && touch is && touch working"
        volumes:
            - data:/tmp
    server:
        image: php
        working_dir: /tmp
        ports:
            - "8000:8000"
        volumes:
            - data:/tmp
        command: ["print '<? var_dump(scandir(__DIR__)); ?>' > index.php", "&&", "php -S 0.0.0.0:8000"]

volumes:
    data:
```
[[En savoir plus]](markdown/compose/explained.md)