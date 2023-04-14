
# TP1 - SE - Machine virtuelle et hyperviseur

DE ZORDO Benjamin

- [TP1 - SE - Machine virtuelle et hyperviseur](#tp1---se---machine-virtuelle-et-hyperviseur)
- [Introduction](#introduction)
  - [Contexte](#contexte)
  - [Choix de l’OS et téléchargement](#choix-de-los-et-téléchargement)
- [Interface Virtual Box](#interface-virtual-box)
- [Créer sa Machine Virtuelle](#créer-sa-machine-virtuelle)
  - [Configuration physique hôte](#configuration-physique-hôte)
  - [Configuration de la VM](#configuration-de-la-vm)
    - [Système](#système)
    - [Hardware](#hardware)
    - [Disque dur virtuel](#disque-dur-virtuel)
    - [Récapitulatif de la configuration](#récapitulatif-de-la-configuration)
  - [Installation de la VM](#installation-de-la-vm)
    - [Lancer la VM](#lancer-la-vm)
    - [Langage, localisation et clavier](#langage-localisation-et-clavier)
    - [Réseau, utilisateur et mot de passe](#réseau-utilisateur-et-mot-de-passe)
    - [Partitionnement](#partitionnement)
  - [Quelques configurations supplémentaires](#quelques-configurations-supplémentaires)
    - [Taille de l'écran](#taille-de-lécran)
    - [Réseaux et Internet](#réseaux-et-internet)
    - [Bidirectionnalité](#bidirectionnalité)
    - [Les dossiers partagés](#les-dossiers-partagés)
- [Conclusion](#conclusion)
- [Références](#références)


# Introduction

**Objectif du TP :**

L'objectif de ce TP est de comprendre comment fonctionne un hyperviseur ainsi que de comprendre l'utilité des différents paramètres qu'il intègre.

J'ai personnellement déjà utilisé VirtualBox mais en suivant bêtement des tutoriels sur internet ou bien en utilisant des images reconstruites en ".vdi".

Dans ce TP, nous essayerons de donner une explication sur chaque choix de paramètres.

## Contexte

Ici le but de ce TP sera de créer machine virtuelle Kali Linux dans le but de faire des CTFs.

Pour des raisons de stockage, ma partition Linux est presque pleine, la mémoire alloué à la machine virtuelle sera moindre que celle expliquée dans de compte rendu.

## Choix de l’OS et téléchargement

Dans ce TP, nous installerons Kali Linux, une distribution basés sur Debian et largement utilisé dans le domaine de la sécurité informatique.

Kali Linux possède la même interface graphique d'installation que Debian, nous pouvons le télécharger sur le [site officiel](https://www.kali.org/get-kali/).
Plusieurs choix s'offrent à nous, mais nous téléchargerons l'image .iso de Kali Linux en 64 bit faisant 3.6 Go.

# Interface Virtual Box

Dans un premier temps, essayons de comprendre l’interface de VirtualBox.

<img src="./screenshots/01_description/virtualbox_interface.png" alt="drawing" style="width:600px;"/>

La bande à gauche représente l'ensemble des machines virtuelles installées sur l'ordinateur.

![machines](./screenshots/01_description/machines.png)

La barre en haut à droite est une barre d'accès rapide qui permet de configurer une "nouvelle" machine virtuelle, d'en `Ajouter` une, d'ouvrir le menu de configuration ou de `Démarrer` la machine virtuelle sélectionnée.

![outils](./screenshots/01_description/outils.png)

`Nouvelle` : permet de configurer de A à Z une machine virtuelle, l'unique prérequis est d'avoir l'ISO du système a installé.

`Ajouter` : permet de lancer une configuration préinstallé d'un système (fichier .vdi)

Ici la machine "root-me" est sélectionnée. 

# Créer sa Machine Virtuelle

Dès à présent, nous utiliserons le vocabulaire suivant : 
- VM : Machine Virtuel,
- Ordinateur hôte : ordinateur physique sur lequel nous sommes en train de créer une VM,

## Configuration physique hôte

Avant toute configuration virtuelle, il est important de prendre conscience des performances que l'ordinateur hôte possède, car VirtualBox ne pourra (malheureusement) pas changer la configuration et les limites physiques de votre ordinateur.

La partition sur laquelle je me trouve actuellement est un `Arch Linux` de `80 Go de SSD` et `16 Go de RAM`. Le processeur est un `AMD Ryzen 3000 series` possédant `6 cœurs` et `12 Threads`.

## Configuration de la VM

La première étape est de configurer notre VM pour le système que l'on a choisis d'installer.

### Système

Après avoir lancé Virtual Box, cliquer sur `Nouvelle`, une nouvelle fenêtre s'ouvre.

<img src="./screenshots/02_configuration/01_nouvelle.png" alt="drawing" style="width:500px;"/>



Nous pouvons rentrer les informations comme cela : 

|         Syntaxe          |                                                                                Description                                                                                 |
| :----------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|           Nom            |                                                                     le nom de votre machine virtuelle                                                                      |
|          Folder          | localisation où les fichiers de la machine virtuelle vont être stocké (fichiers de configuration et fichier de mémoire virtuelle : celui-ci risque de prendre de la place) |
|        ISO Image         |                                                                  chemin où l'image du système est stocké,                                                                  |
| Edition, Type et Version |                                                             ne nous seront pas utile pour cette configuration                                                              |


Ci-dessous un exemple après avoir complété les informations.

<img src="./screenshots/02_configuration/02_nouvelle.png" alt="drawing" style="width:500px;"/>


### Hardware

La configuration Hardware est très importante puisque c'est elle qui permettra à Virtual Box assigner les performances physiques de votre ordinateur hôte à la VM.

C'est ici qu'il faut prendre en compte les performances physiques de notre ordinateur.

Dans le contexte dans lequel nous nous sommes plongés (VM pour les CTF), notre ordinateur ne sera pas amené à faire utiliser beaucoup de ressources autres que la VM.

D'autre part, certains outils de sécurité sont gourmands en ressources (notamment les outils d'attaques par brute force).

**Ainsi, nous pouvons être généreux dans l'attribution des ressources :**
- **Mémoire vive** : 10 Go sur 16 Go disponibles,
- **Processeur** : 10 Threads sur 12 disponibles.

<img src="./screenshots/02_configuration/03_hardware.png" alt="drawing" style="width:500px;"/>


### Disque dur virtuel

Parmi les fichiers stockés par Virtual Box, on retrouvera un fichier compressé qui représentera la mémoire allouée à la VM. Plus nous allouons de mémoire à notre machine, plus ce fichier sera de dimension importante.

Comme je l'ai dit précédemment, par soucis d'espace sur l'ordinateur hôte, je ne mettrais que 10 Go. Néanmoins, `30 à 40 Go` est largement suffisant pour un VM de CTF !

D'autre part, il est important de noter la limite maximale `Disk Size` que nous propose Virtual Box n'est pas celle de votre ordinateur, mais celle que peut produire notre hyperviseur.

En effet, la mémoire virtuelle attribuée va être compressée ainsi `1 Go de mémoire` représentera `plus de 1 Go de mémoire virtuelle`. Ce qui n'est pas le cas avec la mémoire vive !

<img src="./screenshots/02_configuration/04_virtual_hard_disk.png" alt="drawing" style="width:500px;"/>

In fine, voici les fichiers créés par VirtualBox pour une VM de `10 Go`.

![fichier](screenshots/05_systeme/taillefichier.png)

### Récapitulatif de la configuration

Sur cette fenêtre, nous pouvons vérifier les informations préalablement remplies.


<img src="./screenshots/02_configuration/05_recapitulatif.png" alt="drawing" style="width:500px;"/>

En cliquant sur `Finish` nous pouvons observer notre nouvelle configuration qui apparait dans la colonne de gauche !

<img src="./screenshots/02_configuration/06_general.png" alt="drawing" style="width:600px;"/>

## Installation de la VM

Dans cette partie, nous n'installerons pas la VM à proprement parlé, mais plutôt le système de notre VM.

### Lancer la VM

En sélectionnant la VM, nous pouvons la lancer en cliquant sur `Démarrer`.

La VM se lance comme si nous venions d'allumer un ordinateur et nous arrivons sur la fenêtre d'installation de Kali Linux : choisissons l'installateur graphique (`Graphical install`).

<img src="./screenshots/03_installation/01_bios.png" alt="drawing" style="width:300px;"/>

### Langage, localisation et clavier

Dans cette partie, il suffit de suivre les informations.

Sélectionner la langue de configuration de votre OS, ici, nous avons choisi l'anglais (`English`).

<img src="./screenshots/03_installation/02_langage.png" alt="drawing" style="width:300px;"/>

Il faut désormais choisir sa situation géographique, cela permettra notamment à l'OS de savoir quelle heure afficher.

Étant originaire de l'Île de la Réunion, je choisis `other`, `Indian Ocean` puis `La Réunion`. 

<img src="./screenshots/03_installation/03_location.png" alt="drawing" style="width:300px;"/>

<img src="./screenshots/03_installation/04_location.png" alt="drawing" style="width:300px;"/>

<img src="./screenshots/03_installation/05_location.png" alt="drawing" style="width:300px;"/>


Maintenant vient la configuration du clavier. Étant donné que la langue choisie est l'anglais, l'installateur nous proposera de la configuration anglophone.

Peu importe, choisissons l'une d'entre elles (avec `UTF-8`). 

<img src="./screenshots/03_installation/06_clavier.png" alt="drawing" style="width:300px;"/>


Nous pouvons à présent choisir la langue de notre clavier `French`. Néanmoins, il faudra faire attention par notre configuration est la suivante :

```
Clavier au format anglophone (QWERTY)
Encodage UTF-8
Configuration keymap française
```
<img src="./screenshots/03_installation/07_keymap.png" alt="drawing" style="width:300px;"/>

### Réseau, utilisateur et mot de passe

Une fois ces configurations terminées, l'installateur fait son bout de chemin en installant le nécessaire pour la configuration demandée.

<img src="./screenshots/03_installation/08.png" alt="drawing" style="width:300px;"/>

Le système nous demande d'insérer un nom qui identifiera notre VM sur le réseau : `comment s'appelle votre ordinateur virtuel ?`

<img src="./screenshots/03_installation/09_configure_network.png" alt="drawing" style="width:300px;"/>

Ici pas besoin d'utiliser un nom de domaine, passons cette étape avec le bouton `Continue`.

<img src="./screenshots/03_installation/10_configure_network.png" alt="drawing" style="width:300px;"/>

Désormais, nous devons créer notre mot de passe administrateur ainsi que notre nom d'utilisateur.

<img src="./screenshots/03_installation/11_password.png" alt="drawing" style="width:300px;"/>
<img src="./screenshots/03_installation/11_username.png" alt="drawing" style="width:300px;"/>

### Partitionnement

Dans cette partie, nous allons dire au système : `je veux que tu t'installe ici`.

Rappelons-nous que nous avons créé un espace de stockage virtuel, c'est à cet endroit que nous créerons notre partition Kali Linux.

Choisir `utiliser un disque entier` : `Guided - use entire disk`

<img src="./screenshots/03_installation/12_partition_disk.png" alt="drawing" style="width:300px;"/>

Selectionner le disque virtuel (seul disque disponible).

<img src="./screenshots/03_installation/13_selection_disk.png" alt="drawing" style="width:300px;"/>

Puis `All files in one partitions`.

<img src="./screenshots/03_installation/14.png" alt="drawing" style="width:300px;"/>

À partir de ce moment, l'installateur agence le disque virtuel de la manière la plus simple. Il ne reste plus qu'à accepter ces changements : `Finish partitionning and write changes to disk`.

<img src="./screenshots/03_installation/15.png" alt="drawing" style="width:300px;"/>

Accepter avec `Yes`.

<img src="./screenshots/03_installation/16_change.png" alt="drawing" style="width:300px;"/>

L'installation se lance !

<img src="./screenshots/03_installation/17_installation.png" alt="drawing" style="width:300px;"/>

Nous sélectionnons les dernières dépendances à installer : XFCE et les outils de sécurité.

<img src="./screenshots/03_installation/18_xfce.png" alt="drawing" style="width:300px;"/>

L'installation se termine puis le système bootera automatiquement sur l'OS (attention l'installation peut durer un moment !).

<img src="./screenshots/03_installation/19_installation_software.png" alt="drawing" style="width:300px;"/>

Lorsque la fenêtre de login s'ouvre, le clavier sera malheureusement en QWERTY (je n'ai pas sur résoudre ce problème). Néanmoins, lors de la connexion, la configuration revient en AZERTY !

## Quelques configurations supplémentaires

Pour jouir complétement de notre VM certaines configurations sont nécessaires, en voici quelques-unes.

### Taille de l'écran

Comme tout dans une VM, l'écran est un moniteur virtuel. Il est simple de changer la dimension de ce moniteur (surtout si l'on a un écran externe).

Pour cela, nous pouvons aller dans `Ecran` puis `Ecran virtuel n°1` et redimensionner dans le format de notre choix

<img src="./screenshots/04_shareddirectory/ecran.png" alt="drawing" style="width:350px;"/>

### Réseaux et Internet

Il sera pratique d'avoir internet sur notre VM et de pouvoir être identifier comme un appareil réseau. Il faut s'assurer que dans `Paramètres`, `Réseau` le mode d'accès soit bien en `Accès par pont` et que votre carte réseau est sélectionnée (ici `wlan`).

<img src="./screenshots/04_shareddirectory/network.png" alt="drawing" style="width:350px;"/>

Nous pouvons à présent utiliser internet sur notre VM qui reconnait être connecté par liaison filière.

<img src="./screenshots/04_shareddirectory/proof_network.png" alt="drawing" style="width:350px;"/>

### Bidirectionnalité

Afin de pouvoir échanger directement des fichiers ou des informations copiées depuis l'ordinateur hôte vers la VM et réciproquement, VirtualBox nous propose d'activer la bidirectionnalité.

Ce paramètre se trouve dans les `Paramètres`, `Général` et `Avancé`. Il suffit de choisir `Bidirectionnel` pour le presse-papier et le Drag and Drop.

<img src="./screenshots/04_shareddirectory/bidirectionalite.png" alt="drawing" style="width:350px;"/>

### Les dossiers partagés

Cette fois, dans un but d'échanger des fichiers entre l'ordinateur hôte et la VM, Virtual Box propose un système de partage de fichier.

Pour cela, il faut se rendre dans les `Paramètres` de notre VM et aller dans l'onglet `Dossiers partagés`.

<img src="./screenshots/04_shareddirectory/Capture%20d’écran_2023-04-04_10-19-54.png" alt="drawing" style="width:350px;"/>

Sélectionner `Dossier permanents` puis cliquer sur le dossier bleu à droite.

<img src="./screenshots/04_shareddirectory/02_shared_dir.png" alt="drawing" style="width:350px;"/>

Vous allez ajouter un dossier partagé en indiquant : 
- le chemin du dossier à partager sur l'ordinateur hôte,
- le nom du dossier (nom qui apparaitra sur la VM),
- cocher `Montage automatique` et `Configuration permanente`,
- appliquer en cliquant sur `OK`.

<img src="./screenshots/04_shareddirectory/03_add.png" alt="drawing" style="width:350px;"/>

Une fois connecter sur la VM, vous verrez apparaitre dans `Device` l'espace créer. Le dossier partagé est représenté comme une clef USB.

Nous pouvons y retrouver les fichiers mis à l'intérieur : ici le logiciel `Volatility2`.

<img src="./screenshots/04_shareddirectory/proof.png" alt="drawing" style="width:350px;"/>


# Conclusion

Après ces étapes de configuration, vous êtes fin prêt et bien armé pour vos CTF !


# Références

|              Nom               |                                            URL                                             |
| :-----------------------------: | :----------------------------------------------------------------------------------------: |
|     Téléchargement de l'ISO     |                               https://www.kali.org/get-kali/                               |
| Création d'une machine viruelle | https://www.zebulon.fr/dossiers/tutoriaux/83-tutoriel-virtualbox-machine-virtuelle.html/3/ |
|  Accelerer et optimiser une VM  |             https://www.malekal.com/accelerer-et-optimiser-une-vm-virtualbox/              |
|   Créer un dossier de partage   |          https://www.malekal.com/comment-creer-un-dossier-partage-sur-virtualbox/          |
