# **Configurer le Switch**

## Activer le ssh

### Etape 1 : Réinitialiser le switch

Pour réinitialiser il nous suffit de rester appuié sur le bouton de réinitialisation du switch pendant 10sec

### Etape 2 : Configuration

### Etape 3 : Configuration de Base :

Pour commencer nous allons changer le nom du switch :

    #hostname "sw-tours"

Puis nous ajoutons un nom de domaine : 

    #ip domain-name "tours.sportludique.local"

Création d'un vlan de Management :

    #vlan "220"
    #name "Management"
    #"exit"
    #int vlan 220
    #ip address 172.28.32.254 255.255.255.0


| Nom du Vlan | Vlan |
|-|-|
| Management | 220 |
| DMZ PUB | 221 |
| Serveur | 222 |
| DMZ PRV | 223 |
| Conception | 226 |
| Production | 227 |
| Transport-firewall | 228 |
| Transport-routeur | 229 |


## Activer le SSH sur un Switch Cisco

### Etape 1 : Vérifier la version de l'équipement Cisco pour le protocole SSH

On vérifie la version du switch a l'aide de la commande #"show version" pour s'assuré que le switch soit compatible avec le protocole SSH.

### Etape 2 : Création du nom d'hôte et du nom de domaine, et d'un mot de passe pour le mode privilégié

La création d’un nom d’hôte et d’un nom de domaine est indispensable à la configuration d’une connexion SSH. Vous devez donc être en mode configuration et réaliser les commandes suivantes :

<screen>

Ici, nous avons choisi de donner pour nom d’hôte "admin" et pour nom de domaine tours.sportludique.fr.

Pour créer un mot de passe pour le mode privilégié, il faut se placer dans le mode configuration et entrer la commande :

    #enable password "P@ssw0rd123456!"

### Etape 3 : Génération de la paire de clés asymétriques RSA

la clé RSA est une méthode de chiffrement dite asymétrique. Elle est composée de deux clés, une privée et une publique, chiffrées sur 768 bits minimum pour le SSH v2, et permet d’assurer une sécurité optimale. Pour générer cette paire de clés, il suffit d’entrer dans la conf puis entrer la commande :

    #crypto key generate rsa

Il est également demandé d’entrer la taille souhaitée de la clé en bits. Il est préférable de choisir une taille supérieure à 768 bits pour assurer une plus grande sécurité. Nous avons ici choisi des clés de 1024 bits.

### Etape 4 : Activation du protocole SSH sur le switch ou routeur Cisco

Pour activer le protocole SSH, il suffit d’entrer la commande #“ip ssh version 2”. Il faut ensuite entrer en mode configuration de ligne VTY dans le but de :

- n’accepter que les connexions SSH au switch, au moyen de la commande “transport input ssh” ;
- ne permettre que des connexions SSH vers d’autres équipements, au moyen de la commande “transport output ssh” ;
- enregistrer le compte utilisateur existant comme compte permettant de mettre en place la connexion en entrant “login local”.

Commande

    #ip ssh version 2
    #line vty 0 4
    #transport input ssh
    #transport output ssh
    #login local
    #exit

Si vous n’avez pas encore créé de compte utilisateur, retournez en mode configuration et entrez la commande suivante :

    #username "nom_utilisateur" password "mot_de_passe"

Pour vérifier que la configuration a bien été prise en compte par l’équipement, entrez “show run”.

### Etape 5 : Test de connexion SSH avec PuTTy

Pour le mettre en mode SSH, il vous suffit de cliquer sur “SSH” et d’entrer le nom d’hôte ou l’adresse IP de l’équipement auquel vous voulez vous connecter.

Pour le mettre en mode SSH, il vous suffit de cliquer sur “SSH” et d’entrer le nom d’hôte ou l’adresse IP de l’équipement auquel vous voulez vous connecter.

### Etape 6 : Route par Defaut

Ajouter la Route par Defaut sur le switch. La route par defaut va etre l'adresse IP qui va nous permettre de sortir vers le Firewall.


## Attribution d'un port a un vlan :

On entre dans la configuration du switch puis on entre sur l'interface : 

    #int gi0/1
    #switchport mode access
    #switchport access vlan 220
    #exit


    #show vlan


| Vlan | Port attribuer |
|-|-|
| 220 | gi0/1 - gi0/2 |
| 222 | gi0/3 |
| 226 | gi0/13 |
| 227 | gi0/14 |
| 228 | gi0/18 - gi0/20 |
| 229 | gi0/17 - gi0/19 - gi0/21 |


## Relais DHCP

On active le relais DHCP pour relayer la communication DHCP entre les hôtes et le serveur DHCP sur les Vlans :

    #int vlan 222
    #ip helper-address 172.28.37.1

    #show run