# 🔥 1. Injection SQL (SQLi)

## 🚨 Définition
L’injection SQL (SQLi) permet à un attaquant d’exécuter des requêtes SQL malveillantes en manipulant une **entrée utilisateur non sécurisée**.

---

## 🕵️‍♂️ Exemple d’attaque SQLi
Prenons un script PHP vulnérable :
```php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=testdb", "root", "");

// Récupération d’un utilisateur sans protection
if (isset($_GET['id'])) {
    $id = $_GET['id'];
    $query = "SELECT * FROM users WHERE id = $id"; // ⚠️ Vulnérable !
    $stmt = $pdo->query($query);
    $user = $stmt->fetch();
    print_r($user);
}
?>
```

---

## 🔍 Tester la vulnérabilité
Dans l’URL, entrez :
```
http://site.com/vulnerable.php?id=1 OR 1=1
```
Ce qui exécute :
```sql
SELECT * FROM users WHERE id = 1 OR 1=1;
```
Cela **affichera tous les utilisateurs** au lieu d’un seul.

---

## 🛡️ Protection avec `PDO` et requêtes préparées
```php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=testdb", "root", "");
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

if (isset($_GET['id'])) {
    $id = $_GET['id'];
    $stmt = $pdo->prepare("SELECT * FROM users WHERE id = :id");
    $stmt->bindParam(':id', $id, PDO::PARAM_INT);
    $stmt->execute();
    $user = $stmt->fetch();
    print_r($user);
}
?>
```
**✅ Pourquoi ça marche ?**  
Les valeurs passées en `bindParam()` **ne sont pas interprétées** comme du SQL, empêchant ainsi toute injection.

---

# 🔥 2. Cross-Site Scripting (XSS)

## 🚨 Définition
Le **XSS** permet d’injecter du **JavaScript malveillant** dans une page visitée par un utilisateur.

---

## 🕵️‍♂️ Exemple d’attaque XSS
Un script vulnérable :
```php
<?php
if (isset($_GET['name'])) {
    $name = $_GET['name']; 
    echo "Bonjour, $name"; // ⚠️ Vulnérable !
}
?>
```

---

## 🔍 Tester la vulnérabilité
Dans l’URL, entrez :
```
http://site.com/xss.php?name=<script>alert('Hacké !')</script>
```
Cela affichera une **boîte d’alerte** !

---

## 🛡️ Protection avec `htmlspecialchars()`
```php
<?php
if (isset($_GET['name'])) {
    $name = htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8');
    echo "Bonjour, $name"; // ✅ Sécurisé
}
?>
```
**✅ Pourquoi ça marche ?**  
`htmlspecialchars()` transforme `<script>` en `&lt;script&gt;`, empêchant l’exécution du script.

---

# 🔥 3. Cross-Site Request Forgery (CSRF)

## 🚨 Définition
Le **CSRF** force un utilisateur connecté à exécuter **une action non voulue**.

---

## 🕵️‍♂️ Exemple d’attaque CSRF
Un formulaire vulnérable :
```php
<form action="delete_account.php" method="POST">
    <input type="hidden" name="user_id" value="1">
    <button type="submit">Supprimer mon compte</button>
</form>
```
Si l’utilisateur connecté visite un site malveillant contenant :
```html
<form action="https://site-legitime.com/delete_account.php" method="POST">
    <input type="hidden" name="user_id" value="1">
</form>
<script>document.forms[0].submit();</script>
```
Son compte sera **supprimé à son insu**.

---

## 🛡️ Protection avec un `csrf_token`
Ajoutez un **jeton CSRF** unique dans le formulaire :
```php
session_start();
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}
?>
<form action="delete_account.php" method="POST">
    <input type="hidden" name="csrf_token" value="<?php echo $_SESSION['csrf_token']; ?>">
</form>
```
Et vérifiez le token côté serveur :
```php
session_start();
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (!isset($_POST['csrf_token']) || $_POST['csrf_token'] !== $_SESSION['csrf_token']) {
        die("CSRF détecté !");
    }
}
```

---

# 🔥 OWASP
L’**OWASP** est une organisation qui recense les vulnérabilités majeures.

### **OWASP Top 10**
1. **Injection SQL**
2. **Authentification cassée**
3. **Exposition de données sensibles**
4. **XSS**
5. **Mauvaise configuration de sécurité**
6. **Désérialisation non sécurisée**
7. **Utilisation de composants vulnérables**
8. **Accès non autorisé**
9. **Mauvaise journalisation**
10. **Monitoring insuffisant**

📖 **Lien :** [https://owasp.org](https://owasp.org)

---

# 🔥 CVE (Common Vulnerabilities and Exposures)
Le **CVE** attribue un **identifiant unique** aux failles connues.

### **Exemple**
- **CVE-2021-44228 (Log4Shell)** : Une vulnérabilité critique permettant l’exécution de code à distance.

📖 **Consulter les CVE :** [https://cve.mitre.org](https://cve.mitre.org)

---

# 🔥 MITRE ATT&CK
Le **MITRE ATT&CK** est une **base de connaissances** des tactiques des attaquants.

### **Tactiques courantes**
- **Reconnaissance** (scan des ports, OSINT)
- **Exécution** (exploitation de vulnérabilités)
- **Élévation de privilèges** (exploitation de failles)
- **Persistance** (modification des services Windows)

📖 **Explorer MITRE ATT&CK :** [https://attack.mitre.org](https://attack.mitre.org)

---

# 🔥 OWASP ZAP (Zed Attack Proxy)

## 🚨 Définition
**ZAP** est un **outil open-source** utilisé pour **analyser la sécurité des applications web**.

### **Fonctionnalités**
✅ Détection automatique de failles XSS, SQLi, CSRF  
✅ Attaque par force brute sur des identifiants  
✅ Analyse complète du trafic HTTP  

---

## 🛠️ Comment tester une application avec ZAP ?
1️⃣ **Télécharger OWASP ZAP** : [https://www.zaproxy.org/download/](https://www.zaproxy.org/download/)  
2️⃣ **Lancer ZAP** et scanner une URL cible  
3️⃣ **Analyser les résultats** et corriger les vulnérabilités détectées  

📖 **Plus d’infos sur ZAP** : [https://www.zaproxy.org/](https://www.zaproxy.org/)
**Tuto pour realiser une attaque de type injection SQL avec zap : https://asafsahar25.medium.com/testing-for-sql-injection-vulnerabilities-using-owasp-zap-62488b5805f0**

---

# ✅ Bonnes Pratiques en Sécurité
1. **Utiliser `htmlspecialchars()`** contre XSS  
2. **Utiliser `PDO` et requêtes préparées** contre SQLi  
3. **Ajouter un `csrf_token`** dans les formulaires  
4. **Utiliser OWASP ZAP pour tester les failles**  
