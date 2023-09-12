# Infrastructure de Tours

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
| Chateauroux | 172.16.0.0 | /19 255.255.224.0 | 172.16.31.255 |
| Tours | 172.16.32.0 | /19 255.255.224.0 | 172.16.63.255 |
| Chartres | 172.16.64.0 | /19 255.255.224.0 | 172.16.95.255 |
| Orléans | 172.16.96.0 | /19 255.255.224.0 | 172.16.127.255 |

##  VLAN Tours

| VLAN | Service | 
|-|-|
| 220 | Management |
| 221 | Conception | 
| 222 | Production |

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