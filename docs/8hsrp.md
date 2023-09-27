# HSRP

## Présentation 

Le protocole **HSRP** (Hot Standby Router Protocol) est un protocole de routage de premier hop redondant (FHRP) conçu pour améliorer la disponibilité et la redondance dans les réseaux locaux (LAN). Son objectif principal est de garantir la disponibilité et **la tolérance aux pannes** dans les environnements réseau en permettant à plusieurs routeurs de travailler ensemble pour créer une passerelle virtuelle commune. Elle assure une haute disponibilité pour le trafic entrant destiné à l'adresse IP virtuelle, en permettant au routeur standby (passif) de prendre automatiquement le relais si le routeur actif échoue. Cela garantit une continuité de service pour les dispositifs connectés au réseau à travers cette interface.

## Mise en place du protocole HSRP

Une fois que tous les routeurs sont opérationnels (qu’ils font bien tous office de passerelle individuellement), nous pouvons passer à la mise en place du HSRP. Sur chaque routeur, il faut maintenant définir une IP (en plus des IP d’interfaces déjà définies) qui sera l’IP du routeur virtuel et une priorité si l’on souhaite changer celle par défaut :

```js
R1_TRS# enable
R1_TRS(config)# interface gi0/0
R1_TRS(config-int)# standby 10 ip 172.28.41.254
R1_TRS(config_int)# standby 10 priority  105
R1_TRS(config_int)# standby 10 preempt
R1_TRS(config_int)# no shut
R1_TRS(config_int)# exit
```
Ne pas oublié "priority" pour définir la priorité entre les deux routeurs. Cela signifie que ce routeur a une priorité de 105, et s'il n'y a pas d'autres routeurs dans le groupe HSRP avec une priorité plus élevée, il deviendra le routeur actif pour ce groupe. J'active la priorité sur R1_TRS il est cablé à la fibre. 

```js
R2_TRS# enable
R2_TRS(config)# interface gi0/0
R2_TRS(config-int)# standby 10 ip 172.28.41.254
R2_TRS(config_int)# standby 10 preempt
R2_TRS(config_int)# no shut
R2_TRS(config_int)# exit
```


Il est préférable de prioriser le routeur connecté à la fibre plutôt que celui lié à l'ADSL dans une configuration de haute disponibilité en raison de la fibre offrant de meilleures performances, un débit plus élevé, une qualité de service supérieure, et une capacité à gérer une charge de trafic plus importante. Cela garantit que les applications essentielles fonctionnent bien lorsque tout va bien, tout en permettant une transition plus fluide vers la connexion ADSL en cas de panne de la fibre. Le choix dépend de vos besoins spécifiques et de votre configuration réseau.