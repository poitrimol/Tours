# **Controle de domaine**

## Présentation 

**Le Contrôleur de Domaine** (ou Domain Controller en anglais) est un élément central dans la gestion d'un réseau basé sur le **système d'exploitation Windows**. Il joue un rôle fondamental dans la gestion de l'authentification des utilisateurs, la sécurité des données et la gestion des ressources au sein d'un réseau. Le Contrôleur de Domaine est souvent utilisé dans des environnements d'entreprise pour centraliser la gestion des utilisateurs, des ordinateurs et des ressources réseau.

## <span style="color: darkblue"> **Installation DNS**

**- Étape 1 :** Installation du service DNS

Je me connecte à mon serveur Windows.

J'ouvre le "Gestionnaire de serveur" depuis le menu "Gérer" dans la barre des tâches ou en tapant "ServerManager" dans la ligne de commande.

Dans le Gestionnaire de serveur, je clique sur "Gérer" dans le coin supérieur droit, puis je sélectionne "Ajouter des rôles et des fonctionnalités".

Je suis l'assistant d'ajout de rôles et de fonctionnalités et sélectionne le rôle "Serveur DNS".

Je termine l'installation en suivant les étapes de l'assistant. Une fois l'installation terminée, je ferme l'assistant.

**- Étape 2 :** Configuration du service DNS pour "local.tours.sportludique.fr"

Après l'installation, je vais dans le "Gestionnaire de serveur" et je développe "Outils" > "Services de rôles" > "Serveur DNS".

Sous "Serveur DNS", je clique avec le bouton droit sur le serveur DNS nouvellement installé et je sélectionne "Nouvelle zone de recherche directe" (Forward Lookup Zone).

Dans l'assistant de création de zone, je choisis de créer une zone primaire et j'entre "local.tours.sportludique.fr" comme nom de zone.


**- Étape 3 :** Vérification et test du service DNS

Pour tester le service DNS, je peux utiliser la commande "nslookup" sur le serveur ou d'autres machines de mon réseau pour résoudre des noms de domaine en adresses IP.

Je peux également configurer la machine virtuelle Windows 10 client (mentionnée dans la précédente réponse) pour utiliser le serveur DNS nouvellement configuré en tant que serveur DNS primaire.

Ensuite, je teste la résolution DNS en utilisant des noms de domaine de "local.tours.sportludique.fr" pour m'assurer que les enregistrements DNS sont corrects et que les résolutions fonctionnent comme prévu.


## <span style="color: darkblue"> **Installation DHCP**

**- Étape 1 :** Installation du serveur DHCP

Je me connecte à mon serveur Windows (dans mon cas, Windows Server 2016 ou version ultérieure).

J'ouvre le "Gestionnaire de serveur" depuis le menu "Gérer" dans la barre des tâches ou en tapant "ServerManager" dans la ligne de commande.

Dans le Gestionnaire de serveur, je clique sur "Gérer" dans le coin supérieur droit, puis je sélectionne "Ajouter des rôles et des fonctionnalités".

Je suis l'assistant d'ajout de rôles et de fonctionnalités et sélectionne le rôle "Serveur DHCP".

Je termine l'installation en suivant les étapes de l'assistant. Une fois l'installation terminée, je ferme l'assistant.

**- Étape 2 :** Configuration des étendues DHCP

Après l'installation, je vais dans le "Gestionnaire de serveur" et je développe "Outils" > "Services de rôles" > "Serveur DHCP".

Sous "Serveur DHCP", je clique avec le bouton droit sur le serveur DHCP nouvellement installé et je sélectionne "Nouvelle étendue".

Je configure la première étendue "Conception" en spécifiant une plage d'adresses IP de 172.28.38.10 à 172.28.38.250, le masque de sous-réseau approprié (généralement 255.255.255.0), la passerelle par défaut et les serveurs DNS.

Je répète le processus pour créer la deuxième étendue "Production" en spécifiant la plage d'adresses IP de 172.28.39.10 à 172.28.39.250, ainsi que les autres paramètres requis.

Une fois les étendues configurées, je les active pour permettre au serveur DHCP de distribuer des adresses IP.

**- Étape 3 :** Configuration de la machine virtuelle Windows 10 client

J'installe Oracle VM VirtualBox sur ma machine si ce n'est pas déjà fait, puis je le lance.

Je crée une nouvelle machine virtuelle en spécifiant Windows 10 comme système d'exploitation invité et en suivant les étapes de configuration (mémoire, disque dur virtuel, etc.).

Dans les paramètres de la machine virtuelle, je vais dans "Réseau" et je configure l'adaptateur réseau en mode "Bridged" (pont) pour qu'il puisse se connecter au réseau physique.

Je démarre la machine virtuelle Windows 10 et je m'assure qu'elle obtient une adresse IP automatiquement du serveur DHCP que j'ai configuré précédemment.

Je vérifie que la machine virtuelle peut accéder au réseau et à Internet.

**- Étape 4 :** Test du serveur DHCP et du domaine

Je peux maintenant tester le serveur DHCP en vérifiant si la machine virtuelle Windows 10 reçoit une adresse IP de l'une des étendues que j'ai configurées (Conception ou Production).

Si la machine virtuelle reçoit avec succès une adresse IP, je peux également tester la connectivité au serveur DHCP en exécutant des commandes telles que "ipconfig /renew" pour renouveler l'adresse IP.

Ensuite, je peux configurer la machine virtuelle Windows 10 pour rejoindre le domaine Active Directory (si déjà configuré) en utilisant les paramètres appropriés.

Une fois connectée au domaine, je peux tester l'authentification en me connectant en tant qu'utilisateur du domaine et en accédant aux ressources partagées du réseau.