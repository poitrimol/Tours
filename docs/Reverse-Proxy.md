## Installation

- Etape 1 : Création d'une VM Debian 12

- Etape 2 : Installation de nginx

sudo apt-get update
sudo apt-get install nginx

## Configurer nginx

- Etape 1 : Acceder au fichier de conf de nginx

sudo nano /etc/nginx/nginx.conf

- Etape 2 : Ajouter dans http { et modifier selon notre infrastructure : 
server {
        listen 80;
        server_name www.tours.sportludique.fr ; 

        location / {
            proxy_pass http://192.168.37.50:80;   //avec une resolution donnant l'IP interne par le resolver
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

    server {
        listen 80;
        server_name blog.tours.sportludique.fr;

        location / {
            proxy_pass http://192.168.37.50:80; // avec un port d'écoute spécifique à un vhost
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

    server {
        listen 80;
        server_name node.tours.sportludique.fr;

        location / {
            proxy_pass http://192.168.37.10:8080;  #
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}

- Etape 4 : Sauvegarder, fermer le fichier, tester et redémarrer nginx

sudo nginx -t //Permet de tester la configuration nginx

sudo systemctl restart nginx //Permet de redémarrer ngninx

- Etape 5 : Activer le support HTTPS

server {
        listen 80;
        server_name www.tours.sportludique.fr ; 

        location / {
            proxy_pass http://192.168.37.50:80;   //avec une resolution donnant l'IP interne par le resolver
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

    //Activer le support HTTPS

    server {
        listen 443 ssl;
        server_name www.tours.sportludique.fr;
        
        ssl_certificate /etc/nginx/certificats/server.pem;
        ssl_certificate_key /etc/nginx/certificats/key.pem;

        location / {
            proxy_pass http://
            include etc/nginx/proxy_params;
        }
    }

    server {
        listen 80;
        server_name blog.tours.sportludique.fr;

        location / {
            proxy_pass http://192.168.37.50:80; // avec un port d'écoute spécifique à un vhost
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

    server {
        listen 80;
        server_name node.tours.sportludique.fr;

        location / {
            proxy_pass http://192.168.37.10:8080;  #
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

    server {
        listen 443 ssl;
        server_name node.tours.sportludique.fr;
        ssl_certificate /etc/nginx/certificats/server.pem;
        ssl_certificate_key /etc/nginx/certificats/key.pem;

        location / {
            proxy_pass http://192.168.37.10:8080;
            include /etc/nginx/proxy_params;
        }
    }
}

- Etape 6 : Mettre les enregistrements dans le serveur DNS

www    CNAME   ReverseProxy.tours.sportludique.fr
blog   CNAME   ReverseProxy.tours.sportludique.fr
node   CNAME   ReverseProxy.tours.sportludique.fr

ReverseProxy   A   192.168.37.20


