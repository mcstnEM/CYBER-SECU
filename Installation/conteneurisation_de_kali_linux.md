# Conteneriser l'os Kali Linux

## Commande docker run

```sh
docker run -d \
--name=kali-linux \
--security-opt seccomp=unconfined `#optional` \
-e PUID=1000 \
-e PGID=1000 \
-e TZ=Etc/UTC \
-e SUBFOLDER=/ `#optional` \
-e TITLE="Kali Linux" `#optional` \
-p 3000:3000 \
-p 3001:3001 \
--device /dev/dri:/dev/dri `#optional` \
--shm-size="1gb" `#optional` \
--restart unless-stopped \
lscr.io/linuxserver/kali-linux:latest
```

> l'option `--device` nous permet d'utiliser un composant physique de la machinne hôte. Il est optionel.  
l'option `--shm-size` nous permet d'attribuer une quantité de mémoire voulue. Par défaut docker limite la mémoire à 64MB.

## Créer un network et le connecter à Kali linux et DVWA

```sh
# Création d'un network nommé hacking-lab
docker network create hacking-lab

# Création du container DVWA
docker run --name=web-dvwa --rm -it -d vulnerables/web-dvwa

# Création du container Kali Linux
# Voir la commande plus haut

# Connection des container au network hacking-lab
docker network connect hacking-lab kali-linux
docker network connect hacking-lab web-dvwa
```

## Utilisation d'un docker compose pour aller encore plus vite

```yaml
services:
    kali-linux:
        image: lscr.io/linuxserver/kali-linux:latest
        container_name: kali-linux
        security_opt:
            - seccomp=unconfined # optional
        environment:
            - PUID=1000
            - PGID=1000
            - TZ=Etc/UTC
            - SUBFOLDER=/
            - TITLE="Kali Linux"
        ports:
            - "3000:3000"
            - "3001:3001"
        devices:
            - /dev/dri:/dev/dri # optional
        shm_size: "1gb" # optional
        restart: unless-stopped
        networks:
            - hackinglab
            - default # Connect kali-linux to the default bridge network

    web-dvwa:
        image: vulnerables/web-dvwa
        container_name: web-dvwa
        networks:
            - hackinglab
        command: /bin/sh -c "while true; do sleep 30; done;" # keep the container running

networks:
    hackinglab:
        driver: bridge
        internal: true
```

Créez un fichier docker-compose.yaml et exécuter la commande `docker compose up -d` depuis le répertoir du fichier docker compose.

### Se connecter au container depuis Kali Linux

- Exécuter la commande `docker inspect <id-ou-nom-du-container>` et rechercher l'adresse ip du container DVWA (C'est le champ IPAddress).
- Utiliser l'adresse IP depuis un navigateur dans le container Kali Linux.
