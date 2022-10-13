# Résolution d'exercices de CTF

Ce rapport documente la manière dont j'ai réussi à finir les différentes challenges [RootMe](https://www.root-me.org/) dans le cadre du cours de [Sécurité et réseau](https://moodle.ensta-bretagne.fr/course/view.php?id=1819). Le pseudo utilisé est [WilliamsiNIFINITE](https://www.root-me.org/WilliamsiNFINITE)

## Sommaire

* [Réseau](#réseaux)
* [Cryptanalyse](#cryptanalyse)
* [Web - Serveur](#web---serveur)
* [Exercice 1](#exercice-1)

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

### Bluetooth - Fichier inconnu

Pour ce challenge, un fichier d'extension bin est téléchargé puis ouvert avec l'outil [WireShark](https://www.wireshark.org/).
Dans le paquet 9, on trouve les informations utiles (adresse mac et nom du téléphone) 

![image](https://user-images.githubusercontent.com/91114817/193238058-70defad8-71c3-48b4-a748-9878505feeee.png)

La concaténation des deux a ensuite était hachée avec le site (sha1.fr)[https://www.sha1.fr/]

flag : c1d0349c153ed96fe2fadf44e880aef9e69c122b

### IP - Time To Live

Pour ce challenge, un fichier d'extension pcap est téléchargé puis ouvert avec l'outil [WireShark](https://www.wireshark.org/).

![image](https://user-images.githubusercontent.com/91114817/193243055-2f0eadbf-a8c5-4fc5-bebb-ffa6301f6636.png)

Après une succession de ttl qui échouent, on voit que la réponse de l'hôte arrive finalement au paquet 72.

flag : 13

###

## Cryptanalyse

### Chiffrement par décalage

Pour ce challenge, un fichier d'extension .bin est téléchargé. J'ai ouvert le fichier avec un éditeur de texte et j'ai entré son contenu dans le détecteur de code du site [dCode](https://www.dcode.fr/) qui m'a conduit sur la méthode de [chiffrement par décalage ASCII](https://www.dcode.fr/chiffre-decalage-ascii). J'ai entré le texte copié pour trouver le flag.

flag : Yolaihu


### Substitution monoalphabétique - César

Pour ce challenge, le texte suivant été donné : 

```console
tm bcsv qolfp
f'dmvd xuhm exl tgak
hlrkiv sydg hxm
qiswzzwf qrf oqdueqe
dpae resd wndo
liva bu vgtokx sjzk
hmb rqch fqwbg
fmmft seront sntsdr pmsecq
```
J'ai utilisé le détecteur de code du site [dCode](https://www.dcode.fr/) afin de trouver une correspondance avec un algorithme connu. Il m'a proposé le Chiffre de Trithème. Puisqu'il n'y avait pas de résultat, j'ai essayé avec le chiffrement par décalage. Après avoir utilisé du brut force, j'ai reconnu la célèbre comptine :

```console
un deux trois 
j'irai dans les bois
quatre cinq six
cueillir des cerises
sept huit neuf
dans un panier neuf
dix onze douze
elles seront toutes rouges
```

flag : ujqcsddessxsffes

## Web - Serveur 

### HTTP - POST

https://user-images.githubusercontent.com/91114817/193413569-a916fe46-9a57-457d-be91-d41d884c2241.mp4

flag : H7tp_h4s_N0_s3Cr37S_F0r_y0U

### HTTP - Cookies

https://user-images.githubusercontent.com/91114817/193414083-5811a88f-a87d-46e9-9139-97661c833c07.mp4

flag : ml-SYMPA

### HTTP - User-agent

Le challenge avait déjà été utilisé avec l'extension User-Agent Switcher. 

## Exercice 1

A défaut d'avoir abouti à un résultat, voici la méthode que j'ai suivi.

A l'ouverture, j'ai rapidement regardé si le protocole des différents paquets étaient similaires à des paquets pour lesquels j'avais déjà identifié des informations importantes (http, ftp, etc...). L'ensemble des protocoles sont 'USB'. Dans le paquet 2, j'ai trouvé deux informations sur le type de périphérique qui était utilisé dans cette capture. On travaille avec un modèle [AU9540 Smartcard Reader](https://datasheet.lcsc.com/szlcsc/Alcor-Micro-AU9540_C126997.pdf) de l'emtreprise [Alcor Micro Corp](https://www.alcormicro.com/en/).

Sur la page 4 de la [documentation](https://datasheet.lcsc.com/szlcsc/Alcor-Micro-AU9540_C126997.pdf), on peut voir que le modèle de carte pris en charge doit respecter la norme [ISO 7816](https://en.wikipedia.org/wiki/ISO/IEC_7810). 

Le paquet 4 donne quelque caractéristiques techniques : 

![image](https://user-images.githubusercontent.com/91114817/195524513-1d6c0b8f-9880-4359-8156-014514143e23.png)


J'ai ensuite remarqué que les paquets 16 et 28 étaient davantage voluminuex que les autres. J'ai donc décidé de voir ce qu'ils contenaient.

Il semblerait que le paquet 16 corresponde à un transfert de fichiers vidéos.



## Exercice 2
