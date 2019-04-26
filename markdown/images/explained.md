Images
==========================
1. Créer une image
```dockerfile
FROM busybox 
...
```

Dans votre Dockerfile, l'instruction **FROM** va vous permettre de définir sur quelle image vous souhaitez vous baser afin d'y ajouter de nouvelles couches (layers)
> Il en existe une multitude sur [Docker Hub](https://hub.docker.com)

```dockerfile
FROM busybox

ARG PHP_VERSION=7.2

RUN curl -OsL http://monurl/php-${PHP_VERSION}.tar.gz
```
L'instruction **ARG** va vous permettre de pouvoir utiliser des arguments de construction, pour, par exemple construire votre conteneur avec une version spécifique d'un programme (PHP ici)

Pour cela, rien de plus simple, dans le dossier de votre Dockerfile, executez une commande semblable a celle-ci
```sh
$ docker build -t ma-super-image --build-arg PHP_VERSION=7.3 .
```

```dockerfile
FROM busybox

USER toto
```
L'instruction **USER** va vous permettre de définir l'utilisateur utilisé par défaut pour executer les instructions effectuées lors de la construction, ce même utilisateur sera celui utilisé lors de l'execution de votre conteneur

```dockerfile
FROM busybox

RUN echo hello
```
L'instruction **RUN** va vous permettre d'éxecuter une action dans le conteneur lors de sa construction

```dockerfile
...
ADD crontab /etc/crontab
...
```
L'instruction **ADD** va vous permettre de copier un fichier présent dans le dossier courant a l'interieur de votre conteneur lors de la construction de celui-ci, ce qui, permettra de pouvoir y accéder lors de l'éxecution de votre conteneur
Il existe une instruction qui effectue le même travail : **COPY**
L'instruction **ADD** à cependant un avantage par rapport à **COPY**, elle accepte une url en tant que source :
```dockerfile
...
ADD https://path/to/file /opt/file
...
```
A vous de choisir quelle commande répond le mieux a votre besoin !

```dockerfile
...
ENV HTTPS_PROXY http://my.proxy.url
...
```
L'instruction **ENV** va vous permettre de définir une variable d'environnement au sein de votre conteneur qui sera disponible lors de son execution

```dockerfile
...
VOLUME ["/var/log/apache/"]
...
```
L'instruction **VOLUME** va vous permettre de créer un volume sur le chemin specifié entre les crochets.

[Volumes](volumes/explained.md)

```dockerfile
...
EXPOSE 8080
...
```
L'instruction **EXPOSE** va vous permettre de pouvoir exposer les ports de votre container afin de les rendre accessibles depuis l'hote.
Grâce a cette instruction, je pourrais accéder au serveur web (s'il existe) depuis mon navigateur via l'adresse http://127.0.0.1:8080

```dockerfile
...
ENTRYPOINT ["/usr/bin/apache"]
...
```
L'instruction **ENTRYPOINT** va définir le point d'entrée de votre conteneur, si cette instruction est définie, lors de l'utilisation d'un **docker run** ou **docker-compose up** le processus qui va maintenir votre conteneur en vie, sera celui défini dans cette instruction.

```dockerfile
...
ENTRYPOINT ["/usr/bin/php"]
CMD ["--help"]
...
```
L'instruction **CMD** peut avoir deux utilités :
* Couplée avec **ENTRYPOINT** elle définira les arguments qui seront passés a cet entrypoint lors du lancement de votre conteneur, si vous définissez des arguments après le nom de votre image lors d'une execution `docker run mon-super-conteneur --mon-arg`, cet argument écrasera celui ou ceux qui ont été préalablement définis dans votre Dockerfile
* Sans entrypoint, elle définira la commande éxecutée par défaut au démarrage de votre conteneur, et, pourra être écrasée par une autre commande que vous passerez en parametre après le nom de votre image : `docker run mon-super-conteneur sh`

2. [Récuperation d'une image](#recuperation-dune-image)

Recuperation d'une image
--------------
````sh
$ docker pull hello-world

# Résultat
Using default tag: latest
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest
````

[[En savoir plus]](https://docs.docker.com/engine/reference/builder/)