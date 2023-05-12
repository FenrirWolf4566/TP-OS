


---
# Prérequis

Pour ce TP nous avons utiliser les VM de l'ISTIC dont l'ip est la suivante : `148.60.11.67`.
Nous avons également utiliser un nom de domaine : `http://codybenji-cms.istic.univ-rennes1.fr` qui pointe vers l'IP de la VM sous Debian.

Notre choix s'est porté sur les outils : 
- Wordpress : un CMS opensource,
- Nginx : un serveur web,
- MySQL / PhpMyAdmin : une base de données et son interface d'administration.

Nous nous sommes aidé de certains tutoriel pour installer ces outils qui seront cités dans chaque partie de ce document.

Avant de faire quoi que ce soit, il faut se connecter en SSH au serveur puis  mettre à jour la liste des paquets disponibles et leurs versions avec la commande suivante : `sudo apt update`.


# 1. Installation de Nginx ([tutoriel](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10))

Nginx permet de faire le pont entre notre serveur et le client ; il permet de gérer les requetes HTTP et HTTPS.

Pour l'installer NGINX, il faut utiliser la commande suivante : `sudo apt install nginx`.

> Sur le tutoriel le firewall de debian (ufw) est utilisé, néanmoins pour plus de practicité nous avons décidé de ne pas l'utiliser bien que dans un contexte de sécurité il est préférable de l'utiliser.

Le serveur apache est déjà installé sur la VM et utilise le port 80, il est donc necessaire de stopper le service apache : `sudo systemctl stop apache2.service`.

Enfin, pour lancer le service Nginx il faut utiliser la commande suivante : `sudo systemctl start nginx`.

Pour vérifier que Nginx est bien installé, il faut taper l'adresse IP de la VM dans un navigateur web. Si tout est bien configuré, on devrait voir la page suivante :

![nginx_congrat](/TP3%20-%20CMS/assets/1_nginx_congrat.png)


# 2. Wordpress

Wordpress est un CMS opensource qui permet de créer des sites web dynamiques. Selon le site [Saleforce](https://www.salesforce.com/fr/resources/articles/definition-cms/#topic2), un CMS est un outil qui permet de "créer, de gérer et de modifier facilement un site web, sans avoir besoin de connaissances techniques en langage informatique".

Il fait notamenent partie de WySiWyg (What You See Is What You Get) qui est un éditeur de texte qui permet de voir directement le rendu final du texte.

Pour l'installation de Wordpress nous n'avons pas suivis qu'un seul tutoriel mais une multitudes qui nous ont données des informations complémentaires : [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lemp-nginx-mariadb-and-php-on-debian-10), [Spinupwp](https://spinupwp.com/hosting-wordpress-yourself-complete-nginx-configuration/).

## a. Installation de Wordpress

Nous devons nous rendre dans le dossier `/var/www/html/` qui est le dossier racine de Nginx. Pour cela il faut utiliser la commande suivante : `cd /var/www/html/`.

L'installation de Wordpress passe par le téléchargement du package de Wordpress sur le site officiel grâce à la commande suivante : `wget https://wordpress.org/latest.tar.gz`, puis de sa décompression avec la commande suivante : `tar -xzvf latest.tar.gz`.

Ainsi nous nous retrouvons avec ceci : 
```bash
zprojet@debian:/var/www/html$ ls
index.html  phpmyadmin  wordpress
zprojet@debian:/var/www/html$ cd wordpress
zprojet@debian:/var/www/html/wordpress$ ls
index.php        wp-admin              wp-content         wp-load.php      wp-signup.php
license.txt      wp-blog-header.php    wp-cron.php        wp-login.php     wp-trackback.php
readme.html      wp-comments-post.php  wp-includes        wp-mail.php      xmlrpc.php
wp-activate.php  wp-config.php         wp-links-opml.php  wp-settings.php
```

## b. Configuration de Nginx avec Wordpress

Nginx possède un dossier contenant des fichiers de configuration : `/etc/nginx/sites-available/`. Nous allons donc créer un nouveau fichier de configiration pour qu'il prenne en compte Wordpress :

```bash
zprojet@debian:/etc/nginx/sites-available$ ls
default  wordpress.conf
```

Voici le contenu du fichier `wordpress.conf` : 

```
server {
  listen 80;
  root /var/www/html/wordpress;
  index index.php index.html index.html index.htm;
  server_name codybenjam.ddns.net;
  access_log /var/log/nginx/wordpress_access.log;
  error_log /var/log/nginx/wordpress_error.log;
  client_max_body_size 64M;


  location / {
      # First attempt to serve request as file, then
      # as directory, then fall back to WordPress index.php
      try_files $uri $uri/ /index.php?$args;
  }

  # Add the new location block for /phpmyadmin
  location /phpmyadmin {
      alias /var/www/html/phpmyadmin;
      index index.php;

      location ~ \.php$ {
          try_files $uri =404;
          fastcgi_split_path_info ^(.+\.php)(/.+)$;
          include /etc/nginx/fastcgi_params;
          fastcgi_read_timeout 3600s;
          fastcgi_buffer_size 128k;
          fastcgi_buffers 4 128k;
          fastcgi_param SCRIPT_FILENAME $request_filename;
          fastcgi_pass unix:/run/php/php7.4-fpm.sock;
          fastcgi_index index.php;
      }
  }

  # suport php
  location ~ \.php$ {
    try_files $uri =404;
    include /etc/nginx/fastcgi_params;
    fastcgi_read_timeout 3600s;
    fastcgi_buffer_size 128k;
    fastcgi_buffers 4 128k;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    fastcgi_index index.php;
  }
}
```








# 2. MySQL et PhpMyAdmin

[Install MySQL](https://www.digitalocean.com/community/tutorials/how-to-install-the-latest-mysql-on-debian-10)

[Install PhpMyAdmin](https://www.itzgeek.com/how-tos/linux/debian/how-to-install-phpmyadmin-with-nginx-on-debian-10.html)


Name DB : phpmyadmin
host : localhost

user : pma               // n'as pas les droits root (je pense que c'est mieux de prendre ça pour Wordpress)
pass: pmapass

root user : root         // du coup ça c'est plus pour modif a la main je pense 
root pass: ' '