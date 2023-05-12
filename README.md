# TP-OS

- [TP-OS](#tp-os)
- [Introduction](#introduction)
- [](#)


# 1. Introduction 

Le présent document traite de l'installation et de la configuration d'un système de gestion de contenu (CMS) pour l'entreprise fictive TechnoGenix. TechnoGenix est une entreprise spécialisée dans le développement de solutions technologiques innovantes et la fourniture de services d'expertise dans le domaine de l'informatique. Ses clients sont principalement des entreprises du secteur des technologies de l'information et de la communication (TIC), des organismes publics et des institutions éducatives.

Le service informatique de TechnoGenix a identifié le besoin de créer un site vitrine sur Internet pour promouvoir les produits et services de l'entreprise. Pour répondre à ce besoin, le service informatique a choisi de déployer un CMS, qui permettra aux membres de l'équipe de communication de gérer facilement le contenu du site web sans avoir besoin de compétences techniques avancées. De plus, le service informatique souhaite que le CMS soit basé sur des technologies maîtrisées par l'équipe, telles que Debian ou Ubuntu, Apache ou Nginx, MySQL / MariaDB ou PostgreSQL.

Ce document décrit en détail la procédure d'installation et de configuration du CMS, ainsi que les exigences techniques et fonctionnelles qui doivent être respectées. Il servira de guide pour les personnes chargées de l'installation et de la maintenance du CMS, ainsi que pour les membres du service communication et du service informatique qui souhaitent comprendre les choix techniques réalisés et les étapes suivies pour mettre en place le système.


# 2. Choix des technologies

Choix des technologies et argumentation

Afin de mettre en place un système de gestion de contenu adapté aux besoins de l'entreprise TechnoGenix, plusieurs choix technologiques ont été effectués. Dans cette section, nous présentons ces choix et expliquons les raisons qui ont motivé ces décisions.

1. CMS : Wordpress a été choisi comme CMS pour le site vitrine de TechnoGenix en raison de sa popularité et de son caractère open-source. En effet, Wordpress est largement utilisé dans l'industrie, ce qui garantit un support communautaire important et une multitude de plugins disponibles pour étendre ses fonctionnalités. De plus, son caractère open-source permet une personnalisation et une adaptation aux besoins spécifiques de l'entreprise.

2. Base de données : MySQL a été sélectionné comme système de gestion de base de données (SGBD) pour le CMS. Bien que MariaDB aurait également pu être utilisé, MySQL a été préféré en raison de sa compatibilité éprouvée avec Wordpress et de sa réputation en tant que solution performante et fiable. Néanmoins, MariaDB, étant un fork de MySQL, aurait également été une option viable.

3. Superviseur : CheckMK a été choisi comme outil de supervision du CMS et des services associés. CheckMK permet une surveillance efficace des performances du système, ainsi que la détection rapide des problèmes éventuels, ce qui est essentiel pour garantir la disponibilité et la fiabilité du site vitrine.

4. Serveur : NGINX a été sélectionné comme serveur web pour héberger le CMS. NGINX offre des performances élevées et une gestion efficace des ressources, ce qui est particulièrement avantageux pour les sites à fort trafic. De plus, sa configuration flexible et sa compatibilité avec de nombreuses technologies en font un choix judicieux pour le projet.

5. Distribution : Debian a été choisie comme distribution Linux pour héberger le CMS et les services associés. Debian est réputée pour sa stabilité et sa fiabilité, ce qui en fait un choix approprié pour un environnement de production. De plus, la compatibilité de Debian avec les autres technologies sélectionnées assure une intégration harmonieuse des différents éléments du système.

6. Déploiement : Les machines virtuelles (VM) de l'ISTIC ont été utilisés pour déployer le CMS, conformément aux exigences du projet. Ces templates facilitent la mise en place rapide et sécurisée de l'environnement nécessaire pour le CMS et les services associés. 

7. Outils : phpMyAdmin a été choisi comme outil de gestion de la base de données MySQL. phpMyAdmin permet une gestion simplifiée et conviviale des bases de données, ce qui facilite la maintenance et l'administration du CMS par les membres de l'équipe informatique.


# 3. Installation et configuration

## 3.1. Création de la VM Debian

Afin de créer la machine virtuelle (VM) Debian pour héberger le CMS et les services associés, nous avons utilisé le système mis en place par l'ISTIC. Ce système permet de créer et de gérer des VMs à distance via une interface web accessible à l'adresse suivante : [vm.istic.univ-rennes1.fr](https://vm.istic.univ-rennes1.fr/).

Pour créer la VM Debian, suivez les étapes ci-dessous :

1. Connectez-vous à l'interface web de gestion des VMs de l'ISTIC à l'adresse vm.istic.univ-rennes1.fr en utilisant vos identifiants.
2. Cliquez sur le bouton "Créer une nouvelle VM" ou l'option correspondante dans le menu.
3. Sélectionnez le template de VM Debian dans la liste des templates disponibles.
4. Remplissez les informations requises pour la nouvelle VM, telles que le nom, la description, la taille de la mémoire et du disque, etc.
5. Confirmez la création de la VM en cliquant sur le bouton "Créer" ou l'option correspondante.

Une fois la VM Debian créée, vous pourrez y accéder en utilisant le protocole SSH. Pour ce faire, vous devez vous assurer d'être connecté au réseau de l'ISTIC ou d'utiliser le VPN de l'ISTIC pour accéder au réseau depuis un emplacement distant.

Note : Les informations d'authentification et l'adresse IP de la VM vous seront fournies une fois la VM créée. Conservez ces informations en lieu sûr, car elles seront nécessaires pour accéder à la VM et la configurer ultérieurement.

## 3.2. Connection à la VM

Pour vous connecter à la VM de notre entreprise, vous devez utiliser les identifiants suivants. Veuillez noter qu'il est important de changer le mot de passe par défaut en utilisant la commande `sudo passwd`, ce que nous avons déjà fait.

Pour établir une connexion SSH à la VM, utilisez la commande suivante :

```bash
ssh zprojet@148.60.11.67
```

Lorsqu'il vous sera demandé, saisissez le mot de passe :

```
password : zebulon
```

Une fois connecté, vous pourrez accéder et gérer la VM en utilisant les commandes et les outils Linux habituels.

## 3.4. Installation de NGINX

## 3.5. Installation de MySQL

## 3.3. Installation de Wordpress

## 3.6. Installation de phpMyAdmin