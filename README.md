
# tp_Linux


# TP Linux Embarqué

GUIFFAULT GABRIEL JACQUOT NOLAN


## 1 Prise en main
Note : en cas de doute, appelez votre prof avant de faire une connerie !

### 1.1 Préparation de la carte SD
Avant de commencer, il faut flasher la carte SD avec l’image fournie par Terasic. Elle est disponible sur le lien ci-dessus. Il s’agit du fichier suivant :

VEEK_MT2S_LXDE/VEEK_MT2S_LXDE.img

Sous Windows Utilisez l’outile Win32DiskImager. Des tutoriels sont disponibles
en ligne. Il faudra l’installer au besoin.

### 1.2 Démarrage
Insérez la carte SD fraichement programmée, branchez la carte VEEK-MT2S et
allumez-la : bouton rouge ! Ça clignote de partout et un linux se lance.

Prenez un peu de temps pour explorer les différents programmes fournis sur
le bureau de Lxde.

Mais sans clavier, ce n’est pas très pratique pour programmer cette « brique ».
Nous allons y remédier.

### 1.3 Connexion au système

### 1.3.1 Liaison série
Le premier moyen pour se connecter sur un objet embarqué, c’est très souvent
par le port série. Une fois que l’on aura eu accès au DE-10, on configurera le
réseau, pour pouvoir ensuite y accéder via ssh.

Tout d’abord, déterminer le port à utiliser pour se connecter à la carte.
Il y a plusieurs ports USB sur la carte :

— 2 hôtes usb A

— 1 usb B : max blaster pour la programmation

— 1 usb mini : uart to usb ← c’est celui-là qui nous intéresse.

### 1.3.2 Utilisez un logiciel de liaison série
Sous Windows : Utilisez le logiciel PuTTy pour vous connecter grace au port
série. Sélectionnez serial, et à l’aide du panneau de configuration de windows,
cherchez le port COM de votre carte SoC. Pour la vitesse, entrez 115200.
Une fois connecté au SoC :

Pour vous identifier :

— login : root

— password : aucun (vraiment rien, ne tapez que sur entrée)

Pour l’exercice, nous allons redémarrer le SoC pour observer la séquence de
démarrage.
```
reboot
```
Loggez-vous à nouveau. L’image copiée précédemment n’occupe pas la totalité de la carte SD. Quelle est la taille occupée ? Utilisez la commande suivante :
```
df -h
```
Des scripts sont fournis pour augmenter la taille du système de fichiers et
profiter de l’ensemble de la carte SD :
```
./expand_rootfs.sh
```
Rebootez la carte (proprement !), puis, une fois loggé :
```
./resize2fs_once
```
Vérifiez que vous avez bien 32GB de disponible sur la carte SD.

### 1.3.3 Configuration réseau

— Branchez la carte VEEK sur le switch via un câble réseau,

— À l’aide de la commande ifconfig, vérifiez que la carte dispose d’une
adresse IP,

— Éditer le fichier /etc/network/interfaces de la manière suivante :
```
# interfaces(5) file used by ifup(8) and ifdown(8)
# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d
auto eth0
iface eth0 inet dhcp
allow-hotplug eth0
```
Pour ces modification, vous allez devoir utiliser vim de la façon suivante :

— Éditez le fichier avec la commande vim /etc/network/interfaces,

— Effacez toutes les lignes inutiles en appuyant deux fois sur la touche d

— Insérez une nouvelle ligne en appuyant sur o. Cette touche vous bascule
en mode -- INSERT --

— À partir de là vous pouvez taper le contenu du fichier,

— Quand c’est terminé, tapez sur la touche Echap pour revenir en mode command

— Tapez :wq puis sur entrée pour sauvegarder et quitter vim.
Rebootez une nouvelle fois, et là normalement vous allez avoir une adresse IP
correcte. Vérifiez avec ifconfig.

— Si tout est correct, vous devez pouvoir pinger avec le PC dans les 2 sens
ping <adresse_ip>


