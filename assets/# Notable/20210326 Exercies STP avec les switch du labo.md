# 20210326 Exercies STP avec les switch du labo


![](./assets/img/Clipboard_2021-03-25-16-13-11.png)


    EDGE = PORTFAST

    bonne pratique :

    spannning-tree bpduguard enable des que spanning-tree portfast activé
    ( resultat si switch brancher interface devient er-dissabled)






##### ***Spanning-tree rootguard*** ( pas forcement lié a un port d'acces )

    empeche un switch malconfiguré de se proposer comme root ( il est ignoré )
    
***attention*** pas mettre ca entre switch primaire et secondaire



-----------------------------------------------------------------------------------------------------------------

##### ***A ne pas confondre avec bpdu guard***

    -- bpdu filter (filtrer les bpdu)   filtrer = ignorer     ***!!!! attention peut creer des gros problemes !!!!***
    peut etre activer globalement ou par interface.

    bonne pratique a mettre en extremité


------------------------------------------------------------------------------------------------------------------

##### ***Activation global***


    Uplink fast      = GARDE L'INFORMATION EN MEMOIRE
    configurer sur les extremités, ceux qui ont des uplinks vers la topologie 
    memorise quel interface amene un autre chemin vers le root


    Backbone fast    = GARDE INFORMATION EN MEMOIRE
    configurer sur les switch de la **distrib***
    memorise quel sont les chemin secondaire vers le roots




