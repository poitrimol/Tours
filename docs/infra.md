# **Infrastructure de Tours**

![Plan d'adressage](img/tours.png)

## Infrastructure

| Nombre d'utilisateurs | 179 |
|-|-|
| Nombres de stations de travail | 95 |
| Fixes | 90 |
| Portables | 5 |
| Nombre de serveus | 10 |

## Plan d'adressage

| Nom | Adresse réseau | Masque (CIDR, Binaire) | Broadcast |
|-|-|-|-|
| Chateauroux | 172.28.0.0 | /19 255.255.224.0 | 172.28.31.255 |
| Tours | 172.28.32.0 | /19 255.255.224.0 | 172.28.63.255 |
| Chartres | 172.28.64.0 | /19 255.255.224.0 | 172.28.95.255 |
| Orléans | 172.28.96.0 | /19 255.255.224.0 | 172.28.127.255 |

##  VLAN Tours

| VLAN | Adress réseau + CIDR | Plage d'adresse | Broadcast | Service | 
|-|-|-|-|-|
| 220 | 172.28.32.0 /24 | 172.28.32.1 - 172.28.32.254 | 172.28.32.255 | Management |
| 221 | 192.168.37.0 /24 | 192.168.37.1 - 192.168.37.254 | 192.168.37.255 | DMZ |
| 222 | 172.28.34.0 /24 | 172.28.34.1 - 172.28.34.254 | 172.28.34.255 | srv/ad/dhcp |
| 223 | 172.28.35.0 /24 | 172.28.35.1 - 172.28.35.254 | 172.28.35.255 | ... |
| 224 | 172.28.36.0 /24 | 172.28.36.1 - 172.28.36.254 | 172.28.36.255 | ... |
| 225 | 172.28.37.0 /24 | 172.28.37.1 - 172.28.37.254 | 172.28.37.255 | ... |
| 226 | 172.28.38.0 /24 | 172.28.38.1 - 172.28.38.254 | 172.28.38.255 | Conception | 
| 227 | 172.28.39.0 /24 | 172.28.39.1 - 172.28.39.254 | 172.28.39.255 | Production |
| 228 | 172.28.40.0 /24 | 172.28.40.1 - 172.28.40.254 | 172.28.40.255 | Transport-firewall |
| 229 | 172.28.41.0 /24 | 172.28.41.1 - 172.28.41.254 | 172.28.41.255 | Transport-routeur |


## Plan d'adressage de Tours

![Plan d'adressage](img/Schema-reseau.png)

## Port attribuer au switch

![Ports switch](img/cablage-switch_Projet.png)

## Détails des équipements serveurs

| Nom | Fonction | OS |
|-|-|-|
| TRS_DC_01 | Contrôleur principal de domaine | Windows server 2008 R2 |
| | Serveur primaire DNS | Domaine : tour.local |
| | Serveur DHCP | |
| | Serveur de fichiers | |
| TRS_DC_02 | Controleur secondaire de domaine | Windows server 2008 R2 |
| | Serveur secondaire DNS | |
| | Serveur de fichiers répliqué | |