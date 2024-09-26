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

* En raison du message de réponse d'erreur ICMP concernant le port 53, il est très probable que le serveur DNS ne réponde pas. Cette hypothèse est également étayée par les indicateurs associés au message UDP sortant et à la récupération du nom de domaine. <br>
* Dans le cadre du protocole DNS, le protocole UDP a été utilisé pour contacter le serveur DNS afin de récupérer l'adresse IP du nom de domaine de yummyrecipesforme.com. Le protocole ICMP a été utilisé pour répondre avec un message d'erreur, indiquant des problèmes de contact avec le serveur DNS. <br>


## Expliquez votre analyse des données et indiquez au moins une cause de l'incident

| Analyse | Explication |
| :---- | :--- |
| Heure à laquelle l'incident s'est produit : <br> 13 h 24. | Ces informations ont été obtenues à partir des horodatages du fichier journal. Dans le journal, il s'agit de la première séquence de chiffres affichée : 13:24:32.192571. Cela affiche l'heure 13:24, 32.192571 secondes, avec l'heure au format 24 heures. |
| Expliquez comment l'équipe informatique a pris connaissance de l'incident : <br> Les clients ont informé l'organisation qu'ils ont reçu le message « port de destination inaccessible » lorsqu'ils ont tenté de visiter le site Web yummyrecipesforme.com. | Le scénario indique que « une poignée de clients ont contacté votre entreprise pour signaler qu'ils n'étaient pas en mesure d'accéder au site Web de l'entreprise et ont vu l'erreur « port de destination inaccessible » après avoir attendu que la page se charge. » |
| Expliquez les mesures prises par le service informatique pour enquêter sur l'incident : <br> L'équipe de cybersécurité fournissant des services informatiques à son organisation cliente enquête actuellement sur le problème afin que les clients puissent à nouveau accéder au site Web. | Le scénario stipule que « cet incident, entre-temps, est traité par les ingénieurs de sécurité. » |
| Notez les principales conclusions de l'enquête du service informatique : <br> Le constat est que le port DNS 53 était inaccessible après détection des parquets à l'aide de TCPDUMP. | Fournit un récapitulatif concis de ce que vous avez fait pour enquêter sur le problème. Le scénario indique : « Vous visitez le site Web et vous recevez également l'erreur « port de destination inaccessible ». Ensuite, vous chargez votre outil d'analyse de réseau, tcpdump, et chargez à nouveau la page Web. Cette fois, vous recevez un grand nombre de paquets dans votre analyseur de réseau. Dans l'analyseur, vous envoyez des paquets UDP et recevez une réponse ICMP pour retourner à l'hôte. Les résultats contiennent un message d'erreur : « port udp 53 inaccessible ». » |
| Notez une cause probable de l'incident : <br> Le serveur DNS peut être en panne en raison d'une attaque par déni de service réussie ou d'une mauvaise configuration. | L'objectif d'une attaque DoS est d'envoyer un flot d'informations à un périphérique réseau, comme un serveur DNS, pour le faire planter ou l'empêcher de répondre au trafic réseau légitime. Il est possible qu'un attaquant ait désactivé le serveur DNS avec une attaque DoS. Il est également possible qu'un membre de l'équipe ait modifié la configuration du pare-feu et bloqué le port 53. |
