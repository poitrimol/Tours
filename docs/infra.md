# Infrastructure de Tours

![Plan d'adressage](img/tours.png)

## Infrastructure

| Nombre d'utilisateurs | 179 |
|-|-|
| Nombres de stations de travail | 95 |
| Fixes | 90 |
| Portables | 5 |a
| Nombre de serveus | 10 |

## Plan d'adressage

| Nom | Adresse réseau | Masque (CIDR, Binaire) | Broadcast |
|-|-|-|-|
| Chateauroux | 172.16.0.0 | /19 255.255.224.0 | 172.16.31.255 |
| Tours | 172.16.32.0 | /19 255.255.224.0 | 172.16.63.255 |
| Chartres | 172.16.64.0 | /19 255.255.224.0 | 172.16.95.255 |
| Orléans | 172.16.96.0 | /19 255.255.224.0 | 172.16.127.255 |

##  VLAN Tours

<<<<<<< HEAD
| VLAN | Adress réseau + CIDR | Plage d'adresse | Broadcast | Service | 
|-|-|-|-|-|
| 220 | 172.16.32.0 /24 | 172.16.32.1 - 172.16.32.254 | 172.16.32.255 | Management |
| 221 | 172.16.33.0 /24 | 172.16.33.1 - 172.16.33.254 | 172.16.33.255 | DMZ |
| 222 | 172.16.34.0 /24 | 172.16.34.1 - 172.16.34.254 | 172.16.34.255 | ... |
| 223 | 172.16.35.0 /24 | 172.16.35.1 - 172.16.35.254 | 172.16.35.255 | ... |
| 224 | 172.16.36.0 /24 | 172.16.36.1 - 172.16.36.254 | 172.16.36.255 | ... |
| 225 | 172.16.37.0 /24 | 172.16.37.1 - 172.16.37.254 | 172.16.37.255 | ... |
| 226 | 172.16.38.0 /24 | 172.16.38.1 - 172.16.38.254 | 172.16.38.255 | Conception | 
| 227 | 172.16.39.0 /24 | 172.16.39.1 - 172.16.39.254 | 172.16.39.255 | Production |
| 228 | 172.16.40.0 /24 | 172.16.40.1 - 172.16.40.254 | 172.16.40.255 | ... |
| 229 | 172.16.41.0 /24 | 172.16.41.1 - 172.16.41.254 | 172.16.41.255 | ... |
=======
| VLAN | Service | 
|-|-|
| 220 | Management |
| 226 | Conception | 
| 227 | Production |
>>>>>>> 60f1084b4c0e92274d9e1b55f59df57e4ef9564a

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
