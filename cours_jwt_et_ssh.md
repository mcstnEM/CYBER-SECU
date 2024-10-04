## JWT et SSH

### Introduction

La sécurité des applications est un aspect crucial du développement moderne, qu'il s'agisse d'authentification, d'intégrité des données ou de protection contre les attaques externes. Ce cours se concentrera sur deux technologies de sécurité majeures : JSON Web Tokens (JWT) pour l'authentification et l'autorisation, et Secure Shell (SSH) pour la communication sécurisée à distance.

---

## Partie 1 : JSON Web Token (JWT)

### 1.1 Qu'est-ce qu'un JWT ?

Un **JSON Web Token (JWT)** est un standard ouvert (RFC 7519) permettant l'échange sécurisé d'informations entre deux parties sous forme d'un objet JSON. Ce format est souvent utilisé pour gérer l'authentification stateless (sans session) dans les API, car il permet de transmettre des informations sur l'utilisateur ou des droits d'accès dans un format compact et sécurisé.

#### Structure d'un JWT
Un JWT se compose de trois parties séparées par des points (`.`) :
1. **Header (en-tête)** : indique le type de token et l'algorithme de signature utilisé.
2. **Payload (charge utile)** : contient les informations (ou les "claims") sur l'utilisateur et les droits.
3. **Signature** : assure l'intégrité du token et garantit que les informations n'ont pas été modifiées.

Exemple de JWT :
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

#### Explication des parties :
- **Header** : 
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
- **Payload** :
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```
- **Signature** :
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret)
```

### 1.2 Fonctionnement du JWT

1. **Création** : Lorsqu'un utilisateur s'authentifie, le serveur génère un JWT contenant les informations d'identification de l'utilisateur (par ex. son ID) et le signe avec une clé secrète ou une clé privée.
2. **Transmission** : Le JWT est renvoyé au client, souvent sous forme de cookie ou via les headers HTTP (ex. `Authorization: Bearer <token>`).
3. **Vérification** : À chaque requête, le client envoie ce token au serveur. Le serveur vérifie la signature pour s'assurer que le token est valide et non altéré.
4. **Décodage** : Si la vérification réussit, le serveur peut extraire les informations du token (sans avoir à interroger une base de données).

### 1.3 Avantages et Inconvénients

#### Avantages :
- **Stateless** : Ne nécessite pas de maintenir une session côté serveur, ce qui simplifie la mise à l'échelle.
- **Compact** : Peut être facilement transmis dans les headers HTTP, URL ou cookies.
- **Flexible** : Utilisable dans différents types d’applications (API REST, SPA, etc.).

#### Inconvénients :
- **Non-révocable** : Une fois émis, il n'y a pas de moyen natif de révoquer un JWT. Il faut donc gérer une liste de tokens invalidés.
- **Sécurité des informations** : Les informations dans le payload ne sont pas chiffrées, seulement signées. Elles peuvent donc être lues si elles sont décryptées (à moins d'utiliser JWE pour le chiffrement).

### 1.4 Cas d'utilisation
- **Authentification stateless** : Pour les applications web où le serveur ne maintient pas d'état, les JWT sont souvent utilisés pour stocker les informations d'identification de l'utilisateur.
- **Partage sécurisé d'informations** : Les JWT permettent de partager des informations entre différentes parties, comme entre un fournisseur d'authentification et une application cliente.

---

## Partie 2 : Secure Shell (SSH)

### 2.1 Qu'est-ce que SSH ?

**SSH** (Secure Shell) est un protocole réseau cryptographique pour sécuriser les communications entre un client et un serveur. Il est principalement utilisé pour accéder de manière sécurisée à des machines distantes, exécuter des commandes, transférer des fichiers, et gérer des réseaux de manière cryptée.

### 2.2 Fonctionnement de SSH

Le fonctionnement de SSH repose sur une architecture **client-serveur** avec une authentification sécurisée par clé publique ou mot de passe, et un chiffrement pour protéger les données transmises.

#### 2.2.1 Authentification
SSH propose plusieurs méthodes d'authentification :
1. **Par mot de passe** : Le client fournit un mot de passe pour accéder au serveur.
2. **Par clé publique/privée** : Le client utilise une paire de clés (publique/privée) pour s'authentifier. La clé publique est stockée sur le serveur, tandis que la clé privée reste sur le client.

#### 2.2.2 Établissement d'une session
1. Le client démarre une session SSH en se connectant au serveur.
2. Le serveur envoie sa clé publique au client.
3. Le client génère une clé de session partagée qui est chiffrée avec la clé publique du serveur.
4. Une fois le chiffrement établi, toutes les communications entre le client et le serveur sont sécurisées par cette clé de session.

### 2.3 Avantages de SSH

- **Sécurité** : Les communications sont entièrement chiffrées, protégeant contre les attaques comme le "man-in-the-middle".
- **Authentification flexible** : La possibilité d'utiliser des clés privées pour éviter l'utilisation de mots de passe rend SSH plus sécurisé.
- **Portabilité** : SSH est disponible sur de nombreux systèmes d'exploitation, notamment Linux, macOS, et Windows (via OpenSSH).

### 2.4 Cas d'utilisation

- **Administration de serveurs** : SSH est principalement utilisé pour administrer des serveurs distants de manière sécurisée. Il permet d'exécuter des commandes, d'installer des logiciels et de surveiller des services.
- **Transfert sécurisé de fichiers** : Grâce à des outils comme SCP ou SFTP, SSH permet de transférer des fichiers de manière sécurisée entre machines.
- **Tunnels SSH** : Il permet de créer des tunnels pour sécuriser d'autres types de trafic (par exemple, protéger des connexions HTTP via SSH).

---

## Conclusion

JWT et SSH sont deux technologies essentielles pour assurer la sécurité des communications dans les applications modernes. Tandis que JWT se concentre sur l'authentification et l'autorisation des utilisateurs, SSH offre un moyen sécurisé de communiquer avec des serveurs à distance. Une bonne compréhension et une utilisation correcte de ces deux outils sont cruciales pour les développeurs et administrateurs système cherchant à garantir la sécurité de leurs infrastructures et applications.
