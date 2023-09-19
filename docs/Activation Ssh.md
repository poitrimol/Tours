## Configurer le Switch

# Etape 1 : Réinitialiser le switch

Pour réinitialiser il nous suffit de rester appuié sur le bouton de réinitialisation du switch pendant 10sec

# Etape 2 : Configuration

# Etape 3 : Configuration de Base :

Pour commencer nous allons changer le nom du switch :

    #hostname "sw-tours"

Puis nous ajoutons un nom de domaine : 

    #ip domain-name "tours.sportludique.local"

Création d'un vlan de Management :

    #vlan "220"
    #name "Management"
    #"exit"

## Activer le SSH sur un Switch Cisco

#<span style="color: darkblue"> **Etape 1 :** Vérifier la version de l'équipement Cisco pour le protocole SSH

On vérifie la version du switch a l'aide de la commande <code tabindex="0" class="">show version</code> pour s'assuré que le switch soit compatible avec le protocole SSH.

#<span style="color: darkblue"> **Etape 2 :** Création du nom d'hôte et du nom de domaine, et d'un mot de passe pour le mode privilégié

La création d’un nom d’hôte et d’un nom de domaine est indispensable à la configuration d’une connexion SSH. Vous devez donc être en mode configuration et réaliser les commandes suivantes :

<screen>

Ici, nous avons choisi de donner pour nom d’hôte "admin" et pour nom de domaine tours.sportludique.fr.

Pour créer un mot de passe pour le mode privilégié, il faut se placer dans le mode configuration et entrer la commande :

<code tabindex="0" class="">enable password mot_de_passe</code>

#<span style="color: darkblue"> **Etape 3 :** Génération de la paire de clés asymétriques RSA

la clé RSA est une méthode de chiffrement dite asymétrique. Elle est composée de deux clés, une privée et une publique, chiffrées sur 768 bits minimum pour le SSH v2, et permet d’assurer une sécurité optimale. Pour générer cette paire de clés, il suffit d’entrer dans la conf puis entrer la commande :

<code tabindex="0" class="">crypto key generate rsa</code>

Il est également demandé d’entrer la taille souhaitée de la clé en bits. Il est préférable de choisir une taille supérieure à 768 bits pour assurer une plus grande sécurité. Nous avons ici choisi des clés de 1024 bits.

#<span style="color: darkblue"> **Etape 4 :** Activation du protocole SSH sur le switch ou routeur Cisco

Pour activer le protocole SSH, il suffit d’entrer la commande “ip ssh version 2”. Il faut ensuite entrer en mode configuration de ligne VTY dans le but de :

- n’accepter que les connexions SSH au switch, au moyen de la commande “transport input ssh” ;
- ne permettre que des connexions SSH vers d’autres équipements, au moyen de la commande “transport output ssh” ;
- enregistrer le compte utilisateur existant comme compte permettant de mettre en place la connexion en entrant “login local”.

<code tabindex="0" class="">(config)#ip ssh version 2</code><br>
<code tabindex="0" class="">(config)#line vty 0 4</code><br>
<code tabindex="0" class="">(config-line)#transport input ssh</code><br>
<code tabindex="0" class="">(config-line)#transport output ssh</code><br>
<code tabindex="0" class="">(config-line)#login local</code><br>
<code tabindex="0" class="">(config-line)#exit</code>

Si vous n’avez pas encore créé de compte utilisateur, retournez en mode configuration et entrez la commande suivante :

<code tabindex="0" class=""> username "nom_utilisateur" password "mot_de_passe" </code>

Pour vérifier que la configuration a bien été prise en compte par l’équipement, entrez “show run”.

#<span style="color: darkblue"> **Etape 5 :** Test de connexion SSH avec PuTTy

Windows ne possède pas de client SSH natif. Il faut donc en télécharger un. Il est possible d’utiliser PuTTy.

Pour le mettre en mode SSH, il vous suffit de cliquer sur “SSH” et d’entrer le nom d’hôte ou l’adresse IP de l’équipement auquel vous voulez vous connecter.

![Putty configuration](img/Putty_conf.PNG)