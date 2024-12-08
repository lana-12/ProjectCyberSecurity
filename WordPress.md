# Wordpress Standalone Non Protégé
*Port :[ 185.34.102.254](https://)*

## 1. TestS et Scan

###  * Scanner avec Nmap
#### - Tests effectués
##### 1. Scan des portes ouvertes :
nmap 185.34.102.254

Nmap scan report for 185.34.102.254
Host is up (0.036s latency).
Not shown: 995 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
443/tcp  open  https
4848/tcp open  appserv-http
8443/tcp open  https-alt


##### 2. Scan des services et versions :
nmap -sV -p- 185.34.102.254

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-12-06 10:14 EST
Stats: 0:14:56 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 16.23% done; ETC: 11:46 (1:17:06 remaining)
Stats: 0:22:48 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 24.80% done; ETC: 11:46 (1:09:09 remaining)
Stats: 0:54:11 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 72.24% done; ETC: 11:29 (0:20:49 remaining)
Stats: 1:04:26 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 95.30% done; ETC: 11:22 (0:03:11 remaining)
Nmap scan report for 185.34.102.254
Host is up (0.0011s latency).
Not shown: 34804 filtered tcp ports (net-unreach), 30574 filtered tcp ports (no-response)
PORT      STATE SERVICE    VERSION
22/tcp    open  tcpwrapped
80/tcp    open  tcpwrapped
443/tcp   open  tcpwrapped
665/tcp   open  tcpwrapped
1141/tcp  open  tcpwrapped
1480/tcp  open  tcpwrapped
1620/tcp  open  tcpwrapped
1917/tcp  open  tcpwrapped
2959/tcp  open  tcpwrapped
3153/tcp  open  tcpwrapped
3811/tcp  open  tcpwrapped
4357/tcp  open  tcpwrapped...

##### 3. Scan spécifiques de WordPress :
nmap -sS -p 80,443 185.34.102.254

PORT     STATE SERVICE
80/tcp   open  http
443/tcp  open  https



##### 4. Scan des vulnérabilités :
nmap --script vuln 185.34.102.254

Nmap scan report for 185.34.102.254
Host is up (0.035s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http

Nmap done: 1 IP address (1 host up) scanned in 153.50 seconds


#### - Résultats
- Ports ouverts :
 - 22/tcp (SSH) :Le service SSH est actif. Ce port permet l'accès à distance sécurisé et l'exécution de commandes via le protocole SSH.
 - 80/tcp (HTTP) :Le service HTTP est actif, utilisé pour servir des pages web non sécurisées (protocole HTTP).
 - 443/tcp (HTTPS) :Le service HTTPS est actif, utilisé pour des connexions web sécurisées via SSL/TLS.
 - 4848/tcp (appserv-http) :Ce port est généralement associé à l'administration de serveurs d'applications.
 - 8443/tcp (https-alt) : Ce port est utilisé comme une alternative au port 443 pour le trafic HTTPS. Il peut servir à des applications spécifiques ou à des interfaces d'administration.

#### - Recommandations :
- Restreindre l'accès aux ports sensibles (4848, 8443) avec des règles de pare-feu.
- Activer HTTPS uniquement pour les services web (80 et 443).


### *Scanner avec HTTrack
httrack http://185.34.102.254 -O wordpressProtect

Mirror laucnched
mirroring http://185.34.102.254 with the wizard help..
Done.185.34.102.254/ (84753 bytes) -OK

==> l'outil HTTrack a bien réussi à copier le contenu du site à l'adresse http://185.34.102.254 dans un dossier local nommé wordpressProtect



## 2. Analyses
##### a. Ports ouverts détectés :
Les scans Nmap ont permis d'identifier les ports ouverts, les services en cours d'exécution, leurs versions et les éventuelles vulnérabilités associées.

-22/tcp (SSH) : Ce port est utilisé pour accéder au serveur à distance de manière sécurisée. S'il est mal configuré, il peut être exploité pour des attaques de force brute.
- Risque : Moyen à élevé (selon la configuration de sécurité du service SSH).
- Observation : Le service SSH est détecté comme actif. Il n'est pas précisé si l'accès est restreint par des listes d'adresses IP autorisées.

-80/tcp (HTTP) : Ce port sert à l'accès aux sites web en clair (non sécurisé). Il peut être sujet à des attaques.
  - Risque : Élevé si les connexions HTTP ne sont pas redirigées vers HTTPS.
  - Observation : Le port est ouvert, ce qui indique la présence d'un serveur web.

-443/tcp (HTTPS) : Ce port est utilisé pour des connexions chiffrées au serveur web. Sa sécurité dépend du certificat SSL/TLS utilisé.
  - Risque : Faible si la configuration est correcte (certificat SSL valide, protocole sécurisé).
  - Observation : Le port est actif

-4848/tcp (appserv-http) : Ce port est souvent utilisé pour l'administration de serveurs d'applications. Il permet des actions de configuration et de déploiement.
  - Risque : Élevé, car un attaquant peut exploiter une interface d'administration mal protégée.
  - Observation : Ce port est ouvert et accessible, ce qui peut exposer le serveur à des risques de compromission par des attaquants.

-8443/tcp (https-alt) : Ce port est une alternative au port 443 pour les connexions HTTPS. Il est souvent utilisé par des interfaces d'administration ou des services web spécifiques.
  - Risque : Élevé si l'authentification et le chiffrement ne sont pas bien configurés.
  - Observation : Ce port est utilisé comme alternative au port 443. 

##### b. Vulnérabilités détectées :
- La commande nmap --script vuln n'a pas renvoyé d'informations de vulnérabilités sur les ports détectés, mais cela ne signifie pas qu'il n'en existe pas.

- Le service SSH (port 22) est une cible potentielle pour des attaques par force brute.

##### c. Analyse de l'attaque HTTrack

- Le contenu a été copié avec succès.
- L'outil a copié un total de 84 753 octets (environ 85 Ko) du site, ce qui signifie que les fichiers (HTML,log.txt, images, cookies) sont accessibles sans restriction.

- Le log mentionne que des informations sensibles peuvent être présentes dans les fichiers hts-log.txt ou dans le dossier hts-cache. 

## 3. Préconisations 
-Sécurisation des ports : Restreindre l'accès aux ports sensibles (4848, 8443) via un pare-feu et limiter l'accès au SSH (port 22) aux IP de confiance.

-Renforcement des services web : Forcer l'utilisation de HTTPS (443) et désactiver HTTP (80) pour sécuriser les connexions.

-Masquage des services : Configurer des bannières de service personnalisées pour éviter de révéler des informations sur les versions des services (tcpwrapped).

-Protection du site web : Empêcher le mirroring du site avec des outils comme HTTrack.

