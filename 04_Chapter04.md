# Chapter 4: Waiting and Guarding - The Tale of WaitGroups and Semaphores

In our previous chapter, we learned about the importance of synchronization and mutual exclusion when dealing with concurrent operations in Golang. We discovered how essential it is to prevent race conditions and ensure the consistency and integrity of our code.

Now, it's time to explore the next level of concurrency in Go - waiting and guarding. In this chapter, we will delve into the fascinating world of **WaitGroups** and **Semaphores**.

Just like the Greeks had to navigate through the labyrinth of King Minos to reach their goal, so too must we use WaitGroups and Semaphores to guide our concurrent operations to the finish line.

The concept of waiting and guarding is critical when dealing with concurrent operations that have dependencies. We cannot proceed to the next task until the previous one has completed. This is where WaitGroups come in. They allow us to wait for a set of goroutines to finish their execution before continuing with our main thread.

In contrast, semaphores help us to limit the number of concurrent operations that can access a shared resource. Picture the god of war, Ares, guarding the entrance to Troy - he permits only a limited number of soldiers to pass through the gates at any given time. Semaphores have similar functionality in the world of Go.

With WaitGroups and Semaphores in our arsenal, we have the tools we need to conquer concurrent operations in Golang. So tighten your sandals, sharpen your sword, and let's dive into the epic tale of WaitGroups and Semaphores!
# Chapter 4: Waiting and Guarding - The Tale of WaitGroups and Semaphores

In ancient Greece, there was a great hero named Theseus who sought to slay the mighty Minotaur, a monster with the head of a bull and body of a man who dwelled at the center of a labyrinth.

Theseus was determined to reach his goal, but the labyrinth was fraught with danger and obstacles that threatened to impede his progress. However, Theseus was clever and resourceful. He knew that he could not move forward until he had conquered every obstacle in his path and reached his objective.

This is much like working with concurrent operations in Golang. We need to ensure that all of our concurrent operations have completed before proceeding to the next step, just like Theseus navigating through the labyrinth.

Luckily, there are two powerful tools at our disposal in Golang - WaitGroups and Semaphores - that can guide and safeguard us through the perilous landscape of concurrent operations.

The story goes that Theseus encountered an army of soldiers blocking his path. He knew that he could not defeat them all at once, so he devised a plan to limit their access to him. He had his trusted companion, Pylades, guard the entrance while he fought the soldiers, one by one. He instructed Pylades to allow only one soldier to approach at a time until Theseus had defeated them all.

This is precisely what Semaphores can do in Golang. They guard and limit access to shared resources so that only one concurrent operation can access it at any given time, preventing race conditions and ensuring consistency.

Similarly, WaitGroups allow us to wait for concurrent operations to complete their execution before proceeding with our main thread. This is similar to Theseus waiting for Pylades to signal him that he had defeated one soldier before moving on to the next one.

With WaitGroups and Semaphores as our companions, we, too, can conquer the labyrinth of concurrent operations in Golang, just like Theseus conquered the labyrinth of King Minos.

May the gods of concurrency guide and safeguard us on our journey, and let us emerge victorious on the other side!
# Explanation of Code - Chapter 4: WaitGroups and Semaphores

Now that we have explored the tale of Theseus and how it relates to WaitGroups and Semaphores, let's take a closer look at the code that helps make our concurrency safe and efficient.

## WaitGroups

A WaitGroup waits for a set of goroutines to finish their execution before allowing the main thread to proceed. This is immensely useful when dealing with concurrent operations that rely on an order of execution. In Golang, we create a new WaitGroup using the `sync` package:

```go
import "sync"

var wg sync.WaitGroup
```

Then, we can use the `Add()` function to specify the number of goroutines we want to wait for, and the `Done()` function to signal that a goroutine has completed its execution:

```go
func worker(id int) {
  defer wg.Done()
  // code for goroutine
}

for i := 0; i < numWorkers; i++ {
  wg.Add(1)
  go worker(i)
}

wg.Wait() // waits for all goroutines to finish
```

Here, we have a `worker` function that we want to run concurrently, and we want to ensure that all workers have finished before continuing with our main program. So, we use `wg.Add()` to add each worker to the WaitGroup, and `wg.Done()` in the worker function to signal that it has finished. Finally, we use `wg.Wait()` to block the main thread until all workers have completed their execution.

## Semaphores

A Semaphore limits the number of concurrent operations that can access a shared resource, similar to how Pylades guarded the entrance to Theseus in our epic. In Golang, Semaphores can be implemented with channels and `go` routines:

```go
c := make(chan struct{}, maxConcurrency)

func worker(id int) {
  c <- struct{}{} // acquire lock
  // code for goroutine
  <-c // release lock
}

for i := 0; i < numWorkers; i++ {
  go worker(i)
}
```

Here, we create a buffered channel `c` with a capacity of `maxConcurrency`, which will limit the number of concurrent operations that can proceed at any given time. In the `worker` function, we use the `<-c` channel operator to acquire the lock and ensure that a limited number of workers can execute simultaneously. Once the goroutine completes its execution, it releases the lock by using the `c <- struct{}{}` channel operator.

Using WaitGroups and Semaphores in our code can make executing concurrent operations much safer and efficient, much like how Theseus and Pylades worked together to navigate through the labyrinth. By leveraging the power of Golang's concurrency tools, we can conquer the challenges of concurrent programming with ease.


[Next Chapter](05_Chapter05.md)