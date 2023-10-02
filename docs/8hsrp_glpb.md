# **HSRP & GLPB**

## Présentation 

Le protocole **HSRP** (Hot Standby Router Protocol) est un protocole de routage de premier hop redondant (FHRP) conçu pour améliorer la disponibilité et la redondance dans les réseaux locaux (LAN). Son objectif principal est de garantir la disponibilité et **la tolérance aux pannes** dans les environnements réseau en permettant à plusieurs routeurs de travailler ensemble pour créer une passerelle virtuelle commune. Elle assure une haute disponibilité pour le trafic entrant destiné à l'adresse IP virtuelle, en permettant au routeur standby (passif) de prendre automatiquement le relais si le routeur actif échoue. Cela garantit une continuité de service pour les dispositifs connectés au réseau à travers cette interface.

## **Mise en place du protocole HSRP**

Une fois que tous les routeurs sont opérationnels (qu’ils font bien tous office de passerelle individuellement), nous pouvons passer à la mise en place du HSRP. Sur chaque routeur, il faut maintenant définir une IP (en plus des IP d’interfaces déjà définies) qui sera l’IP du routeur virtuel et une priorité si l’on souhaite changer celle par défaut :

*Routeur 1 :*

```js
R1_TRS# enable
R1_TRS(config)# interface gi0/0
R1_TRS(config-if)# standby 10 ip 172.28.41.254
R1_TRS(config-if)# standby 10 priority  105
R1_TRS(config-if)# standby 10 preempt
R1_TRS(config-if)# no shut
R1_TRS(config-if)# exit
```
Ne pas oublié "priority" pour définir la priorité entre les deux routeurs. Cela signifie que ce routeur a une priorité de 105, et s'il n'y a pas d'autres routeurs dans le groupe HSRP avec une priorité plus élevée, il deviendra le routeur actif pour ce groupe. J'active la priorité sur R1_TRS il est cablé à la fibre. 

*Routeur 2 :*

```js
R2_TRS# enable
R2_TRS(config)# interface gi0/0
R2_TRS(config-if)# standby 10 ip 172.28.41.254
R2_TRS(config-if)# standby 10 preempt
R2_TRS(config-if)# no shut
R2_TRS(config-if)# exit
```


Il est préférable de prioriser le routeur connecté à la fibre plutôt que celui lié à l'ADSL dans une configuration de haute disponibilité en raison de la fibre offrant de meilleures performances, un débit plus élevé, une qualité de service supérieure, et une capacité à gérer une charge de trafic plus importante. Cela garantit que les applications essentielles fonctionnent bien lorsque tout va bien, tout en permettant une transition plus fluide vers la connexion ADSL en cas de panne de la fibre. Le choix dépend de vos besoins spécifiques et de votre configuration réseau.


## **Désactivation du protocole HSRP et mise en place du protocole GLPB**

Le passage de HSRP (Hot Standby Router Protocol) à GLBP (Gateway Load Balancing Protocol) peut être préférable pour améliorer la répartition équilibrée du trafic sur un réseau. GLBP permet une meilleure utilisation des ressources, une répartition de charge plus équitable entre les routeurs actifs, et une gestion plus flexible de la redondance asymétrique. Cette transition peut simplifier la configuration et garantir une haute disponibilité tout en optimisant l'utilisation des équipements réseau.

Il suffit de mettre le préfixe "no" devant les commandes pour les annuler.

*Exemple pour le routeur "R1_TRS" :*

```js
R1_TRS# enable
R1_TRS(config)# interface gi0/0
R1_TRS(config-if)# no standby 10 ip 172.28.41.254
R1_TRS(config-if)# no standby 10 priority  105
R1_TRS(config-if)# no standby 10 preempt
R1_TRS(config-if)# no shut
R1_TRS(config-if)# exit
```

## **Mise en place du protocle GLPB**

Le principe de VIP (Virutal IP) est le même pour les deux protocoles. 

Pour configurer GLBP sur deux routeurs distincts (R1_TRS et R2_TRS) sans attribuer de priorité et en activant la répartition de charge, il faut suivre ces étapes :

Configurez GLBP sur une interface (par exemple, FastEthernet0/0) et attribuez-lui une adresse IP virtuelle.

*Routeur 1 :*

```js
R1_TRS# enable
R1_TRS# configure terminal
R1_TRS(config)# interface gi0/0
R1_TRS(config-if)# glbp 1 ip 172.28.41.254
R1_TRS(config-if)# glbp 1 load-balancing host-dependent
```

Cela configure GLBP sur l'interface GigabiteEthernet0/0 avec l'adresse IP virtuelle 172.28.41.254 pour le groupe GLBP numéro 1 et active la répartition de charge en utilisant l'option "load-balancing" qui permet une répartition des charges entre les 2 routeurs.

Il faut répéter les mêmes étapes sur le routeur R2_TRS, en utilisant la même configuration :

*Routeur 2 :*

```js
R2_TRS# enable
R2_TRS# configure terminal
R2_TRS(config)# interface gi0/0
R2_TRS(config-if)# glbp 1 ip 172.28.41.254
R2_TRS(config-if)# glbp 1 load-balancing host-dependent
```

Après avoir effectué ces configurations sur les deux routeurs, ils fonctionneront ensemble pour fournir la haute disponibilité et la répartition de charge. Les deux routeurs partageront la charge du trafic entrant vers l'adresse IP virtuelle (172.28.41.254) du groupe GLBP 1 sans attribuer de priorité, car GLBP gère automatiquement la répartition de charge entre les routeurs actifs.

## **Explication des commandes GLPB**


R1_TRS(config)# interface gi0/0: Cette commande vous place dans le mode de configuration de l'interface GigabitEthernet0/0 (gi0/0) du routeur R1_TRS. Cela signifie que toutes les commandes suivantes que vous entrez affecteront cette interface spécifique.

R1_TRS(config-if)# glbp 1 ip 172.28.41.254: Cette commande configure GLBP sur l'interface gi0/0 du routeur R1_TRS pour le groupe GLBP numéro 1. Elle attribue l'adresse IP virtuelle 172.28.41.254 à ce groupe GLBP. Toutes les communications destinées à cette adresse IP seront gérées par le routeur actif dans le groupe GLBP.

R1_TRS(config-if)# glbp 1 load-balancing round-robin: Cette commande active la répartition de charge en mode "round-robin" pour le groupe GLBP numéro 1 sur l'interface gi0/0. Le mode "round-robin" répartit équitablement le trafic entrant entre les routeurs actifs dans le groupe GLBP. Chaque routeur actif traite des demandes de manière séquentielle, contribuant ainsi à équilibrer la charge entre les routeurs du groupe.