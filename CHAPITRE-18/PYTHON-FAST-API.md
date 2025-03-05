### Table des matières
1. **Introduction à FastAPI**
2. **Installation et configuration**
3. **Créer une première API**
4. **Les routes dans FastAPI**
5. **Les requêtes et réponses**
6. **Modèles de données avec Pydantic**
7. **Validation automatique et gestion des erreurs**
8. **Documentation automatique**
9. **Tests avec FastAPI**
10. **Sécurisation de l'API**

### 1. Introduction à FastAPI

**FastAPI** est un framework moderne pour construire des API RESTful avec Python. Il se distingue par ses performances élevées et sa facilité d'utilisation, notamment grâce à une gestion automatique des types et une documentation interactive générée automatiquement. FastAPI est conçu pour être simple et rapide tout en étant robuste, ce qui en fait une excellente option pour créer des APIs.

### 2. Installation et configuration

Pour installer **FastAPI**, vous devez d'abord installer **FastAPI** et **Uvicorn**, qui est le serveur ASGI qui permet d'exécuter les applications FastAPI.

```bash
pip install fastapi uvicorn
```

Ensuite, pour démarrer le serveur, vous pouvez exécuter la commande suivante en ligne de commande :

```bash
uvicorn main:app --reload
```

Ici, `main` fait référence au fichier Python (le fichier contenant votre application) et `app` est l'instance de l'application FastAPI.

### 3. Créer une première API

Un fichier simple `main.py` pourrait ressembler à ceci :

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

Explication :
- **FastAPI()** : Crée une instance de l'application FastAPI.
- **@app.get("/")** : Un décorateur qui définit une route HTTP GET à la racine.
- La fonction `read_root` retourne une réponse JSON lorsque l'utilisateur accède à cette route.

Pour démarrer l'application, exécutez la commande suivante :

```bash
uvicorn main:app --reload
```

Vous pouvez maintenant ouvrir un navigateur à l'adresse `http://127.0.0.1:8000/` pour voir le message "Hello, World!" dans le format JSON.

### 4. Les routes dans FastAPI

FastAPI vous permet de définir des routes pour différents types de requêtes HTTP, comme GET, POST, PUT, DELETE, etc. Voici quelques exemples :

#### Route GET
```python
@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```
Cette route prend un paramètre de chemin (`item_id`) et un paramètre de requête optionnel (`q`).

#### Route POST
```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str = None

@app.post("/items/")
def create_item(item: Item):
    return {"name": item.name, "description": item.description}
```
Ici, **Item** est un modèle Pydantic qui valide les données envoyées dans la requête POST.

### 5. Les requêtes et réponses

FastAPI simplifie la gestion des données en entrée et en sortie grâce à la validation automatique.

#### Réception des données (request body)

Lorsque vous créez des routes qui acceptent des données, vous pouvez utiliser Pydantic pour valider ces données. Par exemple, pour une route POST :

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/")
def create_item(item: Item):
    return {"name": item.name, "price": item.price}
```

#### Réponses

Les réponses peuvent être retournées sous forme de dictionnaire ou d'objets. Si vous devez personnaliser le code de statut ou le type de réponse, vous pouvez utiliser `JSONResponse` :

```python
from fastapi.responses import JSONResponse

@app.get("/custom-response")
def custom_response():
    return JSONResponse(content={"message": "Custom response!"}, status_code=200)
```

### 6. Modèles de données avec Pydantic

**Pydantic** est utilisé dans FastAPI pour la validation des données. Vous définissez des modèles de données en créant des classes qui héritent de `BaseModel`.

Exemple de modèle Pydantic :

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str = None
    price: float
```

Ici, `Item` est une classe qui valide que le nom est une chaîne, la description est une chaîne ou `None`, et le prix est un nombre flottant.

### 7. Validation automatique et gestion des erreurs

FastAPI effectue une validation automatique des données entrantes. Si les données ne respectent pas le modèle attendu, FastAPI retournera une erreur automatiquement.

Exemple d'erreur pour un modèle invalide :

```json
{
  "detail": [
    {
      "loc": ["body", "item", "price"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

Vous pouvez également gérer les erreurs personnalisées avec des exceptions comme `HTTPException`.

```python
from fastapi import HTTPException

@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id != 42:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"item_id": item_id}
```

### 8. Documentation automatique

L'une des grandes forces de FastAPI est la documentation automatique générée via **Swagger** et **ReDoc**. FastAPI génère automatiquement une interface interactive pour tester l'API. Il vous suffit d'aller à `http://127.0.0.1:8000/docs` pour voir la documentation Swagger ou à `http://127.0.0.1:8000/redoc` pour ReDoc.

### 9. Tests avec FastAPI

FastAPI fournit une excellente prise en charge des tests grâce à `TestClient` intégré à `Starlette`. Voici un exemple de test avec **pytest** :

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello, World!"}
```

### 10. Sécurisation de l'API

Pour sécuriser une API, vous pouvez utiliser des dépendances comme l'authentification via **OAuth2**, **JWT**, ou des mécanismes comme **API keys**. Voici un exemple avec un schéma de sécurité de type **OAuth2** :

```python
from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

app = FastAPI()

@app.get("/items/")
def read_items(token: str = Depends(oauth2_scheme)):
    return {"token": token}
```

RESSOURCES : 

https://www.youtube.com/watch?v=aSdVU9-SxH4  
https://www.youtube.com/watch?v=lWsGhG6N_1E
