# Création VM HTTP

Rajouter l'ACL sur les Routeurs pour que l'adresse réseau de la DMZ puisse accéder à Internet.
Ajouter la redirection de port sur les routeurs pour le port 80.

## Configuration

- Étape 1 : Donner un nom à la VM

Donner un nom significatif à la machine virtuelle, ici on utilise "TRS_SRV_HTTP" (Tours_Serveur_HTTP). Cela permettra de l'identifier facilement dans notre environnement virtuel.


- Étape 2 : Attribution de la mémoire RAM

Diminuer la quantité de RAM de la VM à 2 Go. Par défaut, il y a 4 Go de RAM, mais nous pouvons diminuer cette valeur pour répondre aux besoins de notre VM.


- Étape 3 : Ajouter l'ISO de Debian 12

Ajouter l'ISO de Debian 12 à la VM. Utilisez l'ISO nommé "debian-12.1.0-amd64-netinst.iso" fourni par le professeur. Cela permettra de démarrer la VM à partir de cet ISO.

- Étape 4 : Ajouter un disque de 25 Go

Ajouter un disque de 25 Go à la VM. Ce disque servira à stocker des fichiers, notamment les ISO que nous avons ajoutés précédemment.

- Étape 5 : Attacher un sous-réseau VLAN

Associer un sous-réseau VLAN à la VM. On utilise le VLAN 221 qui a été précédemment configurée. Cette étape permet à la VM d'être connectée au réseau correct.

Ajouter les utilisateurs dans le sudo user.

