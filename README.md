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
 vous devez utiliser la commande 
 docker exec -ti ID_RETOURNÉ_LORS_DU_DOCKER_RUN bash  . Dans cette commande, 
l'argument -ti permet d'avoir un shell bash pleinement opérationnel. Une fois que vous êtes dans votre conteneur, 
vous pouvez vous rendre, via la commande cd /usr/share/nginx/html  , dans le répertoire où se trouve le fichier index.html , 
pour modifier son contenu et voir le résultat en direct à l'adresse http://127.0.0.1:8080
docker stop CONTAINER_ID: pour stopper un conteneur
 docker rm ID_RETOURNÉ_LORS_DU_DOCKER_RUN: Celle-ci va détruire le conteneur et son contenu ;
docker system prune: faire le menage
*********************************************************************************************************
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
***********************************************************************************************
docker tag image_name:latest YOUR_USERNAME/image_name:latest  . 
Celle-ci va créer un lien entre notre image ocr-docker-build:latest créée précédemment et l'image que nous voulons envoyer sur le Docker Hub YOUR_USERNAME/ocr-docker-build:latest
Si le conteneur que vous utilisez n'a pas de nom, utilisez son id de conteneur, que vous pouvez récupérer en retour de la commande  docker build  .
➜ docker tag id_du_conteneur openclassrooms/ocr-docker-build:
Vous pouvez maintenant exécuter la dernière commande nécessaire pour envoyer votre image vers le Docker Hub. Pour cela, vous allez exécuter la commande docker push YOUR_USERNAME/ocr-docker-build:latest
*************************************************************************************************
chercher une image docker search image_name
**************************************************************************************************
Voici une manière fonctionnelle d'écrire votre Dockerfile afin de créer une image démarrant de  openclassrooms/build_image  :

FROM openclassrooms/build_image

RUN apt-get update \
&& apt-get upgrade -y \
&& apt-get install nginx -y
Ensuite, construisez l'image grâce à la commande  docker build -t image_perso . 
. Puis, lancez le conteneur avec  docker run image_perso
************************************************************************************************
récupérer l'ensemble des images décrites dans votre fichier docker-compose.yml et les télécharger depuis le Docker Hub, vous devez faire un docker-compose pull  . Du côté de Docker, la commande serait un docker pull
Démarrer une stack Docker Compose
Si vous souhaitez lancer la création de l'ensemble des conteneurs, vous devez lancer la commande docker-compose up (pour rappel, vous faites un docker run pour lancer un seul conteneur). Vous pouvez ajouter l’argument -d pour faire tourner les conteneurs en tâche de fond.

Voir le statut d'une stack Docker Compose
Après avoir démarré une stack Docker Compose, vous aurez certainement besoin de voir si l'ensemble des conteneurs sont bien dans un état fonctionnel, et prêts à rendre un service.

Pour cela, vous allez utiliser la commande docker-compose ps qui vous affichera le retour suivant

Votre stack Docker Compose est maintenant fonctionnelle, et l'ensemble des services répondent bien ; mais vous pourriez avoir besoin de voir les logs de vos conteneurs. Pour cela, vous devez utiliser la commande docker-compose logs -f --tail 5
Celle-ci permet de voir l'ensemble des logs sur les différents conteneurs de façon continue, tout en limitant l'affichage aux 5 premières lignes

Si vous souhaitez arrêter une stack Docker Compose, vous devez utiliser la commande docker-compose stop  . Cependant, celle-ci ne supprimera pas les différentes ressources créées par votre stack.

Ainsi, si vous lancez à nouveau un docker-compose up -d  , l'ensemble de votre stack sera tout de suite à nouveau fonctionnel

Si vous souhaitez supprimer l'ensemble de la stack Docker Compose, vous devez utiliser la commande docker-compose down qui détruira l'ensemble des ressources créées.

Valider une stack Docker Compose
Lors de l'écriture d'un fichier docker-compose, nous ne sommes pas à l’abri d'une erreur. Pour éviter au maximum cela, vous devez utiliser la commande docker-compose config qui vous permettra de valider la syntaxe de votre fichier, et ainsi d'être certain de son bon fonctionnement

docker-compose up -d vous permettra de démarrer l'ensemble des conteneurs en arrière-plan ;

docker-compose ps vous permettra de voir le statut de l'ensemble de votre stack ;

docker-compose logs -f --tail 5 vous permettra d'afficher les logs de votre stack ;

docker-compose stop vous permettra d'arrêter l'ensemble des services d'une stack ;

docker-compose down vous permettra de détruire l'ensemble des ressources d'une stack ;

docker-compose config vous permettra de valider la syntaxe de votre fichier docker-compose.yml  .