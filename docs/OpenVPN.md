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

    port 1194
    proto udp4
    dev tun
    ca ca.crt
    cert issued/server.crt
    key private/server.key  # This file should be kept secret
    dh dh.pem 
    topology subnet
    server 172.16.20.0 255.255.255.0
    ifconfig-pool-persist /var/log/openvpn/ipp.txt
    push "redirect-gateway def1 bypass-dhcp"
    push "dhcp-option DNS 9.9.9.9"
    push "dhcp-option DNS 8.8.8.8"
    client-to-client
    keepalive 10 120
    tls-auth ta.key 0 # This file is secret
    cipher AES-256-CBC
    persist-key
    persist-tun
    status /var/log/openvpn/openvpn-status.log
    log-append  /var/log/openvpn/openvpn.log
    verb 3
    explicit-exit-notify 1
    auth SHA512

- Etape 14 : Configurer le transfert IP

Décommentez la ligne , net.ipv4.ip_forward=1pour /etc/sysctl.confactiver le transfert de paquets pour IPv4

    sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf

Appliquez les modifications sans redémarrer le serveur.

    sysctl -p

- Etape 15 : Autoriser le port du service OpenVPN via le pare-feu

    ufw autorise 1194/udp

- Etape 16 : Configurer le Masquerading sur le pare-feu

Trouvez votre interface par défaut via laquelle vos paquets sont envoyés.

    ip route get 10.28.37.1

Ensuite on met a jour les règles du pare-feu

    vim /etc/ufw/before.rules


Ajoutez les lignes suivantes juste avant les *filterparamètres du tableau. Notez que l'interface utilisée doit correspondre au nom de l'interface ci-dessus

    *nat
    :POSTROUTING ACCEPT [0:0]
    -A POSTROUTING -s 10.28.37.0/24 -o enp0s3 -j MASQUERADE
    COMMIT
    # Don't delete these required lines, otherwise there will be errors
    *filter

Activer le transfert de paquets UFW 

    sed -i 's/DEFAULT_FORWARD_POLICY="DROP"/DEFAULT_FORWARD_POLICY="ACCEPT"/' /etc/default/ufw

Recharger le pare-feu

    ufw reload

- Etape 17 : Exécuter le serveur OpenVPN

    systemctl enable --now openvpn-server@server

Vérifier le status

    systemctl status openvpn-server@server