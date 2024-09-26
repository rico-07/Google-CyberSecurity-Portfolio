
# Rapport d'incident de cybersécurité : renforcement du système d'exploitation
## Scénario

Vous êtes analyste en cybersécurité pour yummyrecipesforme.com, un site Web qui vend des recettes et des livres de cuisine. Un boulanger mécontent a décidé de publier les recettes les plus vendues du site Web pour que le public puisse y accéder gratuitement.
Le boulanger a exécuté une attaque par force brute pour accéder à l'hébergeur Web. Il a saisi à plusieurs reprises plusieurs mots de passe par défaut connus pour le compte administrateur jusqu'à ce qu'il devine correctement le bon. Après avoir obtenu les identifiants de connexion, il a pu accéder au panneau d'administration et modifier le code source du site Web. Il a intégré une fonction JavaScript dans le code source qui invitait les visiteurs à télécharger et à exécuter un fichier lors de leur visite sur le site Web. Après avoir exécuté le fichier téléchargé, les clients sont redirigés vers une fausse version du site Web où les recettes du vendeur sont désormais disponibles gratuitement.
Plusieurs heures après l'attaque, plusieurs clients ont envoyé un e-mail au service d'assistance de yummyrecipesforme. Ils se sont plaints que le site Web de l'entreprise les avait invités à télécharger un fichier pour mettre à jour leurs navigateurs. Les clients ont affirmé qu'après avoir exécuté le fichier, l'adresse du site Web a changé et que leurs ordinateurs personnels ont commencé à fonctionner plus lentement.
En réponse à cet incident, le propriétaire du site Web essaie de se connecter au panneau d'administration mais n'y parvient pas. Il contacte donc le fournisseur d'hébergement du site Web. Vous et d'autres analystes en cybersécurité êtes chargés d'enquêter sur cet événement de sécurité.
Pour résoudre l'incident, vous créez un environnement sandbox pour observer le comportement suspect du site Web. Vous exécutez l'analyseur de protocole réseau tcpdump, puis saisissez l'URL du site Web, yummyrecipesforme.com. Dès que le site Web se charge, vous êtes invité à télécharger un fichier exécutable pour mettre à jour votre navigateur. Vous acceptez le téléchargement et autorisez l'exécution du fichier. Vous constatez ensuite que votre navigateur vous redirige vers une URL différente, greatrecipesforme.com, conçue pour ressembler au site d'origine. Cependant, les recettes que votre entreprise vend sont désormais publiées gratuitement sur le nouveau site Web. Les journaux montrent le processus suivant :
1. Le navigateur demande une résolution DNS de l'URL yummyrecipesforme.com
2. Le DNS répond avec l'adresse IP correcte
3. Le navigateur lance une requête HTTP pour la page Web
4. Le navigateur lance le téléchargement du logiciel malveillant
5. Le navigateur demande une autre résolution DNS pour greatrecipesforme.com
6. Le serveur DNS répond avec la nouvelle adresse IP
7. Le navigateur lance une requête HTTP vers la nouvelle adresse IP <br>

Un analyste senior confirme que le site Web a été compromis. L'analyste vérifie le code source du site Web. Il remarque qu'un code JavaScript a été ajouté pour inciter les visiteurs du site Web à télécharger un fichier exécutable. L'analyse du fichier téléchargé a révélé un script qui redirige les navigateurs des visiteurs de yummyrecipesforme.com vers greatrecipesforme.com.
L'équipe de cybersécurité signale que le serveur Web a été touché par une attaque par force brute. Le boulanger mécontent a pu deviner le mot de passe facilement car le mot de passe administrateur était toujours défini sur le mot de passe par défaut. De plus, aucun contrôle n'était en place pour empêcher une attaque par force brute.
Votre travail consiste à documenter l'incident en détail, notamment en identifiant les protocoles réseau utilisés pour établir la connexion entre l'utilisateur et le site Web. Vous devez également recommander une action de sécurité à prendre pour empêcher les attaques par force brute à l'avenir.


## Réponse:
### Partie 1 : Identifier le protocole réseau impliqué dans l'incident

Le protocole impliqué dans l'incident est le protocole de transfert hypertexte (HTTP). Étant donné que le problème concernait l'accès au serveur Web de yummyrecipesforme.com, nous savons que les requêtes adressées aux serveurs Web pour les pages Web impliquent du trafic http.

### Partie 2 : Documenter l'incident

*Plusieurs clients ont contacté le service d'assistance du site Web en déclarant que lorsqu'ils ont visité le site Web, ils ont été invités à télécharger et à exécuter un fichier contenant l'accès à de nouvelles recettes. Leurs ordinateurs personnels fonctionnent lentement depuis lors. Le propriétaire du site Web a essayé de se connecter au serveur Web, mais a remarqué que son compte était bloqué.
 
*L'analyste en cybersécurité a utilisé un environnement sandbox pour ouvrir le site Web sans affecter le réseau de l'entreprise. Ensuite, l'analyste a exécuté tcpdump pour capturer les paquets de trafic réseau produits par l'interaction avec le site Web. L'analyste a été invité à télécharger un fichier prétendant qu'il donnerait accès à des recettes gratuites, a accepté le téléchargement et l'a exécuté. Le navigateur a ensuite redirigé l'analyste vers un faux site Web (greatrecipesforme.com).
 
*L'analyste en cybersécurité a inspecté le journal tcpdump et a observé que le navigateur avait initialement demandé l'adresse IP du site yummyrecipesforme.com. Une fois la connexion avec le site Web établie via le protocole HTTP, l'analyste s'est rappelé avoir téléchargé et exécuté le fichier. Les journaux ont montré un changement soudain dans le trafic réseau lorsque le navigateur a demandé une nouvelle adresse IP pour l'URL greatrecipesforme.com. Le trafic réseau a ensuite été redirigé vers la nouvelle adresse IP du site greatrecipesforme.com.
 
*Le professionnel senior en cybersécurité a analysé le code source des sites Web et le fichier téléchargé. L'analyste a découvert qu'un attaquant avait manipulé le site Web pour ajouter du code qui invitait les utilisateurs à télécharger un fichier malveillant déguisé en mise à jour du navigateur. Étant donné que le propriétaire du site Web a déclaré qu'il avait été exclu de son compte administrateur, l'équipe pense que l'attaquant a utilisé une attaque par force brute pour accéder au compte et modifier le mot de passe administrateur. L'exécution du fichier malveillant a compromis les ordinateurs des utilisateurs finaux.
 
 ### Partie 3 : Recommander une solution pour les attaques par force brute

1. Étant donné que la vulnérabilité qui a conduit à cette attaque était la capacité de l'attaquant à utiliser un mot de passe par défaut pour se connecter, il est important d'empêcher que d'anciens mots de passe tels que les mots de passe par défaut soient utilisés pour réinitialiser le mot de passe.

2. Une autre mesure de soutien consiste à exiger des mises à jour de mot de passe plus fréquentes, de sorte qu'au cas où une personne non autorisée prendrait connaissance du mot de passe, elle serait moins susceptible de pouvoir utiliser ce mot de passe si le mot de passe était mis à jour plus tôt que plus tard.

3. Enfin, une autre solution utile consiste à mettre en œuvre l'authentification à deux facteurs (2FA). 
