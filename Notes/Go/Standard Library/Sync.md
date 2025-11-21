#Golang #GoExtra 
## ⚙️ The `sync` Package: Go Synchronization Primitives

The built-in Go `sync` package provides the essential low-level synchronization primitives necessary for managing concurrent access to shared resources and coordinating the execution of goroutines.

While Go's idiomatic concurrency approach favors **channels** (communication), the `sync` package is indispensable when goroutines must access and modify the same memory (the "share memory" approach).

### 🧱 Core Primitives in `sync`

The package includes several tools, each designed for a specific synchronization task:

#### 1. `sync.Mutex` and `sync.RWMutex` (Mutual Exclusion)

- **Purpose:** To enforce mutual exclusion, ensuring only one goroutine can access a specific piece of shared data (the critical section) at a time.
    
- **`Mutex`:** A basic lock for exclusive read/write access.
    
- **`RWMutex`:** A more advanced lock that allows multiple **readers** simultaneously but only one **writer** (blocking all readers and other writers). Ideal for read-heavy data structures like caches.
    

#### 2. `sync.WaitGroup` (Orchestration/Waiting)

- **Purpose:** To wait for a collection of goroutines to finish executing. This is crucial for managing the lifetime of a concurrent task; the main goroutine can continue only after all its launched workers have completed.
    
- **Mechanism:** It operates like a counter:
    
    - `Add(delta)`: Increments the counter (usually called before launching a goroutine).
        
    - `Done()`: Decrements the counter (usually called via `defer` in the goroutine).
        
    - `Wait()`: Blocks the calling goroutine until the counter reaches zero.
        

#### 3. `sync.Once` (Singleton Initialization)

- **Purpose:** To ensure that a function is executed exactly once, regardless of how many goroutines attempt to call it concurrently.
    
- **Use Case:** Initializing global singletons, lazy loading of expensive resources, or setting up configurations that only need to run one time throughout the program's lifecycle.
    

### 📝 Example: Using `sync.WaitGroup`

The `WaitGroup` is the primary tool for coordinating when concurrent tasks are complete. This example ensures the `main` function does not exit before all three worker goroutines have finished their work.

```
package main

import (
	"fmt"
	"sync"
	"time"
)

func worker(id int, wg *sync.WaitGroup) {
	// The defer statement ensures wg.Done() is called 
	// when the function finishes, regardless of how it exits.
	defer wg.Done() 

	fmt.Printf("Worker %d: Starting job...\n", id)
	time.Sleep(time.Duration(id) * 50 * time.Millisecond)
	fmt.Printf("Worker %d: Finished job.\n", id)
}

func main() {
	var wg sync.WaitGroup
	numWorkers := 3

	for i := 1; i <= numWorkers; i++ {
		// 1. Increment the counter for each goroutine launched
		wg.Add(1) 
		
		go worker(i, &wg)
	}

	// 2. Block the main goroutine until the counter reaches zero
	fmt.Println("Main: Waiting for all workers to finish...")
	wg.Wait() 

	// 3. This line executes only after all workers have called wg.Done()
	fmt.Println("Main: All workers complete. Program exiting.")
}
```

### 💡 Summary

- Use `WaitGroup` for **orchestration** (making the main program wait for background tasks).
    
- Use `Mutex`/`RWMutex` for **protection** (safeguarding shared data from race conditions).
    
- Use `Once` for **initialization** (ensuring critical setup code runs only once).