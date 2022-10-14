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