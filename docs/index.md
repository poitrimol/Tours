# Documentation de Tours

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

test5

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

## Plan d'adressage

| Nom | Adresse réseau | Masque (CIDR, Binaire) | Broadcast |
|-|-|-|-|
| Chateauroux | 172.16.0.0 | /19 255.255.224.0 | 172.16.35.255 |
| Tours | 172.16.36.0 | /19 255.255.224.0 | 172.16.63.255 |
| Chartres | 172.16.64.0 | /19 255.255.224.0 | 172.16.95.255 |
| Orléans | 172.16.96.0 | /19 255.255.224.0 | 172.16.127.255 |

##  VLAN Tours

| VLAN | Service | 
|-|-|
| 220 | Management |
| 221 | Conception | 
| 222 | Production |