SSH signifie Secure Shell. Il s’agit à la fois d’un programme informatique et d’un protocole de communication sécurisé.

Il nous permet d’exécuter des commandes ou de transférer des données sur un serveur distant via un terminal sécurisé.

# Se connecter à un serveur

```bash
ssh -p 22 user@host
```

# Clé privé

Une clé privé doit être conserver précieusement sur la machine de l’utilisateur. Cette clé est créé en même temps qu’une clé publique qui elle pourra être communiqué.

[À quoi sert une clé privé ?](https://www.notion.so/quoi-sert-une-cl-priv-8b9ee92cc06a4f828fde08a957c99790?pvs=21)

# Créer une clé privée sécurisée avec `ssh-keygen`

```bash
ssh-keygen -t ed25519 -C "commentaire" -f ~/.shh/monserveur
```

- `-t` : Défini le type de clé; `ed25519` étant le plus solide pour le moment. Si `-t` n’est pas spécifier, la clé ssh sera de type `RSA`.
- `-C` : Ajoute un commentaire à la clé. Pratique pour se souvenir à quoi cette clé correspond. On peut y mettre par exemple un email.
- `-f` : Spécifie le nom du fichier où se trouve la clé.

# Clé publique

# Transmettre une clé publique

## Transmettre une clé publique avec `ssh-copy-id`

```bash
ssh-copy-id -i ~/.ssh/monservexiteur.pub mathieu@monserveur.com
```

- `-i` : Indique la clé à transmettre
- `mathieu@monserveur.com` : ici on indique le serveur où l’on souhaite transmettre la clé publique

## Transmettre une clé publique SANS `ssh-copy-id`

# Utiliser la clé privé pour se connecter

```bash
ssh -i ~/.ssh/monserveur mathieu@monserveur.com
```

- `-i` : Permet d’utiliser la clé privé comme moyen de s’authentifier.

<aside>
☝ La première connexion demandera le mot de passe

</aside>

# Configurer des connexions automatiques avec les alias SSH

On peut éditer un fichier de configuration afin de se connecter automatiquement au serveurs avec `ssh`.

Ce fichier doit se trouver dans le dossier `~/.ssh` et se nommer `config`.

```bash
Host *
	IgnoreUnknown AddKeysToAgent
	IgnoreUnknown UseKeyChain

Host monserveur
	HostName monserveur
	User mathieu
	Port 22
	IdentitiesOnly yes
	IdentityFile ~/.ssh/monserveur
	AddKeysToAgent yes
	UseKeyChain yes
```

- `Host` : Nom de l’alias
- `HostName` : Nom d’hôte (adresse IP, domaine)
- `User` : Nom de l’utilisateur
- `Port` : Le port
- `IdentitiesOnly` : Indique qu’on souhaite utiliser seulement les clés SSH ou non
- `IdentityFile` : Indique où se trouve le fichier dans lequel est la clé SSH privé
- `AddKeysToAgent` : Permet d’activer un agent de clé qui garde en mémoire les connexions récentes pour évité à ré-entrer le mot de passe.
- `UseKeyChain` : Permet d’utiliser le système de clé du système d’exploitation (fonction sur macOS)
- `IgnoreUnknown` : Ne prends pas en compte un paramètre s’il n’est pas pris en compte par le système

# Supprimer les clés d’un hôte dans le fichier known_hosts

Cette opération peut se faire à travers la commande `ssh-keygen` avec l’option `-R`.

```bash
ssh-keygen -R 192.168.1.20
```

Automatiquement les hôtes supprimés sont déplacés dans le fichier known_hosts.old.

# L’agent SSH

L’agent SSH gère les clés privées qu’il met en cache afin de ne pas à avoir entrer plusieurs fois passphrase.

```bash
# Ajoute une clé dans l'agent SSH
ssh-add <nom de la clé privé>

# Liste des paramètres des clés publiques de toutes les identités actuellement représentées par l'agent.
ssh-add -L
```

# Ressources & liens

[Tutoriel SSH : Utiliser une clef SSH](https://youtu.be/Y-S6GtdLaSU?si=EWcsDzTlYWdV_a2R)
