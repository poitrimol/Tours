# **OpenVPN Server**

## Présentation : 

OpenVPN est une solution open-source de réseau privé virtuel (VPN) qui permet de créer des connexions sécurisées et cryptées entre des ordinateurs distants via Internet. Il offre une solution flexible, extensible et compatible avec de nombreuses plateformes pour établir des tunnels VPN, permettant ainsi le transfert sécurisé de données sur des réseaux non sécurisés.

## Mise en place du serveur OpenVPN : 

- Etape 1 : Création d'une VM Debian 12

- Etape 2 : Installation de OpenVPN

    sudo apt-get update
    sudo apt install openvpn

- Etape 3 : Installation de Easy-RSA

Le package Easy-RSA fournit des utilitaires pour générer des paires de clés SSL utilisées pour sécuriser les connexions VPN.

    sudo apt install easy-rsa

- Etape 4 : Initialiser PKI OpenVPN

une clé publique et une clé privée pour le serveur et chaque client
un certificat et une clé d'autorité de certification (CA) principale qui sont utilisés pour signer chacun des certificats du serveur et du client.

    cd /usr/share/easy-rsa/
    ./easyrsa init-pki

- Etape 5 : Génerer le Certificat et la clé de l'autorité de certification (CA)

    cd /usr/share/easy-rsa/
    ./easyrsa build-ca
cd /usr/share/easy-rsa/
./easyrsa build-ca

Cela vous demandera la clé CA, la phrase de passe PEM ainsi que le nom commun du serveur.

Le certificat CA est généré et stocké dans  /usr/share/easy-rsa/pki/ca.crt

- Etape 6 : Générer des paramètres Diffie Hellman

    cd /usr/share/easy-rsa/
    ./easyrsa gen-dh

Paramètres DH de taille 2048 créés à /usr/share/easy-rsa/pki/dh.pem

- Etape 7 : Générer le certificat et la clé du serveur OpenVPN

    cd /usr/share/easy-rsa/
    ./easyrsa build-server-full serveur nopass

Saisissez la phrase secrète de l'autorité de certification créée ci-dessus pour générer les certificats et les clés.

- Etape 8 : Générer une clé de code d'authentification de message basée sur le hachage (HMAC)

    sudo openvpn --genkey secret /usr/share/easy-rsa/pki/ta.key

- Etape 9 : Générer un certificat de révocation OpenVPN

    cd /usr/share/easy-rsa/
    ./easyrsa gen-crl

Le certificat de révocation est généré et stocké dans  /usr/share/easy-rsa/pki/crl.pem

- Etape 10 : Copier les certificats et les clés du serveur dans le répertoire de configuration du serveur

    cp -rp /usr/share/easy-rsa/pki/{ca.crt,dh.pem,ta.key,crl.pem,issued,private} /etc/openvpn/server/

- Etape 11 : Générer des certificats et des clés client OpenVPN

    cd /usr/share/easy-rsa/
    ./easyrsa build-client-full <nom d'utilisateur> nopass

- Etape 12 : Copier les certificats clients et les clés dans le répertoire client

    mkdir /etc/openvpn/client/<nom d'utilisateur>
    cp -rp /usr/share/easy-rsa/pki/{ca.crt,issued/<nom d'utilisateur>.crt,private/<nom d'utilisateur>.key} /etc/openvpn/client/<nom d'utilisateur>

- Etape 13 : Configurer le serveur OpenVPN sur Debian 12

Copiez l'exemple de configuration du serveur OpenVPN dans /etc/openvpn/server

    cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/server/



Commande pour voir les modifications effectuer : 
    grep -vE "^$|^#|^;" /etc/openvpn/server/server.conf