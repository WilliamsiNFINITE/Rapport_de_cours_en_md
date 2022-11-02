# Machines vullnérables et Metasploit
Ce rapport documente la réalisation du TP sur Metasploit

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



## Etude d une machine libre

