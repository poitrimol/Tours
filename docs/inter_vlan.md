# **Inter VLAN**

## Présentation 

La mise en place d'inter-VLAN avec le protocole 802.1Q offre de nombreux avantages pour la gestion et l'optimisation des réseaux d'entreprise. Cela permet de segmenter le trafic, renforçant ainsi la sécurité, optimisant les performances et isolant les domaines de diffusion. De plus, les VLANs permettent de séparer logiquement les fonctions, d'optimiser la bande passante, d'assurer l'évolutivité du réseau et de simplifier la gestion. En résumé, cette configuration est essentielle pour garantir une connectivité efficace et sécurisée au sein d'un réseau d'entreprise.

Ici l'utilisation de l'inter-VLAN avec le protocole 802.1Q permet aux administrateurs du VLAN de management (VLAN 220) d'accéder et de gérer les routeurs sur le VLAN transport routeur (VLAN 229) de manière sécurisée. Cela garantit une connectivité efficace tout en maintenant la séparation logique et la sécurité entre ces deux VLANs.

## Mise en place de l'inter VLAN 

- Étape 1 (sur commutateur) :

Configuration des ports (0/11 et 0/12) en mode Trunk :

```js
sw_Tours# conf t
sw_Tours(config)# int gi0/11
sw_Tours(config-if)# switchport encapsulation dot1q
sw_Tours(config-if)# switchport mode trunk
sw_Tours(config-if)# no shut
sw_Tours(config-if)# exi
sw_Tours(config)# vtp mode transparent
sw_Tours(config)# exi

```

```js
sw_Tours# conf t
sw_Tours(config)# int gi0/12
sw_Tours(config-if)# switchport encapsulation dot1q
sw_Tours(config-if)# switchport mode trunk
sw_Tours(config-if)# no shut
sw_Tours(config-if)# exi
sw_Tours(config)# vtp mode transparent
sw_Tours(config)# exi
```

Le "**mode trunk**" est utilisé sur un commutateur pour permettre le transport de plusieurs VLANs sur un port.

La commande "**encapsulation dot1Q**" est utilisée sur une sous-interface de routeur pour prendre en charge les trames VLAN 802.1Q et les traiter en conséquence.

Le **mode transparent** sert à éviter toute modification non autorisée des VLANs

- Étape 2 (sur R1_TRS) :

Configuration des sous-interfaces VLAN sans oublier de remettre en place le procotole GLBP ainsi que le NAT :

```js
R1_TRS# conf t
R1_TRS(config)# interface GigabitEthernet0/0.229
R1_TRS(config-if)# encapsulation dot1Q 229
R1_TRS(config-if)# ip address 172.28.41.253 255.255.255.0
R1_TRS(config-if)# ip nat inside
R1_TRS(config-if)# glbp 1 load-balancing host-dependent
R1_TRS(config-if)# glbp 1 ip 172.28.41.254
R1_TRS(config-if)# no shut
```

```js
R1_TRS# conf t
R1_TRS(config)# interface GigabitEthernet0/0.220
R1_TRS(config-if)# encapsulation dot1Q 220
R1_TRS(config-if)# ip address 172.28.32.100 255.255.255.0
```

- Étape 3 (sur R2_TRS) :

```js
R2_TRS# conf t
R2_TRS(config)# interface GigabitEthernet0/0.229
R2_TRS(config-if)# encapsulation dot1Q 229
R2_TRS(config-if)# ip address 172.28.41.252 255.255.255.0
R2_TRS(config-if)# ip nat inside
R2_TRS(config-if)# glbp 1 load-balancing host-dependent
R2_TRS(config-if)# glbp 1 ip 172.28.41.254
R2_TRS(config-if)# no shut
```

```js
R2_TRS# conf t
R2_TRS(config)# interface GigabitEthernet0/0.220
R2_TRS(config-if)# encapsulation dot1Q 220
R2_TRS(config-if)# ip address 172.28.32.200 255.255.255.0
```

Après avoir mis en place la configuration d'inter-VLAN avec le protocole 802.1Q, je peux maintenant me connecter depuis mon VLAN de gestion (VLAN 220) et administrer mes routeurs (VLAN 229).