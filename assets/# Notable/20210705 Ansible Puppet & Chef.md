---
title: 20210705 Ansible Puppet & Chef
created: '2021-07-05T07:12:11.140Z'
modified: '2021-07-05T09:50:53.545Z'
---

# 20210705 Ansible Puppet & Chef

drift configuration 
```
meme base mais evolue differement
configuration directement des routers // plus multiple users qui travaillent dessus
risque de derives
versions alterer des configuration dans le temps
```
centralized Configuration Files dans Version Control`( moyen bon marcher de gerer ca)
```
push sur git des cfg , verification possible
Centralisation des configs
Fonctionne tres bien quand on attaque les modifications sur le serveur de fichier
fonctionne moins bien quand on cfg directement sur les routeurs
```
Configuration Monitoring & Enforcement
```
Point de reference de la configuration qui devient systeme de monitoring de la configuration
```

Configuration Provisioning ( permet de gerer les problemes de deploiement)
```
templates avec variables de config (propre a l'equipement)[Toujours cohérent]
fichier de type YAML (pour stocker les variables)
ou jingja2   Ex :    " {{  variable  }} " [Format de template]
```



------------------------------------------------------------------

Ansible [SANS AGENT]
```
Logiciel qui nous permets de gerer la configuration d equipement

Inventaire = zone que l'on veux gerer ( possibilité de regroupement ex tt des switch tt des routeurs et cfg ses regroupement comme 1 entité)
Templates = avoir une running config avec des variable dedans
Variables = fichier qui contient les config du matos 

Playbook = parametre de script en YAML (liste de taches a executer)[gerer config, interfaces, etc]
```

Netconf = present que sur des equipement datacenter (équivalent au ssh mais en forme d'api)

Vault = bibliotheque de mdps peut etre crypté



Puppet  [AVEC UN AGENT TOUJOURS , interne ou externe]
```
Avoir un agent qui tourne (genre api)[ssh ou api rest pour push la configuration]
Possible d'utiliser un agent externe

Puppet master ( manifeste = defini comment les choses doivent etre faites)[ex une recette de cuisine]
ressource classe et module 
Template ( informations de structure de données et avec des systeme de variable)
```

Chef
```
presque comme puppet , mais avec sa propre terminologie
Cookbook = livre de recette
dans le cookbook il y a les receipe = choses qu'on veux pousser sur les equipement
dans la recette y'a des ingrédients = elements qu'on doit mettre dans la cfg
Runlist = defini quelle sont les recette et dans quel ordre 
```
-----------------------------------------------------------------

RECAP
```
Ansible :
Playbook - SSH/NETCONF - Agentless - PUSH
```
```
Puppet :
Manifest - http(rest) - Agent - Pull
```
```
Chef :
Recipe/Runlist - Http(rest) - Agent - Pull
```
-------------------------------------------------------------------

Exercices GNS ansible 2021 07 05 - RDA

