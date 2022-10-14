docker images: liste les images qui sont sur ta machine
docker images -a
docker ps:liste les conteneurs qui sont sur ta machine
docker pull image_name: telecharger une image
docker run -it image_name: tourner le conteur d'une image -it pour que ça soit interactif
docker run -it -d image_name: tourner le conteur d'une image de maniere detacher
docker run -d -p 8080:80 nginx
-p pour définir l'utilisation de ports. Dans notre cas, nous lui avons demandé de transférer le trafic du port 8080 vers le port 80 du conteneur.
 Ainsi, en vous rendant sur l'adresse  http://127.0.0.1:8080  , vous aurez la page par défaut de Nginx
-d 	pour détacher le conteneur du processus principal de la console. 
	Il vous permet de continuer à utiliser la console pendant que votre conteneur tourne sur un autre processus ;
Vous pourriez aussi avoir besoin de "rentrer" dans votre conteneur Docker pour pouvoir y effectuer des actions. Pour cela,
 vous devez utiliser la commande docker exec -ti ID_RETOURNÉ_LORS_DU_DOCKER_RUN bash  . Dans cette commande, 
l'argument -ti permet d'avoir un shell bash pleinement opérationnel. Une fois que vous êtes dans votre conteneur, 
vous pouvez vous rendre, via la commande cd /usr/share/nginx/html  , dans le répertoire où se trouve le fichier index.html , 
pour modifier son contenu et voir le résultat en direct à l'adresse http://127.0.0.1:8080
docker stop CONTAINER_ID: pour stopper un conteneur
 docker rm ID_RETOURNÉ_LORS_DU_DOCKER_RUN: Celle-ci va détruire le conteneur et son contenu ;
docker system prune: faire le menage



















La première chose que vous devez faire est de créer un fichier nommé "Dockerfile", puis de définir dans celui-ci l'image que vous allez utiliser comme base, grâce à l'instruction FROM  . Dans notre cas, nous allons utiliser une image de base Debian 9.
L'instruction FROM n'est utilisable qu'une seule fois dans un Dockerfile.
Ensuite, utilisez l'instruction RUN pour exécuter une commande dans votre conteneur.
Puis, utilisez l'instruction  ADD  afin de copier ou de télécharger des fichiers dans l'image. Dans notre cas, nous l'utilisons pour ajouter les sources de notre application locale dans le dossier /app/ de l'image.
Utilisez ensuite l'instruction WORKDIR qui permet de modifier le répertoire courant. La commande est équivalente à une commande cd en ligne de commande. L'ensemble des commandes qui suivront seront toutes exécutées depuis le répertoire défini.
Puis, l'instruction RUN suivante permet d'installer le package du projet Node.js.
L'instruction EXPOSE permet d'indiquer le port sur lequel votre application écoute.
 L'instruction VOLUME permet d'indiquer quel répertoire vous voulez partager avec votre host.
Vous pouvez maintenant créer votre première image Docker !
L'argument -t permet de donner un nom à votre image Docker. Cela permet de retrouver plus facilement votre image par la suite.

Le . est le répertoire où se trouve le Dockerfile ; dans notre cas, à la racine de notre projet.
docker run -d -p 2368:2368 ocr-docker-build
En résumé
Pour créer une image Docker, vous savez utiliser les instructions suivantes :

FROM qui vous permet de définir l'image source ;

RUN qui vous permet d’exécuter des commandes dans votre conteneur ;

ADD qui vous permet d'ajouter des fichiers dans votre conteneur ;

WORKDIR qui vous permet de définir votre répertoire de travail ;

EXPOSE qui permet de définir les ports d'écoute par défaut ;

VOLUME qui permet de définir les volumes utilisables ;

CMD qui permet de définir la commande par défaut lors de l’exécution de vos conteneurs Docker.