# Chapitre 2 : Concepts de Base de Go

## Types de Données
### Types de base
- **int, float, string, bool**

### Tableaux, slices, maps
- **Tableaux :**
```go
var arr [5]int
```
- **Slices :**
```go
var s []int = arr[1:4]
```
- **Maps :**
```go
var m map[string]int
```

## Structures et Interfaces
### Définition et utilisation des structures
```go
type Person struct {
    Name string
    Age  int
}
```

### Utilisation des interfaces
```go
type Animal interface {
    Speak() string
}
```

## Fonctions et Méthodes
### Définition et appel de fonctions
```go
func add(a int, b int) int {
    return a + b
}
```

### Méthodes de réception
```go
func (p Person) Greet() string {
    return "Hello, " + p.Name
}
```
