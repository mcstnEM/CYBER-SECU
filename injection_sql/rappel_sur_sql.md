### **Cours de rappel sur SQL**

#### **1. Introduction à SQL**
SQL (Structured Query Language) est un langage standard pour interagir avec les bases de données relationnelles. Il permet de créer, lire, mettre à jour et supprimer des données dans une base de données. Les principales actions que vous pouvez effectuer avec SQL sont souvent désignées par l'acronyme **CRUD** :
- **C**reate : création de données (via `INSERT`).
- **R**ead : lecture ou récupération de données (via `SELECT`).
- **U**pdate : modification de données existantes (via `UPDATE`).
- **D**elete : suppression de données (via `DELETE`).

#### **2. Structure d'une table SQL**
Une table dans une base de données SQL est constituée de colonnes (attributs) et de lignes (enregistrements). Chaque colonne a un type de données défini (ex. : `INT`, `VARCHAR`, `DATE`).

**Exemple de table `users` :**

| id  | username | email            | age |
| --- | -------- | ---------------- | --- |
| 1   | alice    | alice@example.com | 25  |
| 2   | bob      | bob@example.com   | 30  |
| 3   | claire   | claire@xyz.com    | 28  |

### **3. Principales commandes SQL**

#### **3.1. SELECT : Récupération de données**
La commande `SELECT` est utilisée pour interroger et lire des données d'une base de données.

**Exemple de requête :**
```sql
SELECT username, email FROM users WHERE age > 25;
```
Cette requête sélectionne les colonnes `username` et `email` pour tous les utilisateurs dont l'âge est supérieur à 25.

#### **3.2. INSERT : Ajout de données**
`INSERT` permet d'ajouter de nouvelles lignes dans une table.

**Exemple de requête :**
```sql
INSERT INTO users (username, email, age) VALUES ('dave', 'dave@example.com', 22);
```
Cela insère un nouvel utilisateur "dave" dans la table `users`.

#### **3.3. UPDATE : Modification de données**
`UPDATE` permet de modifier des enregistrements existants dans une table.

**Exemple de requête :**
```sql
UPDATE users SET email = 'bob.new@example.com' WHERE id = 2;
```
Cette requête met à jour l'adresse email de l'utilisateur ayant l'ID 2.

#### **3.4. DELETE : Suppression de données**
`DELETE` supprime une ou plusieurs lignes dans une table.

**Exemple de requête :**
```sql
DELETE FROM users WHERE age < 25;
```
Cette requête supprime tous les utilisateurs ayant un âge inférieur à 25.

---

### **4. Le mot-clé UNION**

#### **4.1. Qu'est-ce que l'`UNION` ?**
L'opérateur `UNION` permet de combiner les résultats de plusieurs requêtes `SELECT` dans une seule. Les résultats doivent avoir le **même nombre de colonnes** et les types de données correspondants dans chaque requête.

#### **4.2. Syntaxe de base :**
```sql
SELECT column1, column2 FROM table1
UNION
SELECT column1, column2 FROM table2;
```

#### **4.3. Exemples d'utilisation :**

1. **Combiner deux ensembles de résultats :**
   Supposez que vous avez deux tables : `customers` et `suppliers`. Si vous voulez obtenir une liste unique de noms provenant des deux tables, vous pouvez utiliser `UNION` :

   ```sql
   SELECT name FROM customers
   UNION
   SELECT name FROM suppliers;
   ```
   Cette requête va retourner les noms uniques provenant des deux tables. Si vous avez des doublons dans les deux résultats, `UNION` les supprimera automatiquement.

2. **UNION ALL** :
   Si vous voulez conserver tous les doublons dans les résultats combinés, utilisez `UNION ALL` au lieu de `UNION`.

   ```sql
   SELECT name FROM customers
   UNION ALL
   SELECT name FROM suppliers;
   ```

   `UNION ALL` n'élimine pas les doublons et est généralement plus rapide que `UNION` car il n'effectue pas de comparaison pour éliminer les résultats identiques.

#### **4.4. Cas d'utilisation :**
- **Combiner des données de plusieurs tables** : Si vous avez plusieurs tables ayant une structure similaire (par exemple, des tables `orders_2023`, `orders_2024`), vous pouvez les interroger simultanément et combiner les résultats avec `UNION`.
  
- **Requêtes multi-critères** : Par exemple, si vous souhaitez obtenir à la fois les utilisateurs ayant plus de 30 ans et ceux qui vivent dans une certaine ville, vous pouvez combiner deux requêtes distinctes avec `UNION`.

```sql
SELECT username FROM users WHERE age > 30
UNION
SELECT username FROM users WHERE city = 'Paris';
```

---

### **5. Commandes SQL supplémentaires importantes**

#### **5.1. WHERE : Filtrer les résultats**
`WHERE` permet de spécifier des conditions pour filtrer les lignes retournées par une requête.

**Exemple :**
```sql
SELECT username FROM users WHERE age > 25;
```
Cette requête ne retourne que les utilisateurs dont l'âge est supérieur à 25.

#### **5.2. JOIN : Combiner les tables**
Les **`JOIN`** sont utilisés pour combiner des lignes de deux ou plusieurs tables en fonction d'une relation entre elles.

**Exemple de `INNER JOIN` :**
```sql
SELECT users.username, orders.order_id
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```
Cette requête retourne les noms des utilisateurs ainsi que leurs commandes en combinant la table `users` avec la table `orders` via la colonne `user_id`.

#### **5.3. GROUP BY : Agrégation des résultats**
`GROUP BY` est utilisé avec des fonctions d'agrégation comme `COUNT()`, `SUM()`, `AVG()`, pour regrouper les résultats par une ou plusieurs colonnes.

**Exemple :**
```sql
SELECT city, COUNT(*) FROM users GROUP BY city;
```
Cette requête retourne le nombre d'utilisateurs par ville.

#### **5.4. HAVING : Filtrer après agrégation**
`HAVING` est utilisé pour filtrer les résultats après une opération d'agrégation.

**Exemple :**
```sql
SELECT city, COUNT(*) FROM users
GROUP BY city
HAVING COUNT(*) > 10;
```
Cette requête retourne uniquement les villes où il y a plus de 10 utilisateurs.

---

### **6. Précautions de sécurité en SQL**

1. **Éviter les injections SQL** :
   Utilisez des **requêtes préparées** (prepared statements) pour éviter que des utilisateurs malveillants n'injectent du code dans vos requêtes.
   ```sql
   SELECT * FROM users WHERE username = ? AND password = ?;
   ```

2. **Limiter les privilèges** :
   N'accordez pas plus de privilèges que nécessaire aux utilisateurs de la base de données. Par exemple, un utilisateur qui ne fait que des requêtes `SELECT` ne devrait pas avoir accès aux commandes `INSERT`, `UPDATE` ou `DELETE`.

---

### **Conclusion :**
Ce rappel sur SQL vous donne une vue d'ensemble des commandes essentielles et des concepts comme `UNION`. Maîtriser SQL vous permet de manipuler et d'interagir efficacement avec les bases de données relationnelles, tout en étant conscient des bonnes pratiques pour protéger vos données contre des vulnérabilités comme les injections SQL.
