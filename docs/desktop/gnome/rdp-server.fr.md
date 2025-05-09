---
title: Partage du Desktop via RDP
author: Ezequiel Bruni
contributors: Steven Spencer, Ganna Zhyrnova, Zhang Zhuyue
---

## Introduction

Ce guide est pour vous si vous souhaitez partager votre desktop (GNOME) sous Rocky Linux ou accéder à d'autres bureaux partagés.

Pour les débutants, vous allez utiliser RDP. RDP signifie Remote Desktop Protocol, et il fait exactement ce que cela indique : il vous permet de visualiser et d'interagir avec des ordinateurs à distance, le tout avec une interface graphique. Toutefois, vous devrez vous plonger un peu dans l'art d'utiliser la ligne de commande pour configurer le logiciel.

!!! note "Remarque"

```
Par défaut, Rocky Linux vous permet de partager votre bureau via un autre protocole VNC. VNC est assez pratique, mais RDP offre généralement une expérience beaucoup plus fluide et peut gérer des résolutions d'écran inhabituelles.
```

## Prérequis

Pour ce guide, nous partons du principe que vous disposez déjà de la configuration suivante :

- Rocky Linux et GNOME
- Flatpak et Flathub installés et opérationnels
- Un compte utilisateur non root
- Accès administrateur ou sudo et pouvoir coller des commandes dans le terminal
- Le serveur X (pour partager votre bureau)

!!! info "Info"

```
Il y a actuellement quelques projets en cours pour rendre le serveur d'affichage Wayland et RDP agréables et les versions plus récentes de GNOME sont livrées avec un serveur RDP intégré qui fait l'affaire. Cependant, la version Rocky Linux de GNOME ne dispose pas de cette fonctionnalité, il est donc beaucoup plus facile de mettre en place votre session RDP avec x11.
```

## Partager votre bureau Rocky Linux GNOME avec RDP

Pour rendre votre bureau Rocky Linux accessible à distance, vous avez besoin d'un serveur RDP. Pour nos besoins, `xrdp` suffira largement. Vous devrez utiliser le terminal pour cela, car il s'agit d'un programme uniquement CLI.

!!! info "Info"

````
Le paquet `xrdp` se trouve dans le dépôt EPEL (https://wiki.rockylinux.org/rocky/repo/#community-approved-repositories), qui fournit des reconstructions de paquets Fedora pour chaque Enterprise Linux pris en charge. Si vous ne l'avez pas activé, utilisez les commandes suivantes. Vous devez également activer `CRB` (appelé « PowerTools » dans Rocky Linux 8) avant d'ajouter le dépôt `EPEL`.

Rocky Linux 8 :

```bash
sudo dnf config-manager --set-enabled powertools
sudo dnf install epel-release
```

Rocky Linux 9 :

```bash
sudo dnf config-manager --set-enabled crb
sudo dnf install epel-release
```
````

Après avoir ajouté le référentiel EPEL (ou si vous l'avez déjà ajouté), utilisez la commande suivante pour installer `xrdp` :

```bash
sudo dnf install xrdp
```

Une fois installé, vous devez activer le service :

```bash
sudo systemctl enable --now xrdp
```

Si tout se passe bien, le serveur RDP sera installé, activé et en cours d'exécution. Mais vous ne pouvez pas vous connecter pour l’instant, vous devez d’abord ouvrir le port adéquat sur votre pare-feu.

Si vous souhaitez en savoir plus sur le fonctionnement de l'application de pare-feu de Rocky Linux, `firewalld`, veuillez consulter notre [guide du débutant sur `firewalld`](../../guides/security/firewalld-beginners.md). Si vous voulez juste avancer, exécutez les commandes suivantes :

```bash
sudo firewall-cmd --zone=public --add-port=3389/tcp --permanent
sudo firewall-cmd --reload
```

Pour les débutants : ces commandes ouvrent le port RDP de votre pare-feu afin que vous puissiez accepter les connexions RDP entrantes et redémarrer le pare-feu pour appliquer les modifications. Ensuite, vous devez relancer le pare-feu avant de voir les changements. À ce stade, si vous le souhaitez, vous pouvez redémarrer votre PC pour plus de sécurité.

Si vous ne voulez pas redémarrer le système, vous devrez vous déconnecter. RDP utilise les identifiants de votre compte utilisateur pour des raisons de sécurité. Se connecter à distance alors que vous êtes déjà connecté localement à votre bureau n'est pas possible. Du moins, pas sur le même compte utilisateur.

!!! info

```
Vous pouvez également utiliser l'application Firewall pour gérer `firewalld` et ouvrir les ports de votre choix. Surveillez cet espace pour un lien vers mon guide d'installation et d'utilisation de l'application Pare-feu.
```

## Accès à votre Desktop Rocky Linux et/ou à d'autres Bureaux avec RDP

Vous avez vu comment installer un serveur RDP et vous avez désormais besoin d'un client RDP. Sous Windows, l’application Remote Desktop Connection fait très bien l’affaire. Si vous souhaitez accéder à votre machine Rocky Linux à partir d'une autre machine Linux, vous devrez installer une solution tierce.

Sur GNOME, le logiciel `Remmina` bénéficie de la confiance de l'auteur. C'est simple à utiliser, c’est stable et cela fonctionne très bien.

Si `Flatpak`/`Flathub` est déjà installé, ouvrez simplement l’application `Software` et recherchez `Remmina`.

![The Gnome Software app on the Remmina page](images/rdp_images/01-remmina.png)

Installez et lancez. Remarque : il s'agit de la marche à suivre permettant d'ajouter une connexion RDP dans `Remmina` en particulier, mais il est similaire pour presque toutes les autres applications client RDP que vous êtes susceptible de trouver.

Quoi qu’il en soit, cliquez sur le bouton `+` dans le coin supérieur gauche pour ajouter une connexion. Donnez un nom à la connexion dans le champ adéquat et indiquez l'adresse IP de l'ordinateur distant, ainsi que le nom d'utilisateur et le mot de passe de votre compte utilisateur distant. N'oubliez pas que si vos ordinateurs sont sur le même réseau, vous devrez utiliser son adresse IP locale et non celle vue de l'extérieur sur Internet.

![The Remmina connection profile form](images/rdp_images/02-remmina-config.png)

Et si vos ordinateurs ne sont pas sur le même réseau, il est nécessaire que vous sachiez comment effectuer la redirection de port ou que l'ordinateur distant dispose d'une adresse IP statique. Mais cela dépasse le cadre de ce document.

Faites défiler la barre verticale vers le bas pour des options telles que la prise en charge de plusieurs moniteurs, les résolutions personnalisées, etc. De plus, l'option `Network Connection Type` vous permettra d'équilibrer l'utilisation de la bande passante avec la qualité de l'image dans votre client RDP.

Si vos ordinateurs sont sur le même réseau, optez simplement pour le réseau local LAN pour obtenir la meilleure qualité.

Cliquez ensuite sur ++"Save"++ et ++"Connect"++.

Voici à quoi cela ressemble avec le client Windows Remote Desktop Connection. L'auteur a écrit l'intégralité de ce document sur son serveur Rocky Linux local avec RDP.

![A screenshot of my docs-writing environment, at a 5120x1440p resolution](images/rdp_images/03-rdp-connection.jpg)

## Conclusion

C'est tout ce que vous devez savoir pour exécuter RDP sur Rocky Linux et partager votre Desktop. Cela fera l’affaire si vous avez seulement besoin d’accéder à certains fichiers et applications à distance.
