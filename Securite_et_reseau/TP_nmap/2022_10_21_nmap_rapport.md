
# Résolution d'exercices de CTF

Ce rapport documente la réalisation du TP sur nmap

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

