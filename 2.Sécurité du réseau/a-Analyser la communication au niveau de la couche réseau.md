# Rapport d'incident de cybersécurité : analyse de la communication de la couche réseau
## Scénario

Vous êtes analyste en cybersécurité et travaillez dans une entreprise spécialisée dans la fourniture de services de conseil informatique. Plusieurs clients ont contacté votre entreprise pour signaler qu'ils n'étaient pas en mesure d'accéder au site Web de l'entreprise www.yummyrecipesforme.com et ont vu l'erreur « port de destination inaccessible » après avoir attendu que la page se charge.

Vous êtes chargé d'analyser la situation et de déterminer quel protocole réseau a été affecté lors de cet incident. Pour commencer, vous visitez le site Web et vous recevez également l'erreur « port de destination inaccessible ». Ensuite, vous chargez votre outil d'analyse de réseau, tcpdump, et chargez à nouveau la page Web. Cette fois, vous recevez beaucoup de paquets dans votre analyseur de réseau. L'analyseur montre que lorsque vous envoyez des paquets UDP et recevez une réponse ICMP renvoyée à votre hôte, les résultats contiennent un message d'erreur : « port udp 53 inaccessible ».

![Capture d’écran 2024-09-26 111514](https://github.com/user-attachments/assets/8d88bbd7-3740-4521-9095-0e5efc7e406b)


Dans le journal DNS et ICMP, vous trouverez les informations suivantes :

1. Dans les deux premières lignes du fichier journal, vous voyez la requête sortante initiale de votre ordinateur au serveur DNS demandant l'adresse IP de yummyrecipesforme.com. Cette requête est envoyée dans un paquet UDP.

2. Ensuite, vous trouvez des horodatages qui indiquent quand l'événement s'est produit. Dans le journal, il s'agit de la première séquence de chiffres affichée. Par exemple : 13:24:32.192571. Cela affiche l'heure 13:24, 32.192571 secondes.

3. L'adresse IP source et de destination est la suivante. Dans le journal des erreurs, ces informations sont affichées comme suit : 192.51.100.15.52444 > 203.0.113.2.domain. L'adresse IP à gauche du symbole supérieur à (>) est l'adresse source. Dans cet exemple, la source est l'adresse IP de votre ordinateur. L'adresse IP à droite du symbole supérieur à (>) est l'adresse IP de destination. Dans ce cas, il s'agit de l'adresse IP du serveur DNS : 203.0.113.2.domain.

4. Les deuxième et troisième lignes du journal affichent la réponse à votre paquet de requête ICMP initial. Dans ce cas, la ligne ICMP 203.0.113.2 est le début du message d'erreur indiquant que le paquet ICMP n'a pas pu être délivré au port du serveur DNS.

5. Viennent ensuite le protocole et le numéro de port, qui indiquent quel protocole a été utilisé pour gérer les communications et à quel port il a été délivré. Dans le journal des erreurs, cela apparaît comme : port udp 53 inaccessible. Cela signifie que le protocole UDP a été utilisé pour demander une résolution de nom de domaine à l'aide de l'adresse du serveur DNS sur le port 53. Le port 53, qui correspond à l'extension .domain dans 203.0.113.2.domain, est un port bien connu pour le service DNS. Le mot « inaccessible » dans le message indique que le message n’est pas parvenu au serveur DNS. Votre navigateur n’a pas pu obtenir l’adresse IP de yummyrecipesforme.com, dont il a besoin pour accéder au site Web, car aucun service n’écoutait sur le port DNS de réception, comme l’indique le message d’erreur ICMP « port udp 53 inaccessible ».

Les lignes restantes du journal indiquent que des paquets ICMP ont été envoyés deux fois de plus, mais que la même erreur de livraison a été reçue les deux fois.

Maintenant que vous avez capturé des paquets de données à l'aide d'un outil d'analyse de réseau, il vous appartient d'identifier le protocole et le service réseau qui ont été affectés par cet incident. Vous devrez ensuite rédiger un rapport de suivi.

## Fournir un résumé du problème détecté dans le journal du trafic DNS et ICMP

* Le serveur DNS est en panne car le port 53 est inaccessible. Le paquet de requête ICMP indique que le paquet n'a pas été livré avec succès au port du serveur DNS. <br>
* Comme nous le savons, le port 53 est couramment utilisé pour le DNS. Cela étant dit, le problème le plus probable est que le DNS ne répond pas et cela peut être causé par une attaque DDOS contre le serveur DNS. <br>
* Le protocole UDP révèle que : le DNS ne répond pas. <br>
Cela est basé sur les résultats de l'analyse du réseau, qui montrent que la réponse d'écho ICMP a renvoyé le message d'erreur : au port 53, port UDP 53 inaccessible.
* Le port indiqué dans le message d'erreur est utilisé pour : Serveur DNS <br>
Le problème le plus probable est : le serveur DNS ne répond pas.

## Expliquez votre analyse des données et indiquez au moins une cause de l'incident

| ..... | ..... |
| Heure à laquelle l'incident s'est produit : 13 h 24. | Ces informations ont été obtenues à partir des horodatages du fichier journal. Dans le journal, il s'agit de la première séquence de chiffres affichée : 13:24:32.192571. Cela affiche l'heure 13:24, 32.192571 secondes, avec l'heure au format 24 heures. | <br>
* Expliquez comment l'équipe informatique a pris connaissance de l'incident : le client a signalé à l'entreprise qu'il n'était pas en mesure d'accéder au site Web de l'entreprise. Il a ensuite été signalé que le message sur la page Web était « port inaccessible ». <br>
* Expliquez les mesures prises par le service informatique pour enquêter sur l'incident :
Les ingénieurs de sécurité ont consulté la page Web et ont reçu une erreur « port inaccessible ». L'équipe a utilisé TCPdump (analyseur de réseau) pour voir le trafic réseau autour du site Web. <br>
* Notez les principales conclusions de l'enquête du service informatique (c'est-à-dire les détails relatifs au port affecté, au serveur DNS, etc.) :
Accédez au site Web, puis chargez la page Web tout en surveillant les réseaux via TCPdump. Il a reçu beaucoup de trafic. Envoyé des paquets UDP et reçu une réponse ICMP pour retourner à l'hôte qui indique que le port 53 est inaccessible. <br>
* Notez une cause probable de l'incident :
Déterminez si le port 53 fonctionne ou non. SI c'est le cas, vérifiez le pare-feu. <br>
- Pare-feu : capacité à bloquer le trafic réseau sur des ports spécifiques. Le blocage de port peut être utilisé pour arrêter ou empêcher une attaque. <br>
- DOS : un flot d'informations peut être envoyé au périphérique réseau pour le faire planter ou l'empêcher de fonctionner. Le pirate peut désactiver le serveur DNS à l'aide d'une attaque DOS. Ou quelqu'un au sein de l'organisation peut avoir désactivé le port 53 sur les pare-feu. <br>
