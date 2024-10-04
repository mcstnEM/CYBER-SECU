# Création de la pair de clé SSH

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

<aside>
💡 Si vous ajoutez l’option -f vous pourrez indiquer un nom de fichier personnalisé pour la clé publique et privé.

Ex: `ssh-keygen -t ed25519 -C "your_email@example.com" -f $HOME/.ssh/guthub_id_ed25519`.

</aside>

La commande vous demandera une passphrase. Elle ajoute une sécurité en plus. Vous pouvez ne rien mettre si vous le souhaitez.

Cette passphrase vous sera demandé lors des push ou seulement après le premier ajout à l’agent SSH.

<aside>
💡 Pour modifier la passphrase vous pouvez utiliser la commande `ssh-keygen -p -f $HOME/.ssh/id_ed25519`.

`id_ed25519` étant le nom de la clé SSH.

</aside>

Maintenant il ne vous reste plus qu’à récupérer la clé publique pour la transmettre à votre compte GitHub.

# Transmettre la clé publique à GitHub

Rendez-vous dans **Settings > SSH and GPG keys > New SSH key** :

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3936438c-0728-4ee9-9c98-ee0b28b3ac1a/3e211d9c-68d8-4322-9d3a-9785b0e8618e/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3936438c-0728-4ee9-9c98-ee0b28b3ac1a/0c21298c-651d-4cf6-86f8-c775e783ed04/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3936438c-0728-4ee9-9c98-ee0b28b3ac1a/b4032907-1c6b-4046-be7a-40db23f3727d/image.png)

Sur PowerShell et WSL tapez la commande `cat $HOME\.ssh\id_ed25519.pub | clip` pour copier la clé publique dans votre presse-papier.

Sur macOs tapez la commande `cat $HOME\.ssh\id_ed25519.pub | pbcopy` pour copier la clé publique.

Dans la page **"Add new SSH Key"**, remplissez les champs suivants :

- Choisissez un titre dans **Title**.
- Pour le champ Key type, prenez **Authentication key.**
- Et collez la clé SSH publique dans le champ **Key**.

# Transmettre la clé privé à l’agent SSH

Le service `ssh-agent` est utilisé pour gérer les clés SSH en mémoire et permettre une authentification sans avoir à retaper la phrase secrète pour chaque connexion SSH.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3936438c-0728-4ee9-9c98-ee0b28b3ac1a/e5cbab84-ccf9-4644-bcc6-97d57c811914/image.png)

Entré les commandes suivante dans un terminal avec des droits administrateur pour lancer le service `ssh-agent` :

```bash
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
```

Et pour finir, ajouter la clé privé à l’agent SSH :

```bash
# id_ed25519 étant le nom par défaut de la clé privé lors de la commande ssh-keygen.
ssh-add c:/Users/YOU/.ssh/id_ed25519
```

# Tester sa clé SSH

Pour tester que vous êtes bien authentifié, entré la commande suivante :

```bash
ssh -T git@github.com
```

Vous devriez voir une réponse similaire à celle-ci :

```bash
Hi mathieuCstn! You've successfully authenticated, but GitHub does not provide shell access.
```

Si votre nom d’utilisateur apparait, alors vous êtes authentifier !

# Ajout des identifiants de connexion dans le fichier ~/.ssh/config

```
Host *
	IgnoreUnknown AddKeysToAgent
	IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  UseKeychain yes

Host github.com
  User git
  Hostname github.com
  IdentityFile ~/.ssh/id_ed25519
```

# Transfert d’agent SSH

Ajouter le paramètre `ForwardAgent yes` aux serveurs auxquelles vous voulez partager votre agent SSH dans votre fichier de configuration.

`~/.ssh/config`

```
 Host example.com
   ForwardAgent yes
```

[Utilisation du transfert d’agent SSH - Documentation GitHub](https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)

[An Illustrated Guide to SSH Agent Forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html)

### Tester le transfert d’agent SSH

```bash
ssh -T git@github.com
```

Si vous n’êtes pas authentifier, vous pouvez vérifier que la variable d’environnement suivante comporte bien une valeur.

```bash
echo "$SSH_AUTH_SOCK"
```

# Ressources & liens

[Test your ssh terminal connection with github account, (ssh authentication)](https://gist.github.com/ddeveloperr/9574740628637bc2a127)

[ssh/.config](https://gist.github.com/rbialek/1012262)

[Tutoriel SSH : Utiliser une clef SSH](https://youtu.be/Y-S6GtdLaSU?si=sAsYC1FtSk5sogNw)
