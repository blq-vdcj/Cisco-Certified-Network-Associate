---
title: eigrp STEVE
created: '2021-06-01T10:10:07.539Z'
modified: '2021-06-01T14:08:49.106Z'
---

# EIGRP ( Enhanced Interior Gateway Routing Protocol )

EIGRP (Enhanced Interior Gateway Routing Protocol) est un protocole de routage développé par Cisco. Cet article reprend les bases de son fonctionnement et de sa configuration.

## Introduction
EIGRP est un protocole de routage de type « distance vector » (vecteur de distance) avancé (ou hybride selon les points de vue). Bien que son fonctionnement global ressemble très fort à un protocole de type « distance vector » il dispose d’une série de caractéristiques que l’on retrouve par exemple dans OSPF qui est un « link state protocol » (protocole d’état des lien) comme l’établissement de relations d’adjacence.

---

## Caractéristiques
* Supporte le VLSM (Variable Length Subnet Mask), c’est donc un protocole de routage « classless ».
* Fonctionne sur base de l’algorithme DUAL pour une sélection efficace des routes tout en évitant les boucles.
* Relations d’adjacence avec les routeurs voisins.
* Transmissions des messages en multicast (224.0.0.10) et unicast
* Supporte plusieurs protocoles de la couche réseau: IPv4, IPv6, AppleTalk, IPX, …
* Load-balancing et notamment sur routes ayant des métriques différentes.
* « Summarization » à n’importe quel endroit du réseau.
* « Auto-summarization », *par défaut*, entre réseaux majeurs (entre deux réseaux classfull).
* Echange des messages entre routeurs assuré par RTP (Reliable Transfer Protocol).
* Métrique tenant compte de la bande passante et du délai des interfaces. D’autres paramètres peuvent être configurés en plus ( fiabilité et charge de l’interface).
* Distance administrative pour les routes internes: 90 (valeur par défaut)
* Distance administrative pour les routes externes: 170 (valeur par défaut)

---

## Fonctionnement
Tout d’abord, EIGRP fonctionne sur base d’un numéro de système autonome ( « Autonomous System Number » ou « ASN »). C’est-à-dire qu’il pourra uniquement communiquer avec les routers où EIGRP est configuré pour le même ASN.

Ensuite, une fois qu’on l’a activé sur une interface, que ce soit de manière dynamique ou statique, EIGRP tente de découvrir des voisins potentiels pour celà il y envoi des message « HELLO ».
Lorsque deux routeurs reçoivent des messages HELLO l’un de l’autre, ils vérifient alors les conditions d’adjacence afin de décider si oui ou non ils deviendront des voisins EIGRP (neighbors).

Pour que deux routeurs deviennent voisins EIGRP il doivent remplir les conditions suivantes:

* Fonctionner dans le même AS (Autonomous System), donc être configuré avec le même ASN.
* Les deux routeurs doivent pouvoir s’envoyer et recevoir des packets IP.
* Les interfaces doivent être configurées avec une adresse IP dans le même subnet.
* L’interface concernée ne doit pas être configurée comme passive.
* Les valeurs K (valeurs qui définissent le calcul de la métrique) doivent correspondre.
* L’authentification EIGRP (si configurée) doit être passée avec succès.
Si ces différentes conditions sont vérifiées, les deux routeurs se considèrent alors comme voisins EIGRP, ajoutent cette relation dans leur table de voisinage, et commencent à s’échanger des informations.

Lorsqu’une relation de voisins vient de s’établir, chaque routeur commence par envoyer la totalité de ses routes connues pour lesquelles il a une interface active ET configurée dans EIGRP. Par la suite, seules les modifications seront envoyées.

Afin de garantir une certain stabilité, les routeurs s’échangent en permanence des messages HELLO. Ces messages HELLO sont envoyés à intervalles réguliers et ont une durée de vie. Si un des deux routeurs n’a pas reçu de nouveau HELLO avant que la durée de vie du précédent soit écoulée, le routeur voisin est considéré comme défaillant, l’adjacence est rompue et les routes reçues par ce voisin sont retirées de la table de routage.

Chaque routeur garde en mémoire toutes les informations sur les routes reçues de ses voisins et il les stocke dans sa table de topologie. EIGRP utilise ensuite l’algorithme DUAL pour sélectionner la meilleure route vers chaque sous-réseau, calcule la métrique à y associer et place le résultat dans sa table de routage.

---

## Les paquets EIGRP
* **HELLO**:    Formation et maintient des adjacences
* **UPDATE**:   Transmissions des mises à jour de topologie entre les routeurs
* **QUERY**:    Résolution des problèmes de topologie lors de la disparition d'un réseau
* **REPLY**:    Réponse aux *QUERIES*
* **ACK**:      Acquittement des paquets *UPDATE*, *QUERY* et *REPLY*.

---

## La métrique
```
M = 256 * ( ( K1 * BW + ( K2 * BW ) / ( 256 - LOAD ) + K3 * DELAY ) * K5 / (K4 + RELIABILITY) )

    K1, K2, K3, K4, K5 : Coefficients K
    BW = 10^7 / PPBP
    LOAD : charge de l'interface
    DELAY : Somme des délais (10aines de µs)
    RELIABILITY : Fiabilité de l'interface
``` 
Par défaut, EIGRP calcule la métrique d’un subnet en tenant compte de la bande passante et du délai des interfaces (K1=1, K2=0, K3=1, K4=0, K5=0).

La métrique avec les paramètre par défaut se calcule comme suit:
```
M = 256 * ( BW + DELAY )
    BW = 10^7 / PPBP
    DELAY : Somme des délais (10aines de µs)
```

---

## « Feasible Distance » et « Reported Distance »
La **Feasible Distance** (FD) est la métrique pour un subnet du point de vue du routeur lui-même, utilisée pour choisir la meilleure route vers ce subnet.

La **Reported Distance** (RD) est la métrique pour un subnet du point de vue du routeur voisin. (La métrique annoncée par le routeur voisin).

Lorsque deux routeurs s’échangent leur topologie, ils envoient des messages de type « Update » contenant une série d’information ( subnet concerné, délai, bande passante, charge de l’interface, fiabilité de l’interface, MTU et nombre de sauts).

Le routeur qui reçoit l’information l’intègre dans sa table de topologie et ensuite calcule la RD du subnet avec ces informations et la FD pour ce subnet.

---

## « Successor » et « Feasible Successor « 
Une des particularité d’EIGRP réside dans le fait qu’il garde dans sa table de topologie toutes les informations reçues de ses voisins, y compris les routes qui n’ont pas été intégrée dans la table de routage. C’est là qu’on rencontre les termes « Successor  » et « Feasible Successor ».

Quand plusieurs routes possibles existent, le « Successor » est la route qui a la plus petite métrique vers le subnet … donc qui a la plus petite « Feasible Distance ». Le routeur qui annonce cette route sera alors le « next-hop », le prochain saut vers ce subnet. C’est cette route qui sera placée dans la table de routage.

Dans certaines conditions, d’autres routes peuvent être considérés comme valides sans pour autant être les meilleures. Ce sont les « Feasible Successors ». Ces routes ne sont, dans une configuration par défaut, pasp lacée dans la table de routage. Toutefois, si le « Successor » actuel devait tomber, EIGRP irait alors directement chercher une alternative dans sa table de topologie parmi les « Feasible Successors ».

Pour qu’une route soit considérée comme « Feasible Successor » il faut que la RD de cette route soit strictement inférieur à la FD de la meilleure route.  

### Exemple:
(les valeurs FD et RD sont ici simplifiées et non réalistes afin de rendre la compréhension plus aisée).
Un routeur a, dans sa topolgie, trois routes vers le subnet 192.168.0.0 / 24:

Route n°1 : via 10.0.0.1, RD=1000, FD=2000  
Route n°2 : via 10.0.1.1, RD=1500, FD=2750  
Route n°3 : via 10.0.2.1, RD=2500, FD=3000  

la Route n°1 sera le « Successor », puisque c’est la plus petite FD.  
la Route n°2 sera un « Feasible Successor » car RD(route2) < FD(route1)  
la Route n°3 ne sera pas un « Feasible Successor », RD(coute3) > FD(route1)  

## Que se passe-t-il lors d’un accident sur le réseau ?
Il arrive bien entendu qu’une routeur perde une liaison pour une raison x ou y. Que ce soit à cause d’une interface défaillante, d’un voisin qui ne répond plus, … EIGRP va tenter de trouver une solution…

1. EIGRP analyse sa topologie. Si un ou plusieurs « Feasible Successor » existent, il remplace la route par celle parmi les FS ayant la plus petite FD.
2. Si EIGRP n’a pas de FS dans sa topologie, il entre dans un processus de requêtes (*QUERY*/*REPLY*). Il envoi des requêtes à ses voisins à la recherche d’une nouvelle route vers le subnet perdu. Ces voisins, s’ils n’ont pas de solution immédiate à proposer envoient à leur tour des requêtes à leurs propre voisins … etc.  

Autant dire que la présence d’un FS dans la topologie permet d’améliorer sensiblement la convergence du réseau.

---

## Configuration de base

### Création du processus EIGRP
```
router(config)# router eigrp <asn>
```
*asn: n° de système autonome qui doit être commun à tous les routeurs de la topologie*

### Intégration des interfaces dans le processus EIGRP
```
router(config-router)# network <netword-id> [wildcard mask]
```

### (Dés)Activation de l'auto-summary
```
router(config-router)# [no] auto-summary
```


### Définition des interfaces passives
```
router(config-router)# [no] passive-interface <default|interface>
```

### Configuration de la variance
```
router(config-router)# variance <n>
```

### Modification des valeurs K
```
router(config-router)# metric weights 0 <k1> <k2> <k3> <k4> <k5>
```

### Création d'un summary
```
router(config-if)# ip summary-address eigrp <asn> <network-id> <mask> [AD]
```
*asn: n° de système autonome*  
*AD: Distance administrative*  

### Définition de la bande passante maximum utilisée par EIGRP
```
router(config-if)# ip bandwidth-percent egrip <asn> <n>
```
*asn: n° de système autonome*  
*n: pourcentage de la bande passante de l'interface*  

### Modifications de l'intervale et de la durée de vie des paquets HELLO
```
router(config-if)# ip hello-interval eigrp <asn> <sec>
router(config-if)# ip hold-time eigrp <asn> <sec>
```

## Vérifications

### Vérifications générales des protocoles de routages actifs
```
router# show ip protocols
```

### Vérifications des adjacences EIGRP
```
router# show ip eigrp neighbors
```

### Vérifications des interfaces active dans EIGRP
```
router# show ip eigrp interfaces
```

### Vérifications de la topologie EIGRP
```
router# show ip eigrp topology
```

### Propgation route par defaut
```
R1(config-router)#redistribute static metric 1544 2000 255 1 1500
```

