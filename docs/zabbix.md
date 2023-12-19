## Installation de Zabbix 6.4 sous Debian 12

Aller sur le site officiel de zabbix : zabbix.com/fr/download

# Configuration de l'Environnement Zabbix

Version de Zabbix : 6.4
Distribution OS : Debian
Version du système d'exploitation : 12
Composants Zabbix : Server, Frontend, Agent
Base de données : MySQL
Serveur WEB : Apache

# Etape 1 : Installer le répertoire de Zabbix :

--> wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
--> sudo dpkg -i zabbix-release_6.4-1+debian12_all.deb
--> sudo apt update

# Etape 2 : Installation des Composants Zabbix : Serveur, Frontend, Configuration Apache, Scripts SQL, Agent 

Installation de la base de données MySQL, de l'interface Web, du Serveur Web Apache pour l'interface Web, des scripts SQL pour créer la structure initiale de la base de données et de l'Agent Zabbix : 

--> sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# Creer la database via le serveur Zabbix

--> sudo mysql -uroot -p
En mot de passe : password

Une fois dans mysql entrer :

--> CREATE DATABASE zabbix character set utf8mb4 collate utf8mb4_bin;
--> CREATE USER zabbix@localhost IDENTIFIED BY 'password';
--> GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost;
--> set global log_bin_trust_function_creators = 1;
--> exit;

# Importation du Schéma de Base de Données Zabbix dans MySQL

--> sudo zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

# Editer le fichier /etc/zabbix/zabbix_server.conf

--> sudo nano /etc/zabbix/zabbix_server.conf

Rechercher DBPassword=, retirer le commentaire et mettre :
DBPassword=password

# Redémarrer le Zabbix server, agent et apache et l'activer :

--> sudo systemctl restart zabbix-server zabbix-agent apache2
--> sudo systemctl enable zabbix-server zabbix-agent apache2

# Accéder à l'interface Zabbix :

--> 172.28.37.30/zabbix

Suivre les instructions.

Rentrer par défaut pour avoir accès à l'interface de Zabbix :

Utilisateur : Admin
Mot de passe : zabbix
