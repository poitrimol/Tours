# Certificat SSL

- Etape 1 : Autorité de Certification

On généré une clé privée pour la nouvelle CA (Autorité de Certification)

    openssl genrsa -out nouvelle_ca.key 4096


Ensuite, on créé un certificat auto-signé pour cette CA

    openssl req -x509 -new -nodes -key nouvelle_ca.key -sha256 -days 365 -out nouvelle_ca.crt


On rempli les informations demandées pour le certificat.


- Etape 2 : Clé privée de la certification du serveur


On génére la clé privée pour le certificat du serveur

    openssl genrsa -out server.key 2048


On Créer une demande de signature de certificat (CSR) pour le serveur
On a généré un CSR pour le serveur avec la clé privée créée précédemment

    openssl req -new -key server.key -out server.csr

On a fourni les informations nécessaires pour le certificat.

-Etape 3 Signature du certificat :


Signature du certificat du serveur avec la nouvelle CA
On a signé le CSR du serveur avec la nouvelle CA pour émettre un certificat

    openssl x509 -req -in server.csr -CA nouvelle_ca.crt -CAkey nouvelle_ca.key -CAcreateserial -out server.crt -days 365 -sha256