### **1xx - Réponses informatives**
- **100 Continue** : Le serveur a reçu les en-têtes de la requête et attend le corps de la requête.
- **101 Switching Protocols** : Le serveur accepte de changer de protocole.
- **102 Processing** : Le serveur traite la requête, mais aucune réponse n’est disponible pour l’instant.
- **103 Early Hints** : Le serveur envoie des en-têtes avant la réponse finale.

### **2xx - Succès**
- **200 OK** : La requête a réussi.
- **201 Created** : La ressource a été créée avec succès.
- **202 Accepted** : La requête a été acceptée, mais pas encore traitée.
- **203 Non-Authoritative Information** : Réponse modifiée par un proxy.
- **204 No Content** : Pas de contenu à renvoyer.
- **205 Reset Content** : Demande au client de réinitialiser le document d’affichage.
- **206 Partial Content** : Réponse partielle (utilisée pour le téléchargement de fichiers).
- **207 Multi-Status** : Réponse multiple (WebDAV).
- **208 Already Reported** : La ressource a déjà été signalée (WebDAV).
- **226 IM Used** : Résultat d’une requête utilisant un mécanisme de transformation (Delta encoding).

### **3xx - Redirections**
- **300 Multiple Choices** : Plusieurs options possibles pour la requête.
- **301 Moved Permanently** : La ressource a été déplacée définitivement.
- **302 Found** : La ressource a été déplacée temporairement.
- **303 See Other** : Redirection vers une autre ressource.
- **304 Not Modified** : La ressource n’a pas été modifiée (cache).
- **305 Use Proxy** *(obsolète)* : La ressource doit être accédée via un proxy.
- **306 Switch Proxy** *(non utilisé)*.
- **307 Temporary Redirect** : Redirection temporaire (méthode de requête non modifiée).
- **308 Permanent Redirect** : Redirection permanente (méthode de requête non modifiée).

### **4xx - Erreurs client**
- **400 Bad Request** : Mauvaise syntaxe de la requête.
- **401 Unauthorized** : Authentification requise.
- **402 Payment Required** *(expérimental)*.
- **403 Forbidden** : Accès refusé.
- **404 Not Found** : Ressource non trouvée.
- **405 Method Not Allowed** : Méthode HTTP non autorisée.
- **406 Not Acceptable** : Le serveur ne peut pas fournir une réponse acceptable.
- **407 Proxy Authentication Required** : Authentification requise via un proxy.
- **408 Request Timeout** : Temps d’attente de la requête dépassé.
- **409 Conflict** : Conflit avec l’état actuel de la ressource.
- **410 Gone** : La ressource n’est plus disponible.
- **411 Length Required** : La requête nécessite un en-tête `Content-Length`.
- **412 Precondition Failed** : Une précondition a échoué.
- **413 Payload Too Large** : La charge utile est trop volumineuse.
- **414 URI Too Long** : L’URI est trop longue.
- **415 Unsupported Media Type** : Type de média non supporté.
- **416 Range Not Satisfiable** : Plage demandée non satisfiable.
- **417 Expectation Failed** : L’attente spécifiée ne peut être satisfaite.
- **418 I'm a teapot** *(Easter Egg - RFC 2324)*.
- **421 Misdirected Request** : Requête mal dirigée.
- **422 Unprocessable Entity** : Requête bien formée, mais non traitable (WebDAV).
- **423 Locked** : Ressource verrouillée (WebDAV).
- **424 Failed Dependency** : Dépendance échouée (WebDAV).
- **425 Too Early** : Requête envoyée trop tôt (expérimental).
- **426 Upgrade Required** : Le client doit passer à un protocole plus récent.
- **428 Precondition Required** : Précondition requise.
- **429 Too Many Requests** : Trop de requêtes envoyées en un court laps de temps.
- **431 Request Header Fields Too Large** : Les en-têtes de la requête sont trop volumineux.
- **451 Unavailable For Legal Reasons** : Accès à la ressource interdit pour raisons légales.

### **5xx - Erreurs serveur**
- **500 Internal Server Error** : Erreur interne du serveur.
- **501 Not Implemented** : Fonctionnalité non implémentée.
- **502 Bad Gateway** : Mauvaise réponse d’un serveur en amont.
- **503 Service Unavailable** : Service temporairement indisponible.
- **504 Gateway Timeout** : Temps d’attente expiré pour une réponse du serveur en amont.
- **505 HTTP Version Not Supported** : Version HTTP non supportée.
- **506 Variant Also Negotiates** : Erreur de configuration de négociation de contenu.
- **507 Insufficient Storage** : Espace insuffisant (WebDAV).
- **508 Loop Detected** : Boucle infinie détectée (WebDAV).
- **510 Not Extended** : Extensions requises manquantes.
- **511 Network Authentication Required** : Authentification réseau requise.
