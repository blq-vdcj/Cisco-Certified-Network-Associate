---
attachments: [Clipboard_2021-03-24-10-00-31.png, Clipboard_2021-03-24-10-16-03.png]
title: 20210324 Windows server Sergio
created: '2021-03-24T08:25:03.227Z'
modified: '2021-03-24T09:17:51.224Z'
---

# 20210324 Windows server Sergio


retablir machine virtuel = clic droit retablir ( different d'un point de restauration windows)


## Domain

![](@attachment/Clipboard_2021-03-24-10-00-31.png)

1 : Ajouter un controlleur de domaine a un domaine existant 

    (ajoute un controleur de domaine de secours)   
    [si en dessous de 100 Active directory leger]
    [ au minimum 2 servers de domain en pratique a partir de 100 utilisateurs ]
    [ au minimum 3 servers de domain en pratique a partir de 250 utilisateurs ]
    [ au minimum 4 servers de domain en pratique a partir de 500 utilisateurs ]

2 : Ajouter un nouveau domaine a une foret existante 

    typiquement quand il y a plusieurs entreprises
    ex rachat d'une petite entreprise , et creation d 'un nouvau domaine dans la foret de l'entreprise qui a racheté
    possible aussi de faire la communication entre deux forets
    

3 : Ajouter une foret

    premiere creation d'un arbre ( ne peux exister sans foret)
    root domain name : jeff.lan ( ca vas de paire avec DNS voir screen)

  ![](@attachment/Clipboard_2021-03-24-10-16-03.png)

    mdps = Lolilol123 pour recuperer le domain
