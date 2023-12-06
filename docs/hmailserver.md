## Installation de hmailserver

- Etape 1

Installer une machine virtuelle Windows 10 ou Windows Server.

- Etape 2

Installer la dernière version de hmailserver sur le site officiel.

## Configuration de hmailserver en local

- Etape 1 : Ajouter le nom de domaine

Aller dans Domains puis Add... et mettre notre nom de domaine : tours.sportludique.fr

- Etape 2 : Ajouter les utilisateurs

Aller dans Accounts puis Add... et mettre nos noms d'utilisateurs, par exemple : 

Dans Address : prenom.nom@tours.sportludique.fr
Mettre Administration Level en User

- Etape 3 : Configurer le protocole

Aller dans Protocols et vérifier que SMTP, POP3 et IMAP sont cochés puis aller dans SMTP en bas de Protocols.

Aller dans l'onglet Delivery of e-mail, mettre localhost dans localhostname et dans remotehostname : mail.tours.sportludique.fr en port 25.

Aller dans l'onglet advanced et mettre l'adresse ip local, ici : 127.0.0.1

Ne pas oublier de mettre l'enregistrement MX dans les deux serveurs DNS




 
