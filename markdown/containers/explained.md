Containers
==========================
### Pourquoi ?
Créer un environnement

Rapidement installer un ensemble d'applications pour mettre en place un site ou un système complet. 
Il est aussi possible d'en créer, et de les compartimenter afin de ne pas influer sur le système hôte

L'isolation est le mot clé, il permet de garder votre système stable

### Docker
En fait Docker, c'est un linux embarqué :
Docker virtualise un kernel linux qui permet de créer des conteneur nommés LXC <- Avant version 1.8, après utilisation du `libcontainer`

Les containers sont des environnements indépendants qui vont venir utiliser des ressources de l'hôte (mémoires, processeurs, disques)
1. [Executer un container](#executer-un-container)
2. [Execution avancée d'un container](#execution-avancee-dun-container)
3. [Interagir avec un container](#interagir-avec-un-container)

Executer un container
--------------
````sh
$ docker run busybox
````

Execution avancée d'un container
--------------
````sh
docker run -dit --name docker-project-apache -p 8080:80 -v ./examples/httpd:/usr/local/apache2/htdocs/ httpd:2.4
````

Accédez a l'url http://localhost:8080

Analyse de la commande permettant de lancer un container
--------------
````sh
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
````
----
Avec `docker run [OPTIONS]` nous allons pouvoir surcharger le comportement d'une image afin qu'elle corresponde a nos besoins

````sh
$ docker run -p 80:80 busybox
````
Le flag `-p` nous permet de pouvoir exposer les ports de notre container vers la machine hôte, ainsi que vers l'extérieur

````sh
$ docker run -d busybox
````
Le flag `-d` nous permet de pouvoir démarrer notre container en mode détaché, il n'interagira pas avec le terminal en cours d'utilisation, sa sortie standard ne sera pas affichée

````sh
$ docker run -it busybox
````
* L'option `-i` permet de pouvoir dialoguer avec le container, sans cela, tout ce qui sera tapé après la commande ne sera pas pris en compte

* L'option `-t` permet d'attacher un pseudo-tty au container, ce qui, permettra au container de ne pas terminer tant que nous ne l'avons pas décidé (mais l'option `-i` est nécessaire afin de dialoguer avec ce pseudo-tty)

````sh
docker run -dit --name my-apache-app -p 8080:80 -v ${PWD}/examples/:/usr/local/apache2/htdocs/ httpd
````

Le flag `-v` permet de monter un volume sur un container afin de pouvoir synchroniser vos fichiers vers celui-ci

----

Avec `docker run IMAGE[:TAG|@DIGEST]` définir quelle image nous souhaitons exécuter, nous pourrons aller jusqu'a préciser sa version

````sh
$ docker run busybox:latest
# ou
$ docker run busybox:1.30
````
----
Avec `docker run IMAGE [COMMAND] [ARG...]` nous allons pouvoir exécuter des commandes dans notre container instancié

````sh
$ docker run busybox echo lol

# Résultat
lol
````

Interagir avec un container
==========================
````sh
$ docker exec [OPTIONS] CONTAINER [COMMAND]
````

Par exemple, un conteneur précedemment éxecuté est en vie avec un processus principal Apache, seulement je souhaite pouvoir accéder au système de fichiers de celui-ci afin de vérifier son bon fonctionnement, je vais exécuter un shell
```sh
$ docker exec -it mon-container-apache sh
```

[[En savoir plus]](https://docs.docker.com/v17.12/engine/reference/commandline/exec/)
