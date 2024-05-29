# Chapitre 4 : Développement Backend avec Go

## Introduction au Développement Web

### Serveur HTTP basique
Go propose le package `net/http` pour créer des serveurs web et gérer les requêtes HTTP.
```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```
Ce code crée un serveur HTTP qui répond "Hello, World!" à chaque requête.

### Gestion des requêtes et réponses
Vous pouvez gérer différentes routes et réponses en utilisant des multiplexer de requêtes.
```go
package main

import (
    "fmt"
    "net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func goodbyeHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Goodbye, World!")
}

func main() {
    http.HandleFunc("/hello", helloHandler)
    http.HandleFunc("/goodbye", goodbyeHandler)
    http.ListenAndServe(":8080", nil)
}
```
Ce code gère les requêtes pour `/hello` et `/goodbye`.

## Création d'une API REST

### Structurer une application web
Organisez votre code en packages pour une meilleure maintenabilité.
```
myapp/
├── main.go
├── handlers/
│   └── handlers.go
└── models/
    └── models.go
```
Exemple de `handlers/handlers.go` :
```go
package handlers

import (
    "encoding/json"
    "net/http"
)

type Response struct {
    Message string `json:"message"`
}

func HelloHandler(w http.ResponseWriter, r *http.Request) {
    response := Response{Message: "Hello, World!"}
    json.NewEncoder(w).Encode(response)
}
```

### Gestion des routes et middlewares
Utilisez des frameworks web comme Gin ou Echo pour une gestion avancée des routes et middlewares.
```go
package main

import (
    "github.com/gin-gonic/gin"
)

func main() {
    router := gin.Default()
    router.GET("/hello", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "Hello, World!",
        })
    })
    router.Run(":8080")
}
```
Gin simplifie la création de routes et de middlewares.

### Gestion des JSON et des données
Utilisez `encoding/json` pour le traitement des données JSON.
```go
package main

import (
    "encoding/json"
    "net/http"
)

type Response struct {
    Message string `json:"message"`
}

func handler(w http.ResponseWriter, r *http.Request) {
    response := Response{Message: "Hello, World!"}
    json.NewEncoder(w).Encode(response)
}

func main() {
    http.HandleFunc("/hello", handler)
    http.ListenAndServe(":8080", nil)
}
```
Ce code encode la structure `Response` en JSON.

## Interaction avec la Base de Données

### Connexion à une base de données
Utilisez le package `database/sql` pour se connecter à une base de données.
```go
package main

import (
    "database/sql"
    "log"

    _ "github.com/lib/pq"
)

func main() {
    connStr := "user=username dbname=mydb sslmode=disable"
    db, err := sql.Open("postgres", connStr)
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()
}
```
Ce code se connecte à une base de données PostgreSQL.

### ORM avec GORM
Utilisez GORM pour simplifier les interactions avec la base de données.
```go
package main

import (
    "github.com/jinzhu/gorm"
    _ "github.com/jinzhu/gorm/dialects/postgres"
)

type User struct {
    gorm.Model
    Name  string
    Email string
}

func main() {
    db, err := gorm.Open("postgres", "host=localhost user=username dbname=mydb sslmode=disable")
    if err != nil {
        panic("failed to connect database")
    }
    defer db.Close()

    db.AutoMigrate(&User{})

    db.Create(&User{Name: "John", Email: "john@example.com"})
}
```
Ce code utilise GORM pour créer une table `User` et insérer un enregistrement.

### Requêtes SQL et gestion des transactions
Effectuez des requêtes SQL et gérez les transactions avec `database/sql`.
```go
package main

import (
    "database/sql"
    "log"

    _ "github.com/lib/pq"
)

func main() {
    connStr := "user=username dbname=mydb sslmode=disable"
    db, err := sql.Open("postgres", connStr)
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    tx, err := db.Begin()
    if err != nil {
        log.Fatal(err)
    }

    _, err = tx.Exec("INSERT INTO users(name, email) VALUES($1, $2)", "Alice", "alice@example.com")
    if err != nil {
        tx.Rollback()
        log.Fatal(err)
    }

    err = tx.Commit()
    if err != nil {
        log.Fatal(err)
    }
}
```
Ce code montre comment exécuter une transaction et effectuer une insertion dans une table.

