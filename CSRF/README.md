### **Les attaques Cross-Site Request Forgery (CSRF)**

#### **Objectifs :**
- Comprendre ce qu’est une attaque CSRF.
- Identifier les risques liés aux failles CSRF.
- Apprendre les méthodes d’exploitation des failles CSRF.
- Explorer les techniques de protection contre ces attaques.

---

### **1. Introduction à CSRF**
Le **Cross-Site Request Forgery** (CSRF) est une attaque qui exploite la session authentifiée d'un utilisateur pour exécuter des actions non désirées sur une application web. Cela se produit lorsque l'attaquant force un utilisateur à effectuer une requête malveillante à son insu, profitant du fait que l'utilisateur est déjà authentifié.

#### **Exemple typique :**
- **Victime** : Un utilisateur est connecté à son compte sur un site web (ex. : une banque en ligne).
- **Attaquant** : Envoie un lien ou un formulaire malveillant à la victime.
- **But** : Le lien exécute une action comme le transfert d'argent à l'insu de l'utilisateur, en utilisant les cookies de session légitimes de l'utilisateur.

---

### **2. Comment fonctionne une attaque CSRF ?**

1. **Session active** : La victime est connectée à un site web et dispose d'une session authentifiée (par exemple via un cookie de session).
2. **Induction d’une action** : L'attaquant incite la victime à visiter une page malveillante ou à cliquer sur un lien contenant une requête spécialement conçue.
3. **Exécution de la requête** : La page malveillante envoie une requête HTTP à l'application web cible, qui interprète cette requête comme provenant de la victime légitime, puisque le cookie de session est automatiquement inclus dans la requête.
4. **Action non désirée** : Le site web effectue l'action (transfert d’argent, changement de mot de passe, etc.) en pensant que c’est l’utilisateur authentifié qui l'a demandée.

---

### **3. Exemple d'attaque CSRF**

#### **Exemple de requête légitime** (dans une banque en ligne) :
```http
POST /transfert-argent HTTP/1.1
Host: bank.example.com
Cookie: sessionID=abc123
Content-Type: application/x-www-form-urlencoded

amount=1000&account=1234567890
```
Cette requête transfère 1000 € du compte de l'utilisateur connecté à un autre compte.

#### **Exemple de lien malveillant (attaque CSRF)** :
L'attaquant pourrait envoyer un email ou un message avec un lien comme celui-ci :
```html
<a href="http://bank.example.com/transfert-argent?amount=1000&account=attacker123">
   Cliquez ici pour voir un chaton mignon !
</a>
```

Si la victime clique sur ce lien alors qu'elle est déjà connectée à sa banque, la requête sera envoyée à la banque et le transfert sera effectué à l'insu de la victime.

---

### **4. Pourquoi CSRF est-il dangereux ?**
- **Pas de contrôle de l’origine** : Les serveurs ne vérifient pas toujours si une requête provient d'une source légitime. Ils se basent souvent uniquement sur le cookie de session pour identifier l'utilisateur.
- **Large éventail d'actions possibles** : Toute action qui peut être exécutée via une requête HTTP (POST, GET) peut potentiellement être ciblée par une attaque CSRF. Cela inclut les changements de mots de passe, les transferts d'argent, ou même la suppression de données.
- **Exploitation silencieuse** : Les utilisateurs ne se rendent souvent pas compte que des actions sont effectuées à leur insu, car la requête semble être légitime.

---

### **5. Méthodes de protection contre CSRF**

#### **5.1. Utilisation de tokens CSRF**
L'une des protections les plus efficaces contre CSRF est l’utilisation de **tokens CSRF**. Ce sont des chaînes de caractères générées de manière aléatoire et uniques à chaque session, qui sont intégrées dans chaque formulaire HTML ou requête envoyée par l'utilisateur.

- Le token est envoyé à l’utilisateur sous la forme d'un champ caché ou d'un en-tête HTTP.
- Lorsqu'une requête est effectuée, le serveur vérifie si le token CSRF envoyé par l'utilisateur correspond à celui stocké dans sa session.
- Si le token est manquant ou incorrect, le serveur rejette la requête.

**Exemple d'intégration d'un token CSRF dans un formulaire :**
```html
<form action="/transfert-argent" method="POST">
    <input type="hidden" name="csrf_token" value="a1b2c3d4e5f6g7h8i9">
    Montant : <input type="text" name="amount">
    Compte : <input type="text" name="account">
    <button type="submit">Envoyer</button>
</form>
```

Le serveur vérifie alors si le token envoyé correspond au token généré pour cette session.

#### **5.2. Vérification de l'origine ou du référent (Referer)**

Certaines applications peuvent vérifier l’en-tête **`Referer`** ou **`Origin`** pour s'assurer que la requête provient bien d'une page de la même application.

- **Referer** : Cet en-tête indique l’URL de la page à partir de laquelle la requête a été initiée.
- Si l’en-tête `Referer` ne correspond pas au domaine attendu (par exemple, si la requête provient d'un site externe), la requête peut être bloquée.

**Limitation** : Certains navigateurs ou configurations réseau peuvent supprimer ou modifier l’en-tête `Referer`, rendant cette méthode moins fiable que les tokens CSRF.

#### **5.3. Utilisation de cookies SameSite**
Les cookies peuvent être configurés avec l'attribut **`SameSite`**, qui empêche leur envoi lors de requêtes provenant d'un site externe.

- **SameSite=Lax** : Les cookies ne seront pas envoyés avec des requêtes `GET` initiées à partir de sites externes, mais seront envoyés pour des requêtes POST ou internes.
- **SameSite=Strict** : Les cookies ne seront envoyés que pour des requêtes venant du même site.

**Exemple de configuration d'un cookie avec l'attribut SameSite :**
```http
Set-Cookie: sessionID=abc123; SameSite=Strict;
```

---

### **6. Exercice pratique (optionnel)**

#### **Objectif** :
Simuler une attaque CSRF sur une application vulnérable et mettre en place des protections comme les tokens CSRF.

#### **Étapes** :
1. Accédez à une application vulnérable (par exemple, un environnement de test comme **DVWA**).
2. Essayez d’exploiter une fonctionnalité en initiant une requête CSRF malveillante.
3. Implémentez une protection CSRF, comme un token de sécurité, et observez comment la requête malveillante échoue.

---

### **7. Conclusion**
Les attaques **CSRF** sont subtiles mais peuvent être extrêmement dangereuses si elles sont exploitées. Elles peuvent permettre à un attaquant de manipuler des actions sensibles sur un site web sans que l'utilisateur ne s'en rende compte. Heureusement, des mesures comme les tokens CSRF et l’attribut `SameSite` pour les cookies offrent des protections robustes pour prévenir ce type d’attaque.

---

### **Ressources supplémentaires :**
- OWASP CSRF Prevention Cheat Sheet : [https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf)
- Documentation SameSite Cookie : [https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/Set-Cookie/SameSite](https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/Set-Cookie/SameSite)
