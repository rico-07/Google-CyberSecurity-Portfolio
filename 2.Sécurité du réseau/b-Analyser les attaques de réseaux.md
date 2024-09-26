
# Rapport d'incident de cybersécurité : analyse des attaques réseau
## Scénario

Vous travaillez comme analyste de sécurité pour une agence de voyages qui fait la promotion de ses ventes et promotions sur le site Web de l’entreprise. Les employés de l’entreprise accèdent régulièrement à la page Web de vente de l’entreprise pour rechercher des forfaits vacances susceptibles d’intéresser leurs clients.

Un après-midi, vous recevez une alerte automatique de votre système de surveillance indiquant un problème avec le serveur Web. Vous essayez de visiter le site Web de l’entreprise, mais vous recevez un message d’erreur de dépassement de délai de connexion dans votre navigateur.

Vous utilisez un analyseur de paquets pour capturer les paquets de données en transit vers et depuis le serveur Web. Vous remarquez un grand nombre de requêtes TCP SYN provenant d’une adresse IP inconnue. Le serveur Web semble être submergé par le volume de trafic entrant et perd sa capacité à répondre au nombre anormalement élevé de requêtes SYN. ​​Vous soupçonnez que le serveur est attaqué par un acteur malveillant.

Vous mettez le serveur hors ligne temporairement afin que la machine puisse récupérer et revenir à un état de fonctionnement normal. Vous configurez également le pare-feu de l’entreprise pour bloquer l’adresse IP qui envoyait le nombre anormal de requêtes SYN. Vous savez que votre solution de blocage d’adresses IP ne durera pas longtemps, car un attaquant peut usurper d’autres adresses IP pour contourner ce blocage. Vous devez alerter rapidement votre responsable de ce problème et discuter des prochaines étapes pour arrêter cet attaquant et empêcher que ce problème ne se reproduise. Vous devrez être prêt à informer votre patron du type d’attaque que vous avez découvert et de la manière dont elle affecte le serveur Web et les employés.

## Identifier le type d'attaque qui peut avoir provoqué cette interruption du réseau

* Une explication possible du message d'erreur de dépassement de délai de connexion du site Web est une attaque DoS. Les journaux montrent que le serveur Web cesse de répondre après avoir été surchargé de demandes de paquets SYN. ​​Cet événement pourrait être un type d'attaque DoS appelé inondation SYN.

## Partie 2 : Expliquez comment l'attaque provoque le dysfonctionnement du site Web
Lorsque les visiteurs du site Web tentent d'établir une connexion avec le serveur Web, une négociation à trois se produit à l'aide du protocole TCP. La négociation se compose de trois étapes :

1. Un paquet SYN est envoyé de la source à la destination, demandant la connexion.
2. La destination répond à la source avec un paquet SYN-ACK pour accepter la demande de connexion. La destination réservera des ressources pour que la source puisse se connecter.
3. Un dernier paquet ACK est envoyé de la source à la destination, reconnaissant l'autorisation de se connecter. Dans le cas d'une attaque SYN flood, un acteur malveillant envoie un grand nombre de paquets SYN en même temps, ce qui surcharge les ressources disponibles du serveur pour la connexion. Lorsque cela se produit, il ne reste plus de ressources serveur pour les demandes de connexion TCP légitimes. Les journaux indiquent que le serveur Web est surchargé et qu'il est incapable de traiter les demandes SYN des visiteurs. Le serveur ne peut pas ouvrir une nouvelle connexion aux nouveaux visiteurs qui reçoivent un message de dépassement de délai de connexion.
