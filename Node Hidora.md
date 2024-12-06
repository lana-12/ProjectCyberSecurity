
# Node Hidora : Projet Big Data

*Cible : node179686-env-1839015-etudiant-d02-01.sh1.hidora.com*

## 1. Test et Scan

###  Scan avec Nmap
#### Tests effectués
##### 1. Scan réseau et détection des services :
nmap scan  node179686-env-1839015-etudiant-d02-01.sh1.hidora.com 

Nmap scan report for node179686-env-1839015-etudiant-d02-01.sh1.hidora.com (45.86.36.79)
Host is up (0.035s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 72.13 seconds

##### 2. Analyse approfondie des services :
nmap -sV -p 22,80 45.86.36.79


PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
80/tcp open  http    Jetty 6.1.26

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.04 seconds


##### 3. Scan agressif pour plus de détails :
nmap -A 45.86.36.79

Nmap scan report for 45.86.36.79
Host is up (0.028s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 12:82:29:ec:6a:ee:db:76:71:4f:e6:c7:ec:ca:dc:68 (RSA)
|   256 8d:fe:d8:5e:7a:0c:d6:87:ae:02:ac:e1:ad:61:1c:82 (ECDSA)
|_  256 e8:1d:07:84:5e:df:e2:44:7b:96:37:05:b3:4f:bc:65 (ED25519)
80/tcp open  http    Jetty 6.1.26
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Hadoop Administration
|_http-server-header: Jetty(6.1.26)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 116.16 seconds

##### 4. Recherche de vulnérabilités connues

nmap --script vuln -p 22,80 45.86.36.79

Nmap scan report for 45.86.36.79
Host is up (0.031s latency).

PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.

Nmap done: 1 IP address (1 host up) scanned in 510.85 seconds

##### 5. Analyse complète 
nmap -Pn -A -T4 -p- node179686-env-1839015-etudiant-d02-01.sh1.hidora.com

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-12-06 06:03 EST
Stats: 0:29:06 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 25.66% done; ETC: 07:56 (1:24:18 remaining)
Stats: 0:37:02 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 32.43% done; ETC: 07:57 (1:17:07 remaining)
Stats: 0:51:47 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 49.81% done; ETC: 07:47 (0:52:11 remaining)
Stats: 0:54:53 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 76.19% done; ETC: 07:15 (0:17:09 remaining)
Nmap scan report for node179686-env-1839015-etudiant-d02-01.sh1.hidora.com (45.86.36.79)
Host is up (0.0019s latency).
Not shown: 35695 filtered tcp ports (net-unreach), 29361 filtered tcp ports (no-response)
PORT      STATE SERVICE    VERSION
22/tcp    open  tcpwrapped
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp    open  tcpwrapped
614/tcp   open  tcpwrapped
623/tcp   open  tcpwrapped
668/tcp   open  tcpwrapped
735/tcp   open  tcpwrapped
952/tcp   open  tcpwrapped
1088/tcp  open  tcpwrapped
1149/tcp  open  tcpwrapped
1266/tcp  open  tcpwrapped
1294/tcp  open  tcpwrapped
1646/tcp  open  tcpwrapped
1729/tcp  open  tcpwrapped
1747/tcp  open  tcpwrapped
1890/tcp  open  tcpwrapped
2044/tcp  open  tcpwrapped
2245/tcp  open  tcpwrapped
2260/tcp  open  tcpwrapped
2398/tcp  open  tcpwrapped
2402/tcp  open  tcpwrapped
2457/tcp  open  tcpwrapped
2573/tcp  open  tcpwrapped
3032/tcp  open  tcpwrapped
3074/tcp  open  tcpwrapped
3197/tcp  open  tcpwrapped
3366/tcp  open  tcpwrapped.....

##### 6. Analyse de la Sécurité d'OpenSSH 8.0 avec Searchsploit
* Recherche des exploits connus dans la base de données Exploit-DB.
searchsploit openssh 8.0

Exploits: No Results
Shellcodes: No Results


- "No Results" de searchsploit indique qu'aucun exploit connu spécifique à OpenSSH 8.0 n'est répertorié dans la base de données Exploit-DB.
- OpenSSH 8.0 est relativement sécurisé.


#### Resultats :
998 ports TCP filtrés : Ils n'ont pas répondu à la requête Nmap (probablement bloqués par un pare-feu).
2 ports TCP ouverts : 22 et 80.

* Port 22/tcp (SSH)
Service : SSH (Secure Shell), un protocole permettant une connexion sécurisée à distance.
* Port 80/tcp (HTTP)
Service : Serveur HTTP avec Jetty 6.1.26, une implémentation légère et performante utilisée notamment pour des applications web.
Titre de la page web : Hadoop Administration. Cela suggère que l'hôte est un serveur Hadoop 
* Méthodes HTTP détectées :
TRACE : Méthode potentiellement risquée car elle pourrait être exploitée pour des attaques de type Cross-Site Tracing (XST). Cette méthode est généralement désactivée pour des raisons de sécurité.
En-tête HTTP : Le serveur indique clairement qu'il utilise Jetty(6.1.26).

-  D'aprés l'analyse complete de scan nmap, plusieurs ports sont ouverts sur l'hôte node179686-env-1839015-etudiant-d02-01.sh1.hidora.com, mais ces ports sont tous marqués comme "tcpwrapped". Cela signifie qu'ils sont protégés par un mécanisme de sécurité qui enveloppe les connexions TCP, ce qui empêche l'identification claire du service fonctionnant sur chaque port.
- tcpwrapped : Cela signifie que les ports ouverts ont été détectés mais que nmap n'a pas pu identifier quel service ou application les utilise, à cause d'une protection de type "wrapper" qui intercepte les connexions avant qu'elles n'atteignent le service.

### Conclusion :
Ces scan révèlent un accès SSH ouvert (port 22) : Cela pourrait permettre une connexion distante sécurisée si les identifiants sont connus.
- Un serveur HTTP vulnérable (port 80) : L'utilisation de la méthode HTTP TRACE pourrait poser des risques.
- Jetty 6.1.26 est une version relativement ancienne (publiée en 2010), ce qui augmente les risques liés à des vulnérabilités non corrigées.
- 998 ports filtrés : Cela peut indiquer un pare-feu actif ou une politique restrictive au niveau réseau.

### Recommandations
Sécuriser le serveur SSH :Limiter l'accès à des adresses IP spécifiques
Mettre à jour Jetty :Passer à une version plus récente ou envisager une solution alternative pour corriger d'éventuelles vulnérabilités.
Désactiver les méthodes HTTP inutilisées, notamment TRACE.
Analyser les configurations de pare-feu :S'assurer que seuls les ports nécessaires sont ouverts.




### Scan avec HTTrack
#### 1.Téléchargement du site:
httrack http://node179686-env-1839015-etudiant-d02-01.sh1.hidora.com -O ./projectbigdata_copy

Mirror launched on Fri, 06 Dec 2024 08:09:41 by HTTrack Website Copier/3.49-5 [XR&CO'2014]
mirroring http://node179686-env-1839015-etudiant-d02-01.sh1.hidora.com with the wizard help..
Done.: node179686-env-1839015-etudiant-d02-01.sh1.hidora.com/Popover requires tooltip.js (1396 bytes) - 404

#### 2. Téléchargement du site sans le fichier .js :
httrack http://node179686-env-1839015-etudiant-d02-01.sh1.hidora.com -O ./projectbigdata_copy  -*.js

Mirror launched on Fri, 06 Dec 2024 08:21:37 by HTTrack Website Copier/3.49-5 [XR&CO'2014]
mirroring http://node179686-env-1839015-etudiant-d02-01.sh1.hidora.com -*.js with the wizard help..
Done.e179686-env-1839015-etudiant-d02-01.sh1.hidora.com/logs/yarn--resourcemanager-hadoop-master.out.2 (1524 bytes) - OK


### Analyse de fichiers récupérés avec Foremost



## 2. Analyses


## 3. Préconisations 