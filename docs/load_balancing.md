# **Configuration du Load Balancing**

## **Présentation**
Le Load Balancing (équilibrage de charge) est une technique utilisée pour distribuer la charge de travail entre plusieurs serveurs ou ressources réseau afin d'optimiser les performances, d'assurer la disponibilité continue et de prévenir la surcharge d'un seul point.

## **Installation**

- Étape 1 : Installation de Nginx sur les serveurs Proxy (Reverse Proxy)

```js
sudo apt update
sudo apt install nginx
```

- Étape 2 : Configuration de Nginx pour le Load Balancing

Modifier le fichier nginx.conf sur vos serveurs proxy pour inclure la configuration du load balancing.

```js
# Emplacement : /etc/nginx/nginx.conf

http {
    upstream backend {
        server 192.168.37.50;
        server 192.168.37.51;
    }

    server {
        listen 80;
        server_name www.tours.sportludique.fr;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

    # ... Autres configurations ...
}
```

- Étape 3 : Redémarrage de Nginx

```js
sudo systemctl restart nginx
```

- Étape 4 : Différencier les pages WEB

Accédez à vos serveurs WEB et faire une léger changement sur le HTML pour différencier les deux serveurs WEB, comme changer la couleur des titres (rouge/bleu).

- Étape 5 : Test

Pour faire le test il suffit d'accéder à votre page WEB et de la rafraichir (F5), si les titres de vos pages WEB changent de couleur alors tout fonctionne parfaitement.