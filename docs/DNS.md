# **Configurer le DNS**

## Installation DNS

Prérequis :
VM sous Windows Server 2022 via Nutanix.

- Étape 1 : Ouvrir le Gestionnaire de serveur.

Une fois la VM configurée et installée, aller dans le Gestionnaire de serveur, aller sur Gérer tout en haut à droite puis ajouter sur des rôles et fonctionnalités

- Étape 2 : Ajout du rôle DNS

Une fois dans le Gestionnaire de serveur, cliquer sur "Gérer" dans le coin supérieur droit, puis sélectionner "Ajouter des rôles et des fonctionnalités".

Suivre l'Assistant d'installlation jusqu'à "Rôles de serveurs" et cocher "Serveur DNS" puis finir l'Assistant d'installation en suivant les étapes.

- Étape 3 : Configuration de la nouvelle zone

Ouvrir "DNS" depuis la barre de recherche Windows. Sous le noeud DNS, clic droit sur le nom WIN et cliquer sur "Nouvelle zone"

- Étape 4 : Créer une zone DNS principale

Dans "DNS", développer le nœud du serveur DNS (le nom de notre serveur, appelé par défaut WIN-[...]) pour afficher la liste des zones DNS existantes.

Cliquer avec le bouton droit sur "Zones de recherche directe" (Forward Lookup Zones) dans le volet gauche.

Sélectionnez "Nouvelle zone..." pour ouvrir l'Assistant de création de zone.

- Étape 5 : Configuration de la zone DNS principale

L'Assistant de création de zone s'ouvre. Cliquer sur "Suivant" pour commencer.

Choisir le type de zone en sélectionnant "Zone Principale" pour créer une zone DNS principale, puis cliquez sur "Suivant".

Saisissez le nom de domaine pour la nouvelle zone DNS principale créée. Par exemple, notre nom de zone est "tours.sportludique.fr", saisir "tours.sportludique.fr" et cliquez sur "Suivant".

Enregistrer la zone dans un fichier spécifique (zone de stockage) ou de la laisser dans un fichier par défaut. Pour la plupart des configurations standard, on laisse la configuration par défaut. Cliquer sur "Suivant".

On peut activer ou désactiver la mise à niveau dynamique. Cocher "Ne pas autoriser les mises à jour dynamique". Cliquez sur "Suivant".

Cliquer sur "Fermer" pour valider la création.

- Étape 6 : Ajouter un hôte DNS à la zone principale

Ouvrir "DNS" comme décrit précédemment.

Dans "DNS", développer la zone principale que l'on a créée sous le nœud "Zones de recherche directe" (Forward Lookup Zones).

Cliquez avec le bouton droit sur la zone principale (de notre exemple, "tours.sportludique.fr") et sélectionner "Nouvel hôte (A ou AAAA)" pour ajouter un nouvel hôte DNS.

Remplir le nom, dans notre exemple c'est www et l'adresse ip de notre serveur http, ici 221.87.137.2


