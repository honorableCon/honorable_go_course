# Chapitre 2 : Concepts de Base de Go

## Types de Données

### Types de base
Go propose plusieurs types de base :
- **int** : représente les entiers (positifs et négatifs)
- **float** : représente les nombres à virgule flottante
- **string** : représente les chaînes de caractères
- **bool** : représente les valeurs booléennes (true ou false)

Exemples :
```go
var age int = 30
var height float64 = 1.75
var name string = "Alice"
var isStudent bool = true
```

### Tableaux, slices, maps
Go propose des structures de données comme les tableaux, les slices et les maps pour gérer les collections de données.

#### Tableaux
Un tableau est une collection d'éléments de même type avec une taille fixe.
```go
var arr [5]int
arr[0] = 10
arr[1] = 20
```

#### Slices
Un slice est une séquence dynamique d'éléments de même type. Contrairement aux tableaux, les slices peuvent changer de taille.
```go
var s []int = []int{1, 2, 3, 4, 5}
s = append(s, 6)
```

#### Maps
Une map est une collection de paires clé-valeur. Les clés doivent être de type comparable, et les valeurs peuvent être de n'importe quel type.
```go
var m map[string]int = map[string]int{"Alice": 25, "Bob": 30}
m["Charlie"] = 35
```

## Structures et Interfaces

### Définition et utilisation des structures
Les structures (structs) sont des types de données composites qui regroupent des champs sous un même type.
```go
type Person struct {
    Name string
    Age  int
}

func main() {
    var p Person
    p.Name = "Alice"
    p.Age = 30
    fmt.Println(p)
}
```

### Utilisation des interfaces
Les interfaces sont des types abstraits qui spécifient un ensemble de méthodes qu'un type doit implémenter.
```go
type Animal interface {
    Speak() string
}

type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return "Woof!"
}

func main() {
    var a Animal = Dog{Name: "Buddy"}
    fmt.Println(a.Speak())
}
```

## Fonctions et Méthodes

### Définition et appel de fonctions
Les fonctions sont des blocs de code réutilisables qui effectuent une tâche spécifique.
```go
func add(a int, b int) int {
    return a + b
}

func main() {
    result := add(3, 4)
    fmt.Println(result)  // Affiche 7
}
```

### Méthodes de réception
Les méthodes sont des fonctions associées à des types de structure spécifiques. Elles permettent de définir des comportements pour les types.
```go
type Person struct {
    Name string
}

func (p Person) Greet() string {
    return "Hello, " + p.Name
}

func main() {
    p := Person{Name: "Alice"}
    fmt.Println(p.Greet())  // Affiche "Hello, Alice"
}
```
