# Windows

## Vérifier que la virtualisation soit activé

Faite le raccourcie clavier `windows + x` > gestionnaire des tâches > processus > processeur.  
L'interface vous indique si la virtualisation est activé.

## Activer la virtualisation (si non-activé)

Regardez comment accéder au BIOS en fonction du constructeur de la carte mère.  
En générale cela demande un redémarage et maintenir la touche F2 ou DEL lors le l'affichage de la carte mère à l'écran.

- Pour les systèmes avec processeur Intel, chercher le paramètre Intel VT-X.
- Pour les systèmes avec processeur AMD, chercher le paramètre AMD-V.

## Activer le paramètre optionel Hyper-V ou Installer WSL (meilleur performance).

### Activer Hyper-V

Dans powershell entrez la commande `optionalFeatures`. Cherchez Hyper-V et cochez la case. Redémarrez l'ordinateur.

### Intaller WSL

Dans le CMD ou Powershell, exécutez la commande `wsl --install` et redémarez l'ordinateur.

## Installer docker

Téléchargez l'installer de docker desktop, exécutez-le. Faite attention à utiliser WSL plutôt que Hyper-V si vous avez choisi d'utiliser WSL.

# MacOS

Télécharger l'installer de docker desktop et exécutez-le.
