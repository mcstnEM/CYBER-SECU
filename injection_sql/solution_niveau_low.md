# Injection SQL - niveau low

### Étape #0 - Fonctionnement

On peut entrer un numéro et récupérer des information sur un utilisateur.

### Étape #1 - première injection SQL

On peut essayer de récupérer tout les utilisateurs en changeant la condition de l’instruction SQL :

- ' OR 1=1#
    
    Ici on referme des guillemets simple pour une première condition. On met une condition OU pour vérifier une seconde condition (1=1 est toujours vrai). Et puis on comment le reste de la requête SQL avec #.
    
- SELECT <nom>, <prenom> FROM <table> WHERE id = '' OR 1=1#';
    
    Voici ce que nous donnerais supposément la commande SQL dans son entièreté.
    

### Étape #2 - récupérer les tables et identifier la tables stockant les mots de passe

- 'UNION SELECT table_name, NULL FROM information_schema.tables#
    
    Pour utiliser `UNION`, il faut s’assurer qu’on a le même nombre de colonne dans les tables concernées. On suppose qu’on a deux table (au minimum) puisqu’on n’a initialement deux résultats de la part de la base de données.
    

### Étape #3 - afficher les colonnes de la table

- 'UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name= 'users'#
    
    On doit récupérer les colonnes afin de pouvoir les afficher. Pour cela nous récupérons l’information via `information_schema.columns`.
    

### Étape #4 - afficher la base de données

- 'UNION SELECT user, password FROM users #
    
    Une fois les colonnes récupérées, nous allons pouvoir extraire les informations de la base de données.
    

### Étape #5 - récupérer les mots de passe

Pour cette étape nous allons utiliser crackstation pour récupérer les mots de passe, qui sont des mots de passe faibles.

### Étape #6 - tester les mots de passe

Une fois les mots de passe récupérés avec leur identifiant, il est temps de tester si on peut s’identifier avec. Si c’est le cas, nous avons réussi l’intrusion.
