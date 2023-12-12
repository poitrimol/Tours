# **Configuration de Heart Beat**

## **Présentation**
Heartbeat est un logiciel de haute disponibilité open source largement utilisé dans les environnements de serveurs pour garantir la continuité des services en cas de défaillance matérielle ou logicielle. Il est particulièrement adapté pour les clusters de serveurs, où plusieurs nœuds coopèrent pour assurer une disponibilité constante des services.

## **Installation**

- Etape 1 : Cloner la VM "TRS_ReverseProxy" 

Tout d'abord il faut cloner la VM avec le Reverse Proxy déjà configuré, puis changer le nom des VM pour les identifiés (TRS-ReverseProxy-1 et -2) et enfin changer l'adresse IP du clone (192.168.37.21)

- Etape 2 : Installation de HeartBeat

```js
# Mettez à jour votre liste de paquets
sudo apt-get update

# Installez Heartbeat
sudo apt-get install heartbeat
```

- Étape 2 : Configuration de Heartbeat sur les 2 serveurs Proxy

Créer l'un des fichiers de conf :

```js
sudo nano /etc/ha.d/ha.cf
```

Assurez-vous que votre fichier ha.cf ressemble à quelque chose comme ceci :

```js
node Trs-ReverseProxy-1
ucast ens3 192.168.37.20
node Trs-ReverseProxy-2
ucast ens3 192.168.37.21
```

- Étape 3 : Configuration des Ressources Heartbeat

Les ressources que Heartbeat doit surveiller et gérer sont spécifiées dans le fichier haresources (que vous devez créer). Éditez ce fichier, ajoutez les lignes nécessaires pour spécifier les ressources que Heartbeat doit gérer :

```js
Trs-ReverseProxy-1 IPaddr::192.168.37.22/24/ens3/0
```

```js
Trs-ReverseProxy-1 IPaddr::192.168.37.22/24/ens3/1
```

Ici le 0 et le 1 qui diffère indique la priorité (0 est la plus haute).
Cela indique à Heartbeat de surveiller l'adresse IP 192.168.37.22 (qu'est la VIP) et de la migrer vers Trs-ReverseProxy-1 en cas de défaillance.

- Étape 4: Créer la clé d'authentification "Authkeys"

Créer un fichier /etc/ha.d/authkeys avec des droits spécifiques.

```js
sudo nano /etc/ha.d/authkeys
```

Générer une clé privée dans ce fichier.
Assurez-vous que le fichier est accessible uniquement par l'utilisateur root pour des raisons de sécurité :

```js
sudo chmod 600 /etc/ha.d/authkeys
```

- Étape 5: Redémarrez Heartbeat

```js
sudo systemctl restart heartbeat
```

- Étape 6: Vérifier sur les deux proxy 

```js
ping 192.168.37.22
```

