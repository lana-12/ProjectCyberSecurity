
# Projet Springboot

*Cible : "ipAdress:9898"

## 1. TestS et Scan

###  * Scan avec SQLMAP
commande utilisé : sqlmap -u "http://00.00.00.0:9898/api/clients" --batch (on peut augmenter risk(max =3) et level(max=5))

On constate un premier Warning indiquant que l'URL indiqué ne contient pas de paramètre 'get' (qui est généralement indiqué par "?=1" dans l'url)
Un deuxième warning nous indique que le paramètre "#1" n'est pas dynamique, ce qui nous indique que les injections dans la base de donnée se font en statique.
Et à la fin, on a un CRITICAL disant que tous les paramètres testés n'aparaient pas être injectables
Ce test nous indique que le site est protégé contre les injections sql
##### 2. scan avec  Nikto :
+ Vulnerability 1 : The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type.
Cette vulnérabilité potentielle est liée à la sécurité des en-têtes HTTP. Si cet en-tête est absent, un navigateur peut essayer de deviner (sniffer) le type MIME d'un fichier. Un fichier prétendu être un texte brut pourrait être interprété comme JavaScript, ce qui pourrait exécuter un code malveillant.
Solution : Configurez le serveur web pour inclure cet en-tête avec la valeur nosniff : Header set X-Content-Type-Options "nosniff"

+  Vulnerability 2 : HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
Cette vulnérabilité indique un problème potentiel lié à la méthode HTTP PUT et à l'en-tête Allow. Si la méthode PUT est activée sur un serveur web sans restriction appropriée, les clients malveillants peuvent télécharger des fichiers arbitraires (scripts malveillants, logiciels malveillants, etc.) et prendre du serveur en téléchargeant un script permettant l'execussion de commandes. 
Solution : Désactiver 'put' si elle n'est pas nécessaire :  <Limit PUT> Deny from all </Limit>

+  Vulnerability 3 : HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
Cette vulnerabilité indique un risque lié à l'activation de la méthode HTTP DELETE sur un serveur web. Si la méthode DELETE est autorisée sans restrictions appropriées, les utilisateurs malveillants peuvent supprimer des fichiers ou des données importantes directement depuis le serveur. Les suppressions accidentelles ou mal intentionnées pourraient entraîner des interruptions de service ou des pertes de données.
Solution : Désaciver la méthode 'DELETE' si celle ci n'est pas nécessaire : <Limit DELETE> Deny from all </Limit>


##### 3. Scan avec Wapiti :
Dans le rapport généré par le test de Wapiti,  on a un somaire de catégories de vulnérabilités et le nombre de vulnérabilités trouvée dans chaque catégorie. Trois de ces catégories ont des valeurs non nulles de nombre de vulnérabilités : 

+ Content Security Policy Configuration : La Content Security Policy (CSP) est un en-tête HTTP conçu pour protéger les applications web contre des attaques comme le Cross-Site Scripting (XSS) ou l'injection de code malveillant. Si une CSP est malconfigurée ou absente, ceci permet au attaquants d’injecter des scripts malveillants dans le navigateur de la victime. le navigateur autorisera l’exécution de scripts provenant de n’importe quelle source. Les ressources (scripts, images, iframes) provenant de domaines compromis ou malveillants peuvent être exécutées dans l’application web. Aussi, sans restrictions sur les styles (style-src), un attaquant pourrait injecter du CSS malveillant, modifiant l’apparence ou le comportement d’un site.
- Solution : Configurer une CSP robuste (Définir les sources autorisées pour les scripts, styles, images) :
Content-Security-Policy: 
    default-src 'self'; 
    script-src 'self' https://trusted-scripts.com; 
    style-src 'self'; 
    img-src 'self' data:;

+ HTTP Secure Headers : Les HTTP Secure Headers sont des en-têtes HTTP utilisés pour renforcer la sécurité des applications web en configurant des comportements spécifiques pour les navigateurs. Lorsqu’ils sont absents, mal configurés, ou désactivés, ils peuvent exposer une application à plusieurs vulnérabilités.
- SOlution : Définir des HTTP secure headers :
1- Strict-Transport-Security (HSTS): Force les navigateurs à se connecter uniquement en HTTPS, même si l’utilisateur tape http://, si celui-ci n'est pas configuré, les utilisateurs peuvent être redirigés vers des versions non sécurisées du site, exposant leurs données sensibles.
2- X-Frame-Options : Empêche l’intégration d’une page web dans un iframe non autorisé, prévenant ainsi le clickjacking (Un attaquant peut intégrer votre site dans un iframe malveillant et inciter les utilisateurs à cliquer sur des éléments invisibles ou faux, effectuant des actions involontaires.).
3- Referrer-Policy : Contrôle les informations envoyées dans l'en-tête Referer, limitant les données exposées aux sites tiers. Si celui-ci n'est pas configuré, des informations sensibles sur l’utilisateur ou l’application (comme des tokens) peuvent être partagées avec des sites tiers via des URL référentes.
4- Permissions-Policy (anciennement Feature-Policy) : contrôle l’accès aux fonctionnalités du navigateur (caméra, micro, géolocalisation, etc.) afin de protéger la vie privée des utilisateurs.

+ Secure Flag cookie : Le Secure Flag est une directive qui garantit que les cookies sont envoyés uniquement via des connexions sécurisées (HTTPS), Lorsqu'un cookie a le Secure Flag, il ne sera jamais transmis sur des connexions HTTP non chiffrées. Dans le cas contraire, un cookie peut être transmis sur une connexion HTTP non sécurisée, où les données circulent en texte clair. Cela permet à un attaquant de capturer le cookie via des attaques Man-in-the-Middle (MITM), ce qui peut conduire aux vols de sessions d'utilisateurs ou bien à un accès non autorisé à des comptes ou à des informations sensibles.
- Solution : Il faut s'assurer que tous les cookies sensibles, comme les cookies de session, ont l'attribut Secure activé.

