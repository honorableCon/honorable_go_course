# Chapitre 3 : Programmation Concurrentielle en Go

## Introduction à la Concurrence en Go

Go est conçu pour la programmation concurrentielle, offrant des primitives légères pour gérer les processus parallèles.

### Goroutines
Les goroutines sont des fonctions ou des méthodes exécutées de manière concurrente avec d'autres goroutines dans le même espace d'adressage.
```go
package main

import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go say("world")
    say("hello")
}
```
Dans cet exemple, la fonction `say` est exécutée de manière concurrente avec la goroutine principale.

### Canaux (Channels)
Les canaux permettent aux goroutines de communiquer et de synchroniser leurs exécutions en envoyant et recevant des valeurs de manière sécurisée.
```go
package main

import "fmt"

func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // envoie sum à c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int)
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y := <-c, <-c // reçoit de c

    fmt.Println(x, y, x+y)
}
```
Les canaux assurent la synchronisation en attendant que l'opération d'envoi ou de réception soit prête.

## Synchronisation

### Mutex
Le package `sync` fournit des primitives de synchronisation comme les mutex pour éviter les conditions de concurrence.
```go
package main

import (
    "fmt"
    "sync"
)

type SafeCounter struct {
    v   map[string]int
    mux sync.Mutex
}

func (c *SafeCounter) Inc(key string) {
    c.mux.Lock()
    c.v[key]++
    c.mux.Unlock()
}

func (c *SafeCounter) Value(key string) int {
    c.mux.Lock()
    defer c.mux.Unlock()
    return c.v[key]
}

func main() {
    c := SafeCounter{v: make(map[string]int)}
    for i := 0; i < 1000; i++ {
        go c.Inc("somekey")
    }

    fmt.Println(c.Value("somekey"))
}
```
Le verrouillage (`Lock`) et le déverrouillage (`Unlock`) garantissent que seules les goroutines autorisées peuvent accéder aux sections critiques du code.

### WaitGroups
Les `WaitGroups` permettent d'attendre la terminaison d'un ensemble de goroutines.
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d starting
", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d done
", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }

    wg.Wait()
}
```
Le `WaitGroup` attend que toutes les goroutines aient appelé `Done`.

### Sémaphores
Les sémaphores peuvent être utilisés pour limiter le nombre de goroutines simultanées.
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var sem = make(chan struct{}, 3)
    var wg sync.WaitGroup

    for i := 1; i <= 10; i++ {
        wg.Add(1)
        go func(i int) {
            defer wg.Done()
            sem <- struct{}{}
            fmt.Printf("Worker %d starting
", i)
            time.Sleep(time.Second)
            fmt.Printf("Worker %d done
", i)
            <-sem
        }(i)
    }

    wg.Wait()
}
```
Les sémaphores limitent le nombre de goroutines simultanées en utilisant un canal avec une capacité fixe.

