# Compte rendu du TP5 :  BUILDROOT

Ce rapport documente la manière dont Thalul-De Marcelin et moi même, Williams HOARAU, avons réalisé le TP5 sur Buildroot.

## Sommaire

* [Installation de Buildroot](#installation-de-buildroot)
* [Installation de l'environnement embarque](#installation-de-l-environnement-embarque)
* [Communication avec la carte](#communication-avec-la-carte)


## Installation de Buildroot

- A quoi sert chacun des paquets ? (Information disponibles sur https://doc.ubuntu-fr.org/)
  - [sed](https://doc.ubuntu-fr.org/sed) : sed (et cut) permettent de modifier ou de supprimer une partie d’une chaîne de caractères. 
  - [make](https://doc.ubuntu-fr.org/make) : make est un utilitaire pour "scripter" la compilation et l'édition de liens
  - [binutils](https://doc.ubuntu-fr.org/tutoriel/compilation_croisee) : Le compilateur comporte deux parties : les binutils et gcc. Les binutils comportent les outils de gestion comme ld ou ar.
  - [gcc](https://doc.ubuntu-fr.org/gcc) : GCC (GNU Compiler Collection) est une suite de logiciels libres de compilation. On l'utilise dans le monde Linux dès que l'on veut transcrire du code source en langage machine. gcc correspond à l'outil de compilation en C
  - [g++](https://doc.ubuntu-fr.org/gcc) : Identique à gcc pour le langage C++.
  - [bash](https://doc.ubuntu-fr.org/bash) : BASH est un shell. C'est le shell de base utilisé dans le terminal (Voir [Shell](https://doc.ubuntu-fr.org/shell))
  - [patch](https://doc.ubuntu-fr.org/patch) : Patch permet d'appliquer un patch obtenu au moyen d'un diff (svn diff, git diff, …)
  - [gzip](https://doc.ubuntu-fr.org/archivage) : Package qui s'occupe des archives
  - [bzip2](https://doc.ubuntu-fr.org/tutoriel/reparer_une_archive_corrompue) : Package qui s'occupe des archives. 
  - [perl](https://www.perl.org/) : Perl est un langage de programmation polyvalent, interprété de haut niveau et dynamique
  - [tar](https://doc.ubuntu-fr.org/tar) : C'est un outil tres puissants qui permet de creer et de manipuler des archives.
  - [cpio](https://en.wikipedia.org/wiki/Cpio): CPIO signifie « copy in, copy out ». Il est utilisé pour traiter les fichiers d'archive comme *.cpio ou *.tar. Cette commande permet de copier des fichiers vers et depuis des archives.
  - [python](https://en.wikipedia.org/wiki/Python_(programming_language)): Python est un langage de programation, de haut niveau et dynamique
  - [unzip]: C'est un outil qui nous permet de deziper des archives
  - [rsync](https://en.wikipedia.org/wiki/Rsync): rsync est un utilitaire permettant de transférer et de synchroniser efficacement des fichiers entre un ordinateur et un lecteur de stockage et entre des ordinateurs en réseau en comparant les temps de modification et les tailles des fichiers
  - [wget](https://www.gnu.org/software/wget/): GNU Wget est un logiciel libre permettant de récupérer des fichiers en utilisant HTTP, HTTPS, FTP et FTPS.
  - [libncurses-dev](https://packages.ubuntu.com/focal/libncurses-dev)
  - [libssl-dev](https://packages.ubuntu.com/bionic/libssl-dev): libssl-dev fait partie de l'implémentation par le projet OpenSSL des protocoles cryptographiques SSL et TLS pour une communication sécurisée sur Internet.

## Installation de l environnement embarque

- Expliquer en quelques paragraphes à quoi correspond chacun des fichiers copiés :
  - MLO, u-boot.img : Ces deux fichiers contituent le bootloader. Ce sont eux qui vont être utilisés à la place du bootloader habituel (XLoader) lorsque la carte va être démarrée depuis une carte microSD.
  - zImage : Elle constitue l'image du noyau.
  - uEnv.txt : Ce fichier contitue la [séquence de démarrage](https://definir-tech.com/sequence-damorcage/#:~:text=La%20s%C3%A9quence%20de%20d%C3%A9marrage%20est,d'exploitation%20(OS).) (l'ordre dans lequel un ordinateur recherche les périphériques de stockage de données non volatiles contenant le code de programme pour charger le système d'exploitation).
  - am335x-boneblack.dtb : Ce fichier est la [device tree](https://en.wikipedia.org/wiki/Devicetree) (structure de données décrivant les composants matériels d'un ordinateur particulier afin que le noyau du système d'exploitation puisse utiliser et gérer ces composants).


- Vous pouvez lancer GTKterm. La configuration à mettre est différente de la précédente. Le port à utiliser
est le ttyUSB0. Quelle différence y a-t-il entre ce port et le port utilisé précédemment (ttyACM0) ?
  - ttyUSB0 est le périphérique pour le premier convertisseur série USB. En utilisant un câble série USB, on  utilise un ttyUSBn pour nous connecter au port série du peripherique.
  - Tandis que le ttyAMA0 est le périphérique pour le premier port série sur l'architecture ARM.

Remarque : Lorsque  nous sommes censé avoir un noyau en version 5.3.14, en pratique, nous en avons un de version 4.19.94

## Communication avec la carte

- Qu’est-ce que le protocole tftp ?
  - Le Trivial File Transfer Protocol, TFTP pour faire court, est un protocole client-serveur très simple, qui gère les transferts de fichiers dans les réseaux informatiques. Par défaut, le protocole TFTP est basé sur le protocole de transport simplifié UDP (User Datagram Protocol) qui permet d’envoyer des données entre partenaires de communication sans partager une connexion fixe. Il est également possible d’implémenter le TFTP basé sur d’autres protocoles.

- Expliquons les etapes d'installation de tftp sur notre carte
  - On commence par installer les package tftp, le daemon tftp et aussi le daemon xinetd pour notre server.
  - Nous créons ensuite le répertoire /tftpboot, c'est le repertoire de dépôt de fichiers à transmettre vers la carte, on prend soin aussi de donner tous les droits d'access sur le repertoire.
  - On creer un fichier de confguration pour le serveur tftp en specifiant les parametres :
    ```
      {
       socket_type = dgram
       protocol = udp
       wait = yes
       user = root
       server = /usr/sbin/in.tftpd
       server_args = -s /tftpboot
      }
    ```
  - Puis on redemare le daemon xinet pour prendre en compte nos modifications

## Additionnel

IP machine : 192.168.141.134
