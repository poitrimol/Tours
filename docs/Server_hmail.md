# **Hmail Server**

## Installation


- Etape 1 : Installation de Hmail Server

On installe la version 2.0 de Dotnet Framework

On installe la derniere version de HmailServer

On coche une base de donnée interne dans l'installations

- Etape 2 : Configuration du server Hmail

Dans l'onglet Domain:

on ajoute notre domaine 

    tours.sportludique.fr

on ajoute un compte dans le domaine

Dans l'onglet Setting/Protocol/SMTP/Delivery of e-mail/Local Host Name

    mail.tours.sportludique.fr

on laisse vide le Remote Host Name

Dans l'onglet Setting/Protocol/SMTP/Delivery of e-mail/Advanced

on laisse vide Bind to local IP address

on decoche StartTLS

On désactive Auto-BAN

- Etape 3 : Configuration des enregistrement DNS

DNS Server Interne : 

On ajoute un nouvelle Hôte : 
    Hôte = mail
    Ip address = SRV Hmail address

On ajoute un nouvelle enregistrement MX : 
    nom de domaine pleinement qualifier : mail.tours.sportludique.fr

DNS Server Externe :

Nouvelle enregistrement MX :
    nom de domaine pleinement qualifier : pointe vers l'adresse ip publique du routeur

- Etape 4 : Configuration du client

On installe ThunderBird comme client de messagerie et on configure notre adresse mail.
