---
title: '20210701 SDA fabric, underlay, overlay'
created: '2021-07-01T07:33:01.211Z'
modified: '2021-07-01T08:07:15.348Z'
---

# 20210701    SDA fabric, underlay, overlay

VxLAN ( virtual xtended lan)
```
encapsulé en UDP (peut etre rooté dans une infra)
ressemble a GRE (mais protocol different)
serie d'equipement compatible ( car besoin de la puissance des asic pr faire le travail)

```

Underlay
```
tunnel vxlan entre deux layer3 pour acheminer des données d'un coté a l'autre
```

Police
```
SGT = scalable group tag
pas de probleme de chevauchement avec les police comparé au access-list
```


Cas de figure quand le Dna n'est pas possible
```
brownfield = archi en production
on ne gere pas le deploiement de la fabrique avec le dna : [def : UNDERLAY]
on mets en place la fabrique et on ne gere avec dna center que la partie utilisation  ( groupe,police , vxlan)
```




