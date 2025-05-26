# Go Channel
## Simple Example
```go
package main

import (
	"fmt"
)

func main() {
	// Create a channel of type int
	ch := make(chan int)

	// Start a goroutine that sends data into the channel
	go func() {
		fmt.Println("Sending 42 to channel")
		ch <- 42 // send value into the channel
	}()

	// Receive the value from the channel
	val := <-ch
	fmt.Println("Received:", val)
}

```

## Complex Example
```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	rand.Seed(time.Now().UnixNano())

	// Step 1: Generate numbers
	numbers := make(chan int, 10)
	go func() {
		for i := 0; i < 10; i++ {
			num := rand.Intn(100)
			fmt.Println("Generated:", num)
			numbers <- num
		}
		close(numbers) // close when done
	}()

	// Step 2: Workers (fan-out)
	squares := make(chan int, 10)
	for i := 0; i < 3; i++ {
		go worker(i, numbers, squares)
	}

	// Step 3: Fan-in (collect results)
	go func() {
		time.Sleep(1 * time.Second) // simulate delay
		close(squares)
	}()

	// Step 4: Print results
	for sq := range squares {
		fmt.Println("Result:", sq)
	}
}

func worker(id int, input <-chan int, output chan<- int) {
	for num := range input {
		fmt.Printf("Worker %d processing %d\n", id, num)
		time.Sleep(200 * time.Millisecond) // simulate work
		output <- num * num
	}
}

```

# Go WaitGroup and MutexLock

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	val := &AtomicInt{}
	var wg sync.WaitGroup

	for i := 0; i < 10000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			val.Inc()
		}()
	}

	wg.Wait()
	fmt.Println(val.val)
}

func slowPrint(txt int) {
	time.Sleep(100 * time.Millisecond)
	fmt.Println(txt)
}

type AtomicInt struct {
	mu  sync.Mutex
	val int
}

func (a *AtomicInt) Inc() {
	a.mu.Lock()
	time.Sleep(100 * time.Millisecond) // simulate slow operation
	a.val++
	a.mu.Unlock()
}

```


# GoLang RWLock
```go
package main

import (
	"fmt"
	"sync"
)

type SafeCounter struct {
	mu  sync.RWMutex
	val int
}

func (c *SafeCounter) Increment() {
	c.mu.Lock()        // Exclusive lock for write
	c.val++
	c.mu.Unlock()
}

func (c *SafeCounter) Value() int {
	c.mu.RLock()       // Shared lock for read
	defer c.mu.RUnlock()
	return c.val
}

func main() {
	counter := SafeCounter{}
	var wg sync.WaitGroup

	// Launch 1000 goroutines to increment
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			counter.Increment()
		}()
	}
	
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			fmt.Println(counter.Value())
		}()
	}

	wg.Wait()

	// Read the final value safely
	fmt.Println("Final Counter Value:", counter.Value())
}

```
