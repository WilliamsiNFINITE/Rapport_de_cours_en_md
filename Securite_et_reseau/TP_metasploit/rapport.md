# Machines vullnérables et Metasploit
Ce rapport documente la réalisation du TP sur Metasploit

Procédure : 
- On isole la machine
- On isole le service

## Sommaire

* [Introduction rapide a Metasploit](#Introduction-rapide-a-Metasploit)
* [Installation des machines virtuelles](#Installation-des-machines-virtuelles)
* [Pentest de la machine Metasploitable 1](#Pentest-de-la-machine-Metasploitable-1)
* [Etude d'une machine libre](#Etude-d-une-machine-libre)

## Introduction rapide a Metasploit
### Question 1

Le nombre d'exploit mis a disposition est trouvé avec la commande **banner**:

```console
msf6 > banner
# cowsay++
 ____________                                                                                                                                                  
< metasploit >                                                                                                                                                 
 ------------                                                                                                                                                  
       \   ,__,                                                                                                                                                
        \  (oo)____                                                                                                                                            
           (__)    )\                                                                                                                                          
              ||--|| *                                                                                                                                         
                                                                                                                                                               

       =[ metasploit v6.2.9-dev                           ]
+ -- --=[ 2230 exploits - 1177 auxiliary - 398 post       ]
+ -- --=[ 867 payloads - 45 encoders - 11 nops            ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: To save all commands executed since start up 
to a file, use the makerc command
```

On y trouve donc 2230 exploits.

### Question 2

La commande **help** donne une aide concernant les commandes disponibles. Une partie est intéressante : 

```console
Module Commands
===============

    Command       Description
    -------       -----------
    advanced      Displays advanced options for one or more modules
    back          Move back from the current context
    clearm        Clear the module stack
    favorite      Add module(s) to the list of favorite modules
    info          Displays information about one or more modules
    listm         List the module stack
    loadpath      Searches for and loads modules from a path
    options       Displays global options or for one or more modules
    popm          Pops the latest module off the stack and makes it active
    previous      Sets the previously loaded module as the current module
    pushm         Pushes the active or list of modules onto the module stack
    reload_all    Reloads all modules from all defined module paths
    search        Searches module names and descriptions
    show          Displays modules of a given type, or all modules
    use           Interact with a module by name or search term/index

``` 
J'ai "naïvement" executé la commande **info Apache**, qui ne donne pas de résultat.
La commande **listm** permet d'afficher une liste de module. J'ai essayé de l'utiliser afin de pouvoir avoir le nom des modules Apache. La console me dit que le module stack est vide. 
J'ai ensuite utilisé la commande **search Apache** qui renvoit dans le terminal une liste de 110 modules.

### Quesiton 3

**search MySQL** donne une liste de 32 modules.

### Quesiton 4 

**search buffer overflow** donne 753 modules.

### Question 5

```console
msf6 > info auxiliary/server/capture/mysql     

       Name: Authentication Capture: MySQL
     Module: auxiliary/server/capture/mysql
    License: Metasploit Framework License (BSD)
       Rank: Normal

Provided by:
  Patrik Karlsson <patrik@cqure.net>

Available actions:
  Name     Description
  ----     -----------
  Capture  Run MySQL capture server

Check supported:
  No

Basic options:
  Name        Current Setting                           Required  Description
  ----        ---------------                           --------  -----------
  CAINPWFILE                                            no        The local filename to store the hashes in Cain&Abel format
  CHALLENGE   112233445566778899AABBCCDDEEFF1122334455  yes       The 16 byte challenge
  JOHNPWFILE                                            no        The prefix to the local filename to store the hashes in JOHN format
  SRVHOST     0.0.0.0                                   yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
  SRVPORT     3306                                      yes       The local port to listen on.
  SRVVERSION  5.5.16                                    yes       The server version to report in the greeting response
  SSL         false                                     no        Negotiate SSL for incoming connections
  SSLCert                                               no        Path to a custom SSL certificate (default is randomly generated)

Description:
  This module provides a fake MySQL service that is designed to 
  capture authentication credentials. It captures challenge and 
  response pairs that can be supplied to Cain or JtR for cracking.

```

### Question 6

Dans la commande précédente on voit que le port utilisé est le port 3306. Ce port est aussi trouvé avec la commande **options**

### Question 7

Les commandes obligatoires sont indiquées avec le champ required (CHALLENGE, SRVHOST, SRVPORT, SRVVERSION) 

### Question 8 

On peut avoir accès au code avec la commande **edit** (ou depuis le site [Exploit Database](https://www.exploit-db.com/) ?).


## Installation des machines virtuelles

Check 

## Pentest de la machine Metasploitable 1

Identifiant de connexion : msfadmin msfadmin

### Question 9

OS : Ubuntu (**lsb_release -a** depuis la machine cible)

### Question 10

Depuis la machine virtuelle cible : **ifconfig** --> 192.168.23.129

### Question 11 

Depuis la VM attaquante : **sudo netdiscover**
```console
Currently scanning: 192.168.77.0/16   |   Screen View: Unique Hosts                
                                                                                    
 4 Captured ARP Req/Rep packets, from 3 hosts.   Total size: 240                    
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.23.1    00:50:56:c0:00:01      2     120  VMware, Inc.                     
 192.168.23.129  00:0c:29:7c:2b:2e      1      60  VMware, Inc.                 <------ VM Cible     
 192.168.23.254  00:50:56:ed:73:6b      1      60  VMware, Inc.                     


```

### Question 12

On utilise nmap avec la commande **nmap -A -sV 198.168.23.129** (c'est un peu long)

```console
┌──(hoarauwi㉿kali)-[~]
└─$ nmap -A  -sV  198.168.11.44
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-02 09:35 GMT
Nmap scan report for 198.168.11.44
Host is up (0.017s latency).
Not shown: 995 filtered tcp ports (no-response), 2 filtered tcp ports (host-unreach)
PORT     STATE SERVICE    VERSION
80/tcp   open  http?
443/tcp  open  https?
8080/tcp open  http-proxy Squid http proxy
|_http-server-header: squid

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 250.01 seconds

```

### Question 13

```console
┌──(hoarauwi㉿kali)-[~]
└─$ nmap 198.168.11.44 -A -T5 
...
PORT     STATE SERVICE    VERSION
80/tcp   open  http-proxy Squid http proxy
8080/tcp open  http-proxy Squid http proxy
...

```

On utilise la commande vue en question 2 **search Samba** : 25 différents exploit ont été trouvés.

L'OS de la metaploitable est Ubuntu 8.04

### Question 14

Cette version de l'OS d'Ubuntu esst sortie en avril 2008. Nous allons donc nous intéresser aux vulnérabilités de l'année 2007.

```console

#   Name                                                 Disclosure Date  Rank       Check  Description
-   ----                                                 ---------------  ----       -----  -----------
8   exploit/multi/samba/usermap_script                   2007-05-14       excellent  No     Samba "username map script" Command Execution
17  exploit/linux/samba/lsa_transnames_heap              2007-05-14       good       Yes    Samba lsa_io_trans_names Heap Overflow
18  exploit/osx/samba/lsa_transnames_heap                2007-05-14       average    No     Samba lsa_io_trans_names Heap Overflow
19  exploit/solaris/samba/lsa_transnames_heap            2007-05-14       average    No     Samba lsa_io_trans_names Heap Overflow

```

### Question 15

La vulnérabilité que nous allons exploiter s'appelle [Samba "username map script" Command Execution - Metasploit](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/multi/samba/usermap_script). 

Options: 
HttpPassword : tomcat
HttpUsername : tomcat
PATH : /manager
SSL : false

### Question 16

L'exploit retourne une erreur avec les options ci dessus.

### Question 17

[lien](https://www.syloe.com/glossaire/apache-tomcat/#:~:text=Apache%20Tomcat%20est%20un%20logiciel,de%20d%C3%A9ployer%20les%20servlets%20Java.)
Apache Tomcat est un logiciel de serveur d’applications web open source conçu pour la programmation en Java et développé et maintenu par Jakarta, le groupe de projets open source Java de la fondation Apache.

L’objectif initial du logiciel Apache Tomcat est d’héberger et de déployer les servlets Java.

### Question 18 

Exploit : [https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/multi/http/tomcat_mgr_deploy](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/multi/http/tomcat_mgr_deploy)

On aurait pû utiliser Apache Tomcat User Enumeration.

Payload : [Windows Meterpreter (Reflective Injection), Reverse TCP Stager - Metasploit](https://www.infosecmatter.com/metasploit-module-library/?mm=payload/windows/meterpreter/reverse_tcp)

## Question 19

exploit

## Etude d une machine libre

### Question 20

D'abord on va isoler la machine. On va donc essayer de voir si elle est affichée avec la commande **sudo netdiscover**

```console
Currently scanning: 192.168.52.0/16   |   Screen View: Unique Hosts                                               
                                                                                                                   
 4 Captured ARP Req/Rep packets, from 3 hosts.   Total size: 240                                                   
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.141.133 00:0c:29:1d:24:3a      2     120  VMware, Inc.                                                    
 192.168.141.2   00:50:56:f0:77:77      1      60  VMware, Inc.                                                    
 192.168.141.1   00:50:56:c0:00:08      1      60  VMware, Inc.                                                    


```

Je vérifie que l'IP (192.168.141.1) est accessible avec un ping. J'ai ping mais ce n'est pas fructueux. En tout cas, en vu des informations, on va partir du principe que c'est la bonne machine. 

A posteriori (et avec les indications du profs, en rentrant 192.168.141.1 dans mon navigateur), je me rends compte que ce n'est pas la bonne machine.
Par contre avec la machine 192.168.141.2, j'arrive à **ping**. De même pour l'ip 192.168.141.133

Dans mon navidateur en entrant 192.168.141.133 on arrive sur la page suivante :

![image](https://user-images.githubusercontent.com/91114817/202677490-f1b935e8-1bcb-40e5-aa86-757dacf90ba9.png)

Je fais ensuite un **nmap -A -sV -sC ip_machine_cible** pour voir les ouvertures.

```console
┌──(hoarauwi㉿kali)-[~/Desktop/folder test]
└─$ nmap -A -sV  192.168.141.2 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-18 10:00 GMT
Nmap scan report for 192.168.141.2
Host is up (0.00021s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE    SERVICE VERSION
53/tcp filtered domain

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.49 seconds

```

```console
┌──(hoarauwi㉿kali)-[~/Desktop/folder test]
└─$ nmap -A -sV  192.168.141.133 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-18 10:12 GMT
Nmap scan report for 192.168.141.133
Host is up (0.00080s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 7f:55:2d:63:a8:86:4f:90:1f:05:3c:c9:9f:40:b3:f2 (RSA)
|   256 e9:71:11:ed:17:fa:48:06:a7:6b:5b:b6:0e:1b:11:b8 (ECDSA)
|_  256 db:74:42:c4:37:c3:ae:a0:5c:30:26:cb:1a:ef:76:52 (ED25519)
80/tcp open  http    Apache httpd 2.4.41
| http-ls: Volume /
| SIZE  TIME              FILENAME
| 31K   2020-12-01 11:23  skeylogger
|_
|_http-title: Index of /
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.66 seconds

```

Je vais donc voir ce que je peux faire avec le fichier skeylogger que j'ai téléchargé depuis l'url 192.168.141.133.

J'execute **cat skeylogger** je j'obtiens :

```console
┌──(hoarauwi㉿kali)-[~/Downloads]
└─$ cat skeylogger
ELF>�!@�q@8
           @#"@@@h�����   ��00088�=�M�M�
�
�=�M�M����DDP�td�4�4�4��Q�tdR�td�=�M�M▒▒/lib64/ld-linux-x86-64.so.2GNU�"�,�#���A��▒����kߔGNU���e�m�;� RWl�.x QXL�5� 
                                                                                                                    ^� �e��"exitfopendaemonpopenstrdup__assert_failprintffget
```

Donc à la place faut utiliser **string**

```console
┌──(hoarauwi㉿kali)-[~/Desktop/folder test]
└─$ strings skeylogger
...
Could not determine keyboard device file
ZHJhY2FyeXMK
Usage: skeylogger [OPTION]
Logs pressed keys
  -h, --help            Displays this help message
  -v, --version         Displays version information
  -l, --logfile         Path to the logfile
  -d, --device          Path to device file (/dev/input/eventX)
Simple Key Logger version 0.0.1

...
```

En décodant ZHJhY2FyeXMK en base64, on trouve le mot de passe : dracarys.
On peut se connecter avec **ssh daenerys@192.168.141.133**

Je commence avec **ls -a**. Ensuite je visite le **.bash_history** (pour voir les commandes faites)
```console
daenerys@osboxes:~$ cat .bash_history 
cd .local/
ls
cd share/
ls
...
```

Je visite le répertoire qui a été visité avant.
Je tombe sur une archive que je dezip
Je **cat** le fichier dézipé et il me donne le mot de passe pour le root (khaldrogo). Ensuite je me donne les privilèges root
**su root** password : kahldrogo


