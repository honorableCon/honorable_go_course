# Chapitre 1 : Introduction à Go

## Introduction à Go

### Historique et Caractéristiques de Go
- **Historique :** Go a été développé par Google en 2007 par Robert Griesemer, Rob Pike, et Ken Thompson. Il a été conçu pour améliorer la productivité des développeurs travaillant sur de grands projets.
- **Caractéristiques :** Go est un langage compilé, typé statiquement, conçu pour la simplicité, la performance, et la gestion de la concurrence.

### Comparaison avec d'autres langages
- **JavaScript/NestJS :** NestJS est basé sur TypeScript/JavaScript, des langages dynamiques, alors que Go est typé statiquement. Go est plus performant et simplifie la gestion de la concurrence.
- **Python :** Python est interprété et typé dynamiquement, ce qui le rend plus flexible mais moins performant que Go.

## Installation et Configuration

### Installation de Go
- Téléchargez Go depuis [le site officiel](https://golang.org/dl/).
- Suivez les instructions d'installation pour votre système d'exploitation (Windows, macOS, Linux).

### Configuration de l'environnement de développement
- Configurez votre `GOPATH` et `GOROOT`.
- Ajoutez Go à votre `PATH`.

### Utilisation de l'éditeur de code
- **VSCode :** Installez l'extension Go pour Visual Studio Code.
- **GoLand :** IDE de JetBrains spécialement conçu pour Go.

## Premiers Pas avec Go

### Structure d'un programme Go
Chaque programme Go commence par le paquet `main` et la fonction `main`.

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Syntaxe de base
- **Variables :**
```go
var x int = 10
y := 20
```
- **Fonctions :**
```go
func add(a int, b int) int {
    return a + b
}
```
- **Boucles et conditions :**
```go
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

if x > y {
    fmt.Println("x is greater than y")
} else {
    fmt.Println("y is greater than x")
}
```

### Gestion des erreurs
Utilisation des erreurs en tant que valeur :

```go
file, err := os.Open("filename.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()
```
