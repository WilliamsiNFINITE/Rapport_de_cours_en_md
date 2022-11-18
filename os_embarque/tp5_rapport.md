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

- 

## Communication avec la carte
