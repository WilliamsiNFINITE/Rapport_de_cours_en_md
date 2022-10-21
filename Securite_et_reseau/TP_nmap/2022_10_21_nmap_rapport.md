# Résolution d'exercices sur Google Gruyère

Ce rapport documente la réalisation du TP sur nmap et Google Gruyère

## Sommaire

* [Introduction à Nmap](#introduction-a-nmap)
* [Google gruyere](#Google-gruyere)

## Introduction a nmap

Le premier scan a été effectué avec la commande suivante (c'est celle dans le man et le fichier a été sauvegardé et il est disponible [ici](scan_1.txt)): 

```console
$ nmap -A -T4 scanme.nmap.org playground -oN scan_1.txt

```

Puis, j'ai uniquement voulu scanner sans les arguments donnés dans le manuel et j'ai donc lancé la commande suivante (le fichier est disponible [ici](scan_2.txt):

```console
$ nmap scanme.nmap.org -oN scan_2.txt
```

Pour déterminer la version du service, on utilise le paramètre -sV. Le scan est disponible à [cette adresse](scan_3.txt). On peut constater les différentes versions :

```console
PORT     STATE SERVICE    VERSION
80/tcp   open  http       Apache httpd 2.4.7 ((Ubuntu))
443/tcp  open  https?
3128/tcp open  http-proxy Squid http proxy
8080/tcp open  http-proxy Squid http proxy
```

L'exclusion du port 22 se fait avec le paramètre -exclude-ports 22. Le retour est similaire au précédent qui ne contenait déjà pas le port 22.
Le scan UDP est réalisé avec le paramètre -sU et les privilèges root.

```console
Starting Nmap 7.70 ( https://nmap.org ) at 2022-10-21 03:38 EDT
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.0011s latency).
Not shown: 999 open|filtered ports
PORT    STATE SERVICE
123/udp open  ntp

Nmap done: 1 IP address (1 host up) scanned in 25.11 seconds
```

Lorsqu'on scanne avec le paramètre -T0, on se rend compte que le scan est plutôt long est qu'il n'y a pas un débit important de paquets envoyés. L'inverse est visible avec -T5 où l'analyse est rapide et le débit de paquet est très élevé.


## Google gruyere 

Id gruyere : 572500445629261051927482590611010663090

### Cross-site scripting

#### File Upload XSS 

Il est possible d'executer un script en l'uploadant. J'ai crée un fichier html dans lequel j'ai mis :
```html
<i>hello</i>
<script>alert('Alerte')</script>
```

Lorsque l'upload est fait, je peux accéder à mon fichier avec [ce lien : https://google-gruyere.appspot.com/572500445629261051927482590611010663090/bob/tres_mechant_script.html](https://google-gruyere.appspot.com/572500445629261051927482590611010663090/bob/tres_mechant_script.html).

Le pop-up est ainsi affiché :

![image](https://user-images.githubusercontent.com/91114817/197162227-6d0ddb2a-aae7-44e8-a1c2-8559ba64fc93.png)


#### Reflected XSS 

L'objectif est de trouver une URL qui va nous permettre d'executer un script. En ajoutant des caractères aléatoires dans l'url, on se rend compte que ces caractères sont affichés dans la page. 

![image](https://user-images.githubusercontent.com/91114817/197165301-f79a4f72-8506-4070-9ac6-c9744419187a.png)

L'idée est de rajouter dans le lien une commande comme celle que l'on a mise dans l'exercice précédent. Par exemple:

```html
https://google-gruyere.appspot.com/572500445629261051927482590611010663090/%3Ci%3Ehello%3C/i%3E%3Cscript%3Ealert('Alerte')%3C/script%3E
```

#### Store XSS

L'objectif est de stocker un XSS pour qu'un autre utilisateur le voit. On peut utiliser les snippet et en poster un avec le code précédent. 
![image](https://user-images.githubusercontent.com/91114817/197166741-af7cce18-4057-4e9f-9f39-742bc4cc7956.png)

Avec cette méthode, l'alerte n'est pas affichée comme prévue. 

![image](https://user-images.githubusercontent.com/91114817/197166874-2bb78fef-afbd-4da9-aea1-4a5f350eb39a.png)

#### Stored XSS via HTML Attributes

### Path Traversal

#### Information disclosure via path traversal

#### Data tampering via path traversal










