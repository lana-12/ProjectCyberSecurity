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
4357/tcp  open  tcpwrapped


##### 3. Scan spécifiques de WordPress (WPScan-like) :
nmap --script http-wordpress-enum --script-args host=185.34.102.254

##### 4. Scan des vulnérabilités :
nmap --script vuln 185.34.102.254

Nmap scan report for 185.34.102.254
Host is up (0.035s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-vuln-cve2014-3704: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-aspnet-debug: ERROR: Script execution failed (use -d to debug)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
443/tcp  open  https
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-aspnet-debug: ERROR: Script execution failed (use -d to debug)
8443/tcp open  https-alt
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
|_ssl-ccs-injection: No reply from server (TIMEOUT)
|_http-phpself-xss: ERROR: Script execution failed (use -d to debug)
|_http-vuln-cve2014-3704: ERROR: Script execution failed (use -d to debug)

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

