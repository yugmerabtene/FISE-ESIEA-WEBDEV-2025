### Table des matières du syllabus :
1. **Introduction à Spring Boot**
2. **Installation et configuration de Spring Boot**
3. **Structure d'un projet Spring Boot**
4. **Les contrôleurs et les routes**
5. **Les vues avec Thymeleaf**
6. **Gestion des bases de données avec Spring Data JPA**
7. **Validation des entrées avec Hibernate Validator**
8. **Sécurisation de l'application avec Spring Security**
9. **Gestion des erreurs et des exceptions**
10. **Tests avec Spring Boot**
11. **API REST avec Spring Boot**
12. **Déploiement et optimisation**

---

### 1. Introduction à Spring Boot

**Spring Boot** est un projet du framework Spring qui simplifie la création d'applications Java. Il permet de créer des applications Java robustes avec une configuration minimale. Spring Boot fournit une approche de convention plutôt que de configuration, ce qui permet de démarrer plus rapidement avec des applications Spring.

- **Spring Boot** permet de configurer des applications de manière autonome sans avoir besoin de déployer sur un serveur.
- Il simplifie l'intégration de diverses bibliothèques et services.
- **Spring Boot** offre des mécanismes de configuration et de gestion des propriétés.

### 2. Installation et configuration de Spring Boot

#### a. Créer un projet Spring Boot

Vous pouvez créer un projet Spring Boot de plusieurs manières. Voici comment faire via **Spring Initializr** (outil en ligne) ou avec **Maven**.

1. **Via Spring Initializr** :
   - Allez sur [Spring Initializr](https://start.spring.io/).
   - Choisissez les options suivantes :
     - Type de projet : **Maven** ou **Gradle**.
     - Langage : **Java**.
     - Version : **2.x.x** (la dernière version stable).
     - Dépendances : **Spring Web**, **Spring Data JPA**, **Thymeleaf**, **Spring Security** (si nécessaire).
     - Cliquez sur "Generate" pour télécharger le projet.

2. **Via Maven (CLI)** :

   Vous pouvez également utiliser Maven pour générer un projet Spring Boot en exécutant cette commande dans votre terminal (si Maven est installé) :

   ```bash
   mvn archetype:generate -DgroupId=com.example -DartifactId=my-springboot-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

   Ensuite, ajoutez les dépendances Spring Boot dans votre fichier `pom.xml`.

#### b. Lancer l'application Spring Boot

Voici un exemple de fichier de lancement de votre application. Dans Spring Boot, la classe principale contient la méthode `main` qui démarre le serveur intégré (Tomcat par défaut).

```java
package com.example.myapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```

La notation `@SpringBootApplication` active la configuration par défaut de Spring Boot.

Pour exécuter votre application, ouvrez un terminal et exécutez la commande suivante :

```bash
mvn spring-boot:run
```

Ou exécutez directement la classe Java dans votre IDE.

### 3. Structure d'un projet Spring Boot

Voici à quoi ressemble la structure de base d'un projet Spring Boot :

```
my-springboot-app/
├── src/main/java/com/example/myapp/
│   ├── MySpringBootApplication.java    # Point d'entrée de l'application
│   ├── controller/                     # Contrôleurs (gestion des routes)
│   ├── model/                          # Entités (Modèles de données)
│   ├── repository/                     # Repositories (Accès aux données)
│   ├── service/                        # Services (logique métier)
├── src/main/resources/
│   ├── application.properties          # Configuration de l'application
│   ├── templates/                      # Templates Thymeleaf (si utilisée)
│   └── static/                         # Fichiers statiques (CSS, JS, images)
└── pom.xml                             # Dépendances Maven
```

### 4. Les contrôleurs et les routes

Dans Spring Boot, les contrôleurs sont responsables de la gestion des requêtes HTTP. Les contrôleurs sont annotés avec `@RestController` pour les API REST ou `@Controller` pour les applications avec des vues.

#### a. Exemple d'un contrôleur avec `@RestController`

```java
package com.example.myapp.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

#### b. Exemple d'un contrôleur avec `@Controller` pour les vues

```java
package com.example.myapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("message", "Hello, Spring Boot with Thymeleaf!");
        return "index";
    }
}
```

#### c. Template Thymeleaf

Créez un fichier `src/main/resources/templates/index.html` pour Thymeleaf :

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Spring Boot Example</title>
</head>
<body>
    <h1 th:text="${message}"></h1>
</body>
</html>
```

### 5. Les vues avec Thymeleaf

Thymeleaf est le moteur de templates par défaut pour les vues dans Spring Boot. Pour utiliser Thymeleaf, vous devez ajouter cette dépendance dans votre `pom.xml` :

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### 6. Gestion des bases de données avec Spring Data JPA

Spring Boot intègre **Spring Data JPA** pour gérer l'accès aux bases de données.

#### a. Exemple d'entité JPA

```java
package com.example.myapp.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Article {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    private String content;

    // Getters et Setters
}
```

#### b. Repository

Créez un `Repository` pour accéder à la base de données.

```java
package com.example.myapp.repository;

import com.example.myapp.model.Article;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ArticleRepository extends JpaRepository<Article, Long> {
    // Vous pouvez ajouter des méthodes de recherche personnalisées ici
}
```

#### c. Exemple de service qui utilise le repository

```java
package com.example.myapp.service;

import com.example.myapp.model.Article;
import com.example.myapp.repository.ArticleRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class ArticleService {

    @Autowired
    private ArticleRepository articleRepository;

    public List<Article> getAllArticles() {
        return articleRepository.findAll();
    }
}
```

### 7. Validation des entrées avec Hibernate Validator

Spring Boot inclut le **Hibernate Validator** pour valider les entrées des utilisateurs.

#### a. Exemple d'entité avec validation

```java
package com.example.myapp.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.Size;

@Entity
public class Article {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank
    @Size(min = 5, max = 100)
    private String title;

    // Getters et Setters
}
```

#### b. Validation dans le contrôleur

```java
package com.example.myapp.controller;

import com.example.myapp.model.Article;
import com.example.myapp.service.ArticleService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;

import javax.validation.Valid;

@RestController
public class ArticleController {

    @Autowired
    private ArticleService articleService;

    @PostMapping("/article")
    public String createArticle(@Valid @RequestBody Article article, BindingResult result) {
        if (result.hasErrors()) {
            return "Error: Invalid input data";
        }
        articleService.save(article);
        return "Article created successfully!";
    }
}
```

### 8. Sécurisation de l'application avec Spring Security

**Spring Security** permet de gérer l'authentification et l'autorisation dans votre application.

#### a. Configuration de base pour Spring Security

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Dans votre fichier `application.properties`, configurez les utilisateurs par défaut :

```properties
spring.security.user.name=admin
spring.security.user.password=admin
```

### 9. Gestion des erreurs et des exceptions

Spring Boot permet de gérer les exceptions et erreurs via des contrôleurs d'exception.

#### a. Exemple de gestion des erreurs

```java
package com.example.myapp.controller;

import org.springframework.http.HttpStatus;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public String handleException(Exception e, Model model) {
        model.addAttribute("errorMessage", e.getMessage());
        return "error";
    }
}
```

### 10. Tests avec Spring Boot

Spring Boot fournit un excellent support pour les tests avec **JUnit** et **Spring Test**.

#### a. Exemple de test d'un contrôleur

```java
package com.example.myapp.controller;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest
public class HelloControllerTest {

    private MockMvc mockMvc;

    @Test
    public void hello() throws Exception {
        mockMvc.perform(get("/hello"))
                .andExpect(status().isOk());
    }
}
```

### 11. API REST avec Spring Boot

Spring Boot permet de créer des API RESTful facilement avec des annotations comme `@RestController`.

#### a. Exemple d'API REST

```java
package com.example.myapp.controller;

import com.example.myapp.model.Article;
import com.example.myapp.service.ArticleService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class ArticleController {

    private final ArticleService articleService;

    public ArticleController(ArticleService articleService) {
        this.articleService = articleService;
    }

    @GetMapping("/articles")
    public List<Article> getArticles() {
        return articleService.getAllArticles();
    }
}
```

### 12. Déploiement et optimisation

Pour déployer une application Spring Boot en production, vous pouvez la packager en un fichier **JAR** ou **WAR**. Utilisez cette commande pour construire le JAR :

```bash
mvn clean package
```

Ensuite, déployez-le sur un serveur ou une plateforme comme **Heroku**, **AWS**, **Google Cloud**, ou **Docker**.
