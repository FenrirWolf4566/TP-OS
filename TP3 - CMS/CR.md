Wysiwyg : idée de base de l'éditeur de texte



https://www.salesforce.com/fr/resources/articles/definition-cms/#topic2




new : 148.60.11.212


1. Installation de Nginx
Nous avons choisis de travailler avec Nginx Proxy manager permettant d'une part d'utiliser les focntionalités Ngonx comme serveur web mais également le Manager permettant de gérer les requetes HTTPS

Pour installer Nginx Proxy Manager, nous avons utiliser une docker (uniquement accessible comme cela). Il a donc fallut installer Docker sur la machine hôte.

```
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    # Uncomment the next line if you uncomment anything in the section
    # environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```
Email:    admin@example.com
Password: changeme
Notre 


2. install phpmyadmin & maria

[Install MySQL](https://www.digitalocean.com/community/tutorials/how-to-install-the-latest-mysql-on-debian-10)
[Install phpmyadmin](https://www.itzgeek.com/how-tos/linux/debian/how-to-install-phpmyadmin-with-nginx-on-debian-10.html)


Name DB : phpmyadmin
host : localhost

user : pma               // n'as pas les droits root (je pense que c'est mieux de prendre ça pour Wordpress)
pass: pmapass

root user : root         // du coup ça c'est plus pour modif a la main je pense 
root pass: ' '


