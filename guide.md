Guide d’installation du site web de VirtuoWorks 

I/ INTRODUCTION
Ce site web a pour finalité de créer, modifier, consulter et gérer des offres et des contrats de maintenance et de réalisation. Drupal est un cms (Content Management System) permettant la gestion de contrats.  
L’architecture de l’application repose sur Lando (qui marche avec Docker), Docker (qui marche avec WSL 2), PHP, MySQL, Apache et une distribution Linux sur Windows (Ubuntu 22.04.1 LTS). L’utilisation de cette distribution Linux est fortement conseillée car installer les dépendances sur un terminal Windows peuvent s’avérer être compliqué.
Lando est environnement de développement local open-source et un outil de développement d’application construit sur Docker. Ici, lando offre un environnement de travail déjà prêt pour Drupal.
Ce guide regroupe toutes les étapes pour installer les dépendances nécessaires au bon fonctionnement de l’application.
II/ Installation
1-	Télécharger une distribution Linux (par exemple Ubuntu 22.04.1 LTS) à partir de Microsoft Store.
2-	Installer et configurer Linux sur votre machine. (Créer un compte utilisateur et racine)
3-	Dans l’explorateur de fichiers, cliquez sur « Réseaux », cliquer sur le petit message jaune en haut de la fenêtre, et écrire à la place de « Réseaux » \\wsl.localhost\ si vous êtes sur Windows 11 ou sinon \\wsl$\ sur Windows 10. Faire entrer. Faire Clic droit sur le disque dur correspondant à la distribution de Linux installée et cliquer sur « Connecter un lecteur réseau ». Ainsi, le disque dur apparait dans la liste de vos autres disques durs locaux.
4-	Ouvrir un terminal Ubuntu.
5-	Cloner le dépôt de source. Un nouveau répertoire contenant ce dépôt apparait « virtuocm ».
6-	Toujours dans le répertoire, /home/user, installer Docker avec les commandes suivantes :
a.	curl -fsSL https://get.docker -o get-docker.sh
b.	sh get-docker.sh
7-	 Vérifier l’installation docker avec « docker –version ». Si le terminal répond avec la version du docker installée, cela veut dire que Docker a bien été installée.
8-	Installer Lando avec les commandes suivantes : 
a.	Wget https://files.lando.dev?installer/lando-x64-stable.deb 
b.	sudo dpkg -i lando-x64-stable.deb
9-	Vérifier l’installation de lando avec « lando –version ».
10-	Se positionner dans le répertoire virtuocm (dépôt de code source), dans le répertoire dump, extraire la base de données. Et remplacer les valeurs « utf8mb4_900_ai_ci » par « utf8mb4_general_ci » et le sauvegarder dans le répertoire dump.
11-	Dans le répertoire virtuocm en tant qu’utilisateur (pas en root), initialiser un nouveau projet Drupal avec Lando : « lando init », cliquez sur current directory working, puis sélectionner drupal9, puis écrire  ./web et enfin, donner un nom à votre projet. Prendre en note l’adresse du site web donnée sur le terminal (exemple : http://<nom_app>.lndo.site).
12-	Faire lando start pour démarrer les conteneurs de Docker.
13-	Vérifier que les conteneurs ont bien démarré : « sudo service docker status ». Le terminal doit répondre que docker est en train de tourner sinon le faire démarrer à la main « sudo service docker start ». Si après avoir fait lando start, le docker ne tourne toujours pas, faire : 
« sudo update-alternatives --set iptables /usr/sbin/iptables-legacy » et « sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy »
Vérifier que le Docker est en marche avec les commandes : sudo service docker status. Cela doit être le cas.
14-	Remplacer le code source de votre Drupal9 par celui du projet. Il y en a 2 à remplacer : « settings.php » et « settings.local.php ». Ces deux fichiers se trouvent dans virtuocm/web/site/default. 
15-	Faire « lando composer install » pour installer les dépendances de Drupal.
16-	Toujours dans virtuocm, Importer la base de données avec « lando db-import dump/<nom_base_de_données>.sql      . Le terminal répond « Import complete ».
17-	Aller sur un navigateur, écrire l’adresse du site web communiquée lors de la création de votre projet, cliquer sur entrer. Vous êtes sur le site de VirtuoWorks. 
18-	Appuyez sur « se connecter » et connecter - vous avec vos identifiants sur le site.
19-	Remarques : A chaque fois que vous voudriez utiliser le site, il faudra ouvrir le terminal de Linux sur lequel le Drupal du site est hébergée, puis se positionner dans virtuocm, et faire un lando start. Quand vous avez fini d’utiliser le site, faire un lando stop.


III/ Sources 
•	Documentation sur Docker : https://get.docker.com
•	Documentation sur Lando : https://docs.lando.dev/
•	Bug informatique avec WSL : https://github.com/microsoft/WSL/discussions/4872
