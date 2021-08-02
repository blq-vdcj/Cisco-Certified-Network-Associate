---
title: 20210628 SDN et Controller bases network
created: '2021-06-28T07:26:23.045Z'
modified: '2021-06-28T09:08:43.085Z'
---

# 20210628 SDN et Controller bases network

DATA PLANE ( pris en charge par les asic , les vrai opérations)
````
Routage
commutation
nat
filtrage acl
````

CONTROL PLANE (tout les outils que des protocol utilisent pour comm together)
```
Protocol routage
Arp/icmpv6 ND
STP
MPLS
CDP
```
le control plane donne les informations necessaire ( table de routage etc ) a la couche data plane qui l'execute

MANAGEMENT PLANE ( configuration des equipements)
```
SSH
api Rest
HTTPS
TELNET
net conf
````

petites intelligence mis en commun, pas considerer comme un block 


SBI = south bound interfaces ( pas physique : type api )
interface qui communique avec les couches d'en dessous
controleur a donné des ordres au equipements reseau

unifie la data plane et control plane
````
open-flow : interface de communication entre data plane et controle plane
            crée par l'onf open network foundations

opflex    : proprietaire cisco

````
unigie management plane
````
Cli e snmp      : telnet ssh
````
----------------------------------------------------


Nbi = NorthBound interface  la communication entre les couche dans une api
````
Rest
SSh
````

------------------------------------

Cisco ACI ( data center)
````
systeme proprietaire
APIC = controleur sdn version cisco 
````
Artichitecture
````
spine : colonne

Leaf  : permet les interconnections physique

modele minimal : 2 spine , 1 leaf et un ACI 
````
pas d'interconnection au meme niveau , spine to leaf , leaf to aci 

````
Epg = Endpoint GROUP (groupe dans lesquel on mets des ressources : routeur // server)
Policy =  les regles pour les interractions dans l'epg
````

Fabrique = gros ressources de switchs

-------------------------------

Cisco APIC-EM (version entreprise module)
````
Management plane unifié
on communique avec le control plane
````

Anoncé comme n'etant plus suivis par cisco au moment de la certification 201-300 mais toujours dans le bouqin
Trace path = possibilité de simulé le transfert de données entre les élléments
-----------------------------------------------------------------------------
Comparing traditional versus controller-based network (exemple : yaml + jinga)

````
l'utilisation de script a la place de ligne de commande
avec des api rest
definition de template a la place de CLI
Script >>>>>>>> api > controller >>>>>> device
```






