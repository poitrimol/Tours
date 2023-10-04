# **Port mirroring**

## Présentation 

Le port mirroring est une fonctionnalité des commutateurs réseau qui permet de copier le trafic d'un ou plusieurs ports sources vers un port de destination, généralement connecté à un outil d'analyse tel que Wireshark. Cette technique est largement utilisée pour surveiller, dépanner, analyser et optimiser le trafic réseau. En configurant le port mirroring, les administrateurs peuvent capturer des trames réseau à partir d'un firewall, par exemple, pour effectuer une analyse approfondie à l'aide de Wireshark. Cela permet de détecter des anomalies, d'identifier des problèmes de performances, de renforcer la sécurité et d'optimiser le réseau, offrant ainsi une visibilité précieuse sur le trafic réseau en temps réel.

## Mise en place du port mirroring

- Étape 1 : Attribuer le VLAN "Transport-routeur" (229) au port destination

```js
sw_Tours(config)# interface GigabitEthernet0/23
sw_Tours(config-if)# switchport access vlan 229
sw_Tours(config-if)# switchport mode access
sw_Tours(config-if)# exit
```

- Étape 2 : Copier le trafic du port GigabitEthernet 0/21 (connecté au firewall) dans les deux sens ("both") vers le port GigabitEthernet 0/23 

```js
sw_Tours(config)# monitor session 1 source interface GigabitEthernet0/1 both
sw_Tours(config)# monitor session 1 destination interface GigabitEthernet0/23
```

- Étape 3 : Faire un test

Brasser un poste (ici le poste M2_P3) au port GigabitEthernet 0/23 puis aller sur WireShark afin de faire une analyse de trame. 