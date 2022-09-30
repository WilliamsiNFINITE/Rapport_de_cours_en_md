# Résolution d'exercices de CTF

Ce rapport documente la manière dont j'ai réussi à finir les différentes challenges [RootMe](https://www.root-me.org/) dans le cadre du cours de [Sécurité et réseau](https://moodle.ensta-bretagne.fr/course/view.php?id=1819). Le pseudo utilisé est [WilliamsiNIFINITE](https://www.root-me.org/WilliamsiNFINITE)

## Sommaire

* [Réseau](#réseaux)
* [Cryptanalyse](#cryptanalyse)
* [Web - Serveur](#web---serveur)

## Réseaux

### FTP - Authentification

Pour ce challenge, un fichier d'extension pcap est téléchargé puis ouvert avec l'outil [WireShark](https://www.wireshark.org/).
Avec comme indice le nom du challenge, j'ai choisi de filtrer uniquement les paquets avec le protocole FTP et une recherche 'manuelle' du password 

![image](https://user-images.githubusercontent.com/91114817/193211793-d24ac287-7fc6-44f8-8d5c-b25ea7258ad8.png)

flag : cdts3500

### TELNET - authentification

Pour ce challenge, un fichier d'extension pcap est téléchargé puis ouvert avec l'outil [WireShark](https://www.wireshark.org/).
Avec comme indice le nom du challenge, j'ai choisi de filtrer uniquement les paquets avec le protocole TELNET et une recherche 'manuelle' du password 
J'ai trouvé le paaquet 56 intéressant 

![image](https://user-images.githubusercontent.com/91114817/193214515-5c75a235-c887-4788-9383-57c7eb43878d.png)

Les paquets suivants ont pour data respectivement : 'u', 's', 'e', 'r', '\r', '\r\n'

flag : user

### ETHERNET - trame

Pour ce challenge, un fichier d'extension txt a été téléchargé. J'ai cherché 'ethernet trame reader' dans mon moteur de recherche et suis arrivé sur le site [Hex Packet Decoder](https://hpd.gasmi.net/). J'y ai collé le contenu du fichier téléchargé.

![image](https://user-images.githubusercontent.com/91114817/193216866-746609ee-23c0-41a7-918c-02be10b5ab00.png)

Tout en bas on trouve : 

![image](https://user-images.githubusercontent.com/91114817/193216966-9578901c-4a00-43ff-8a21-3830864c6b36.png)

flag : confi:dential

### Authentification twitter

Pour ce challenge, un fichier d'extension pcap est téléchargé puis ouvert avec l'outil [WireShark](https://www.wireshark.org/).
Le fichier ne contient qu'un seul paquet dans lequel on trouve 

![image](https://user-images.githubusercontent.com/91114817/193217699-48d44190-1cfc-453d-9d8d-e53b45efdaed.png)

flag : password

## Cryptanalyse

### 

## Web - Serveur 

### 


