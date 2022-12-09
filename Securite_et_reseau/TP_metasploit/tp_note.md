# Machines vulnérables et Metasploit
Ce rapport documente la réalisation du TP noté sur Metasploit

## Sommaire

* [Questions](#Questions)
* [Etude d'application web](#Etudes-d-application-web)
* [Etude de la machine MoneyBox - Port 80](#Etude-de-la-machine-moneybox-port-80)
* [Etude de la machine MoneyBox - Port 21](#Etude-de-la-machine-moneybox-port-21)
* [Etude de la machine MoneyBox - Port 22](#Etude-de-la-machine-moneybox-port-22)

## Questions

1 - 

2 - Metasploit est écrit en Ruby

3 - L'utilisation pricnipale de MetaSploit est de créer et éxecuter des exploits pour des vulnérabilités connues

4 - Un meterpreter est un payload qui permet à l'attaquant d'éxecuter des commandes à distance

5 - Quand on parle de payload, on parle d'un extrait de code donné à la machine cible avec l'exploit

6 - Un exploit module est un extrait de code prêt à l'emploi qui permet de tirer profit d'une vulnérabilité connue

7 - Avec nmap on peut réaliser des scans TCP ou UDP

8 - 

## 3.1 Etudes d application web

### 3.1.2 Exploitation de la vulnérabilité 

Sur le site de [National Vulnerability Database](https://nvd.nist.gov/vuln/detail/CVE-2006-5702), on trouve que la vulnérabilité CVE-2006-5702 concerne TikiWiki et  permet aux attaquants de pouvoir obtenir des informations compromettantes (login MySQL) grace à plusieurs fichiers php. 

Afin d'en apprendre plus sur cette vulnérabilité, on peut aller sur notre terminal et entrer les commandes

```console
>msfconsole
>search tikiwiki

Matching Modules
================

   #  Name                                             Disclosure Date  Rank       Check  Description
   -  ----                                             ---------------  ----       -----  -----------
   0  exploit/unix/webapp/php_xmlrpc_eval              2005-06-29       excellent  Yes    PHP XML-RPC Arbitrary Code Execution
   1  exploit/unix/webapp/tikiwiki_upload_exec         2016-07-11       excellent  Yes    Tiki Wiki Unauthenticated File Upload Vulnerability
   2  exploit/unix/webapp/tikiwiki_unserialize_exec    2012-07-04       excellent  No     Tiki Wiki unserialize() PHP Code Execution
   3  auxiliary/admin/tikiwiki/tikidblib               2006-11-01       normal     No     TikiWiki Information Disclosure
   4  exploit/unix/webapp/tikiwiki_jhot_exec           2006-09-02       excellent  Yes    TikiWiki jhot Remote Command Execution
   5  exploit/unix/webapp/tikiwiki_graph_formula_exec  2007-10-10       excellent  Yes    TikiWiki tiki-graph_formula Remote PHP Code Execution

```

Après réflexion, la commande `search CVE-2006-5702` est plus appropriée car elle renvoit directement 

```console
Matching Modules
================

   #  Name                                Disclosure Date  Rank    Check  Description
   -  ----                                ---------------  ----    -----  -----------
   0  auxiliary/admin/tikiwiki/tikidblib  2006-11-01       normal  No     TikiWiki Information Disclosure

```

[La page dédiée à cette vulnérabilité sur Infosecmatter](https://www.infosecmatter.com/metasploit-module-library/?mm=auxiliary/admin/tikiwiki/tikidblib) Donne davantage d'informations sur la vulnérabilité.



## Etude de la machine moneybox port 80
## Etude de la machine moneybox port 21
## Etude de la machine moneybox port 22



