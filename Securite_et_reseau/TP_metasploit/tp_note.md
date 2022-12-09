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

### 3.1.1 Decouverte d'application

Pour trouver les versions d'Apache et de PHP utilisée, on utilise `nmap` avec l'adresse de la machine cible que nous avons d'ores et déjà déterminée grâce à la commande `sudo netdiscover`. 

![image](https://user-images.githubusercontent.com/91114817/206655313-b72c276f-548f-48ee-bf30-18aebed8a789.png)

J'ai ping l'ip 192.168.23.129 et visité l'adresse pour m'assurer d'avoir ciblé la bonne machine. La commande `nmap` dont le retour est ci-dessous indique les versions Apache 2.2.8 (Ubuntu) et PHP 5.2.4

```console
>nmap -A -sV 192.168.23.129

Starting Nmap 7.92 ( https://nmap.org ) at 2022-12-09 08:27 GMT
Nmap scan report for 192.168.23.129
Host is up (0.0023s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.1
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
|_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
53/tcp   open  domain      ISC BIND 9.4.2
| dns-nsid: 
|_  bind.version: 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.10 with Suhosin-Patch)
|_http-server-header: Apache/2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.10 with Suhosin-Patch
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html).
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
|_ssl-cert: ERROR: Script execution failed (use -d to debug)
|_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
|_ssl-date: ERROR: Script execution failed (use -d to debug)
|_sslv2: ERROR: Script execution failed (use -d to debug)
|_tls-alpn: ERROR: Script execution failed (use -d to debug)
| mysql-info: 
|   Protocol: 10
|   Version: 5.0.51a-3ubuntu5
|   Thread ID: 9
|   Capabilities flags: 43564
|   Some Capabilities: SupportsCompression, ConnectWithDatabase, SwitchToSSLAfterHandshake, Speaks41ProtocolNew, Support41Auth, LongColumnFlag, SupportsTransactions
|   Status: Autocommit
|_  Salt: Zfr,"eV^Vuj3]]:G9%K/
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
|_ssl-date: 2022-12-09T08:29:41+00:00; +25s from scanner time.
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
|_http-title: Apache Tomcat/5.5
|_http-favicon: Apache Tomcat
Service Info: Host:  metasploitable.localdomain; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: METASPLOITABLE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   NetBIOS computer name: 
|   Domain name: localdomain
|   FQDN: metasploitable.localdomain
|_  System time: 2022-12-09T03:28:27-05:00
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 1h40m25s, deviation: 2h53m12s, median: 24s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 124.30 seconds


```




J'utilise ensuite la commande `dirb http://192.168.23.129/`
```console
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Dec  9 08:24:41 2022
URL_BASE: http://192.168.23.129/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.23.129/ ----
+ http://192.168.23.129/cgi-bin/ (CODE:403|SIZE:330)                                
+ http://192.168.23.129/index (CODE:200|SIZE:45)                                    
+ http://192.168.23.129/index.html (CODE:200|SIZE:45)                               
+ http://192.168.23.129/phpinfo (CODE:200|SIZE:47577)                               
+ http://192.168.23.129/phpinfo.php (CODE:200|SIZE:47395)                           
+ http://192.168.23.129/server-status (CODE:403|SIZE:335)                           
==> DIRECTORY: http://192.168.23.129/twiki/                                         
                                                                                    
---- Entering directory: http://192.168.23.129/twiki/ ----
==> DIRECTORY: http://192.168.23.129/twiki/bin/                                     
+ http://192.168.23.129/twiki/data (CODE:403|SIZE:332)                              
+ http://192.168.23.129/twiki/index (CODE:200|SIZE:782)                             
+ http://192.168.23.129/twiki/index.html (CODE:200|SIZE:782)                        
==> DIRECTORY: http://192.168.23.129/twiki/lib/                                     
+ http://192.168.23.129/twiki/license (CODE:200|SIZE:19440)                         
==> DIRECTORY: http://192.168.23.129/twiki/pub/                                     
+ http://192.168.23.129/twiki/readme (CODE:200|SIZE:4334)                           
+ http://192.168.23.129/twiki/templates (CODE:403|SIZE:337)                         
                                                                                    
---- Entering directory: http://192.168.23.129/twiki/bin/ ----
+ http://192.168.23.129/twiki/bin/attach (CODE:200|SIZE:4360)                       
+ http://192.168.23.129/twiki/bin/changes (CODE:200|SIZE:21787)                     
+ http://192.168.23.129/twiki/bin/edit (CODE:200|SIZE:5349)                         
+ http://192.168.23.129/twiki/bin/manage (CODE:302|SIZE:0)                          
+ http://192.168.23.129/twiki/bin/passwd (CODE:302|SIZE:0)                          
+ http://192.168.23.129/twiki/bin/preview (CODE:302|SIZE:0)                         
+ http://192.168.23.129/twiki/bin/register (CODE:302|SIZE:0)                        
+ http://192.168.23.129/twiki/bin/save (CODE:302|SIZE:0)                            
+ http://192.168.23.129/twiki/bin/search (CODE:200|SIZE:3550)                       
+ http://192.168.23.129/twiki/bin/statistics (CODE:200|SIZE:534)                    
+ http://192.168.23.129/twiki/bin/upload (CODE:302|SIZE:0)                          
+ http://192.168.23.129/twiki/bin/view (CODE:200|SIZE:10049)                        
+ http://192.168.23.129/twiki/bin/viewfile (CODE:302|SIZE:0)                        
                                                                                    
---- Entering directory: http://192.168.23.129/twiki/lib/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                    
---- Entering directory: http://192.168.23.129/twiki/pub/ ----
+ http://192.168.23.129/twiki/pub/favicon.ico (CODE:200|SIZE:1078)                  
==> DIRECTORY: http://192.168.23.129/twiki/pub/Main/                                
                                                                                    
---- Entering directory: http://192.168.23.129/twiki/pub/Main/ ----
                                                                                    
-----------------
END_TIME: Fri Dec  9 08:25:08 2022
DOWNLOADED: 23060 - FOUND: 26
                                                                                    
```

Ensuite j'utilise la commande `nikto -h http://192.168.23.129/` qui donne : 

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.23.129
+ Target Hostname:    192.168.23.129
+ Target Port:        80
+ Start Time:         2022-12-09 08:29:47 (GMT0)
---------------------------------------------------------------------------
+ Server: Apache/2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.10 with Suhosin-Patch
+ Server may leak inodes via ETags, header found with file /, inode: 67575, size: 45, mtime: Wed Mar 17 14:08:25 2010
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Uncommon header 'tcn' found, with contents: list
+ Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. See http://www.wisec.it/sectou.php?id=4698ebdc59d15. The following alternatives for 'index' were found: index.html
+ Apache/2.2.8 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ PHP/5.2.4-2ubuntu5.10 appears to be outdated (current is at least 7.2.12). PHP 5.6.33, 7.0.27, 7.1.13, 7.2.1 may also current release for each branch.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS, TRACE 
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ Retrieved x-powered-by header: PHP/5.2.4-2ubuntu5.10
+ /phpinfo.php: Output from the phpinfo() function was found.
+ OSVDB-3233: /phpinfo.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information.
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ Cookie PHPSESSID created without the httponly flag
+ /tikiwiki/tiki-graph_formula.php: Output from the phpinfo() function was found.
+ OSVDB-40478: /tikiwiki/tiki-graph_formula.php?w=1&h=1&s=1&min=1&max=2&f[]=x.tan.phpinfo()&t=png&title=http://cirt.net/rfiinc.txt?: TikiWiki contains a vulnerability which allows remote attackers to execute arbitrary PHP code.
+ 8725 requests: 1 error(s) and 18 item(s) reported on remote host
+ End Time:           2022-12-09 08:30:43 (GMT0) (56 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```


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



Le site de [National Vulnerability Database]([https://nvd.nist.gov/vuln/detail/CVE-2006-5702](https://nvd.nist.gov/vuln/detail/CVE-2007-5423)), informe que la vulnérabilité CVE-2007-5423 concerne également TikiWiki et permet aux attaquants de pouvoir exécuter du code arbitraire via des sequences de PHP.

La commande `search CVE-2007-5423` renvoit les informations suivantes 

```console
Matching Modules
================

   #  Name                                             Disclosure Date  Rank       Check  Description
   -  ----                                             ---------------  ----       -----  -----------
   0  exploit/unix/webapp/tikiwiki_graph_formula_exec  2007-10-10       excellent  Yes    TikiWiki tiki-graph_formula Remote PHP Code Execution

```

Plus d'informations peuvent être consultées sur la [page dédiée de la vulnérabilité sur Infosecmatter](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/unix/webapp/tikiwiki_graph_formula_exec)

### 3.1.4 Conclusion

L'exploit CVE-2006-5702 me parit moins important que les deux autres car il a un classement seulement "normal" par rapport à l'exploit "Apache Tomcat Manager Application Deployer Authenticated Code Execution" vu dans le TP précédent et à l'exploit "CVE-2007-5423".

## Etude de la machine moneybox port 80

La machine a pour adresse IP 

Pour connaitre les ports ouvert on utilise `nmap -A -sV  `


## Etude de la machine moneybox port 21
## Etude de la machine moneybox port 22



