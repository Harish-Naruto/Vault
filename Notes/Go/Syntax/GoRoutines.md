[Go] #Golang #GoSyntax 
# Understanding Go Routines

A goroutine is a lightweight thread of execution managed by the Go runtime. They are the core building block for concurrency in Go, allowing you to run many tasks simultaneously.

## Concurrency vs. Parallelism

It's important to first understand the difference between concurrency and parallelism.

- **Concurrency** is about _dealing_ with a lot of things at once. It's about structure. Your program might be working on multiple tasks, switching between them as needed.
    
- **Parallelism** is about _doing_ a lot of things at once. It's about execution. This requires multiple CPU cores to run tasks simultaneously.
    

Go is designed for **concurrency**. It makes it easy to structure your program to handle many tasks. If you have multiple CPU cores, the Go runtime will automatically run your concurrent tasks in **parallel**.

## Why Use Goroutines?

- **Lightweight:** Goroutines are much cheaper than traditional OS threads. They start with a very small stack (a few KBs) that can grow and shrink as needed. You can easily have thousands or even millions of goroutines running.
    
- **Simple to Start:** Creating one is as simple as prefixing a function call with the `go` keyword.
    
- **Managed by Go Runtime:** The Go runtime has its own scheduler (an M:N scheduler) that multiplexes "M" goroutines onto "N" OS threads. This is far more efficient than relying on the OS to schedule thousands of threads.
    

## 1. Creating a Goroutine

You start a goroutine by using the `go` keyword before a function call.

```
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 3; i++ {
		fmt.Println(s)
		time.Sleep(100 * time.Millisecond)
	}
}

func main() {
	// Start a new goroutine
	go say("Hello")

	// The main function is also a goroutine!
	say("World")
}
```

**Problem with the above:** If you run this, you might just see "World" printed. Why?

When the `main` goroutine finishes, the program exits, and it doesn't wait for other goroutines to complete. In the example above, `go say("Hello")` starts, but the `main` goroutine finishes `say("World")` and then the program terminates, killing the "Hello" goroutine before it can finish.

## 2. Waiting for Goroutines: `sync.WaitGroup`

The standard way to wait for a collection of goroutines to finish is by using a `sync.WaitGroup`.

A `WaitGroup` is a counter. You tell it how many goroutines to wait for (`Add`), each goroutine signals when it's done (`Done`), and you can block until all are done (`Wait`).

1. `Add(n)`: Increases the counter by `n`. Call this _before_ starting your goroutines.
    
2. `Done()`: Decreases the counter by 1. Call this in the goroutine (usually with `defer`) when it's finished.
    
3. `Wait()`: Blocks the program until the counter reaches 0.
    

```
package main

import (
	"fmt"
	"sync"
	"time"
)

func worker(id int, wg *sync.WaitGroup) {
	// When the function exits, notify the WaitGroup that it's done.
	defer wg.Done()

	fmt.Printf("Worker %d starting\n", id)
	time.Sleep(time.Second) // Simulate work
	fmt.Printf("Worker %d done\n", id)
}

func main() {
	// Create a new WaitGroup
	var wg sync.WaitGroup

	// We are going to start 3 worker goroutines
	for i := 1; i <= 3; i++ {
		fmt.Printf("Starting worker %d\n", i)
		// Increment the WaitGroup counter
		wg.Add(1)
		// Start the goroutine, passing the WaitGroup pointer
		go worker(i, &wg)
	}

	fmt.Println("Waiting for all workers to finish...")
	// Block until the WaitGroup counter goes back to 0
	wg.Wait()

	fmt.Println("All workers completed.")
}
```

## 3. Common Gotchas

### Race Conditions

A race condition occurs when multiple goroutines try to access and modify the same variable at the same time.

```
// BAD EXAMPLE - RACE CONDITION
var counter int

func main() {
	var wg sync.WaitGroup
	wg.Add(2)

	go func() {
		defer wg.Done()
		counter++ // Read, increment, write
	}()

	go func() {
		defer wg.Done()
		counter++ // Read, increment, write
	}()

	wg.Wait()
	fmt.Println(counter) // Could be 1 or 2!
}
```

How to find: Use Go's built-in Race Detector.

go run -race main.go

**How to fix:**

1. **Use channels** to pass data (preferred).
    
2. **Use a `sync.Mutex`** (a "mutual exclusion" lock) to protect the data.
    

```
// Fixed with a Mutex
var counter int
var mu sync.Mutex

func main() {
	var wg sync.WaitGroup
	wg.Add(2)

	go func() {
		defer wg.Done()
		mu.Lock()   // Lock
		counter++ 
		mu.Unlock() // Unlock
	}()

	go func() {
		defer wg.Done()
		mu.Lock()   // Lock
		counter++
		mu.Unlock() // Unlock
	}()

	wg.Wait()
	fmt.Println(counter) // Always 2
}
```

### Goroutines and Loop Variables (The Classic Pitfall)

This code will probably print `3` (or `2`, or `1`) three times, not `0`, `1`, `2`.

```
// COMMON MISTAKE
func main() {
    var wg sync.WaitGroup
    for i := 0; i < 3; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            fmt.Println(i) // 'i' is shared and changes
        }()
    }
    wg.Wait()
}
```

**Why?** The `go func()` is a closure. It captures the _variable_ `i`, not its value. By the time the goroutines run, the loop has finished, and `i` is (most likely) 3.

**The Fix:** Pass the value as an argument to the goroutine.

```
// THE FIX
func main() {
    var wg sync.WaitGroup
    for i := 0; i < 3; i++ {
        wg.Add(1)
        // Pass 'i' as an argument
        go func(val int) {
            defer wg.Done()
            fmt.Println(val) // 'val' is a copy for this goroutine
        }(i) // Pass the current value of 'i'
    }
    wg.Wait()
}
```

## Summary

- Start goroutines with the `go` keyword.
- Use `sync.WaitGroup` to wait for goroutines to finish.
- Test for race conditions with `go run -race`.
- Be careful of closures and loop variables.