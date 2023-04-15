# Chapter 11: Fanning Out and Fanning In Concurrency Patterns

*Enter the world of ancient Greek mythology, where the gods and goddesses reign supreme, and the word ‘concurrency’ is synonymous with the epic battles that characterize this mythological universe. In this chapter, we will explore the fanning out and fanning in concurrency patterns and unravel their mysteries.*

But before we set foot on this journey, let us recount the events of the previous chapter, where we explored race conditions and data races. Rob Pike, our special guest, helped us understand the importance of data synchronization in Go programming. Rob Pike, a well-known computer scientist, one of the original creators of the Go programming language, and a luminary in the field of concurrency, will, once again, guide us on this epic quest.

Now, let us set our sights on fanning out and fanning in, two powerful concurrency patterns. Fanning out is a pattern where a single task is split into multiple parallel sub-tasks or threads that work independently, while fanning in is precisely the opposite. We merge the output of each sub-task or thread into a single task or thread, which results in a faster and more efficient process.

This pattern is useful when we need to divide a significant task into smaller sub-processes that can be completed independently, and these sub-tasks can be performed simultaneously in parallel. By dividing labor, we can complete the task more efficiently and quickly.

In Go, we can use channels to coordinate the interaction between fanned-out goroutines. Rob Pike, our knowledgeable guide, will provide us with examples and anecdotes that will help us make the jump from the mythological universe to the world of Go programming. 

So, let us put on our helmets, and pick up our swords, as we venture into the world of fanning out and fanning in concurrency patterns.
# Chapter 11: Fanning Out and Fanning In Concurrency Patterns

## The Quest for Efficiency

As the sun rose over Mount Olympus, the gods and goddesses were already buzzing with excitement. They had heard rumors of a powerful new tool that could increase their efficiency tenfold. Their curiosity aroused, they gathered at the feet of Zeus, the king of the gods.

"Dear father," asked Athena, the goddess of wisdom, "We have heard that there are new concurrency patterns that we can use to increase our efficiency. Pray tell us about them."

Zeus thought for a moment, stroking his beard, and then spoke. "My dear Athena, you speak of fanning out and fanning in, powerful concurrency patterns that can divide a significant task into smaller sub-processes, and allow us to complete our work more efficiently."

Hermes, the messenger of the gods interjected, "But how do we do it? We cannot possibly multi-task like mere mortals!"

Zeus smiled. "Ah, that is where our special guest comes in - Rob Pike, the concurrency master. He will guide us through this quest and show us the ways of fanning out and fanning in."

As they traveled through the heavens, they came across Rob Pike, who greeted them warmly, and led them to a magnificent temple. 

Rob Pike began, "My dear gods and goddesses, let me tell you a story that will help you understand the powers of fanning out and fanning in."

## The Tale of the Heroic Thread

In the ancient times, there lived a powerful hero named Thread - a skilled warrior who could accomplish any task with ease. One day, Thread was tasked with scaling a great mountain to retrieve a magical object. However, the task was too daunting and one hero alone could not complete it.

Thread, being a wise hero, looked to the skies and prayed to the gods for assistance. The gods heard his plea and granted him a power that would change his fate forever. They gave him a magic sword that could split into several blades, each with the same power as the original. Thread then proceeded to split his sword into multiple blades and sent them out to climb the mountain.

Each blade traversed a different path, cutting through obstacles and conquering steep cliffs with ease. As each blade reached the top, they collected each other's shard, forged it into a stronger blade, and returned to aid their comrades.

The united force of the blades of Thread proved to be mighty, and they were successful in retrieving the magical object. Thread returned home victorious, and from that day on, Thread was known as the hero who conquered the mountain.

And so, the gods and goddesses finally understood the power of fanning out and fanning in - a power that could transform any impossible task into a conquerable one.

Rob Pike smiled at the gods and goddesses, "And that is the tale of the heroic thread, a tale that teaches us the power of fanning out and fanning in. By dividing a task into smaller sub-tasks and merging their results, we can complete any significant task, no matter how daunting it may seem."

Armed with newfound knowledge, the gods and goddesses ventured forth, eager to try out these powerful concurrency patterns. With the guidance of Rob Pike, they would become the most efficient gods and goddesses in all the heavens.
## Fanning Out and Fanning In Concurrency Patterns in Go

In Go programming, we can use the fanning out and fanning in concurrency patterns to divide a single task into multiple sub-tasks and then merge their results into a final outcome. This approach allows us to perform parallel processing and achieve speed and efficiency whilst preventing data race conditions and conflicts.

Here is an example Go code that demonstrates fanning out of tasks using goroutines and a channel:

```go
package main

import (
	"fmt"
	"sync"
)

func fanOut(input []int, size int, fanOutSize int) [][]int {
	output := make([][]int, size/fanOutSize)
	wg := sync.WaitGroup{}

	for i := 0; i < size; i += fanOutSize {
		wg.Add(1)
		go func(start int) {
			defer wg.Done()
			var end int
			if start+fanOutSize > size {
				end = size
			} else {
				end = start + fanOutSize
			}
			chunk := input[start:end]
			output[start/fanOutSize] = chunk
		}(i)
	}
	wg.Wait()
	return output
}

func main() {
	input := make([]int, 100)
	for i := 0; i < 100; i++ {
		input[i] = i
	}
	output := fanOut(input, len(input), 10)
	fmt.Println(output)
}
```

In the code above, we take a slice of integers and divide it into smaller chunks, which we then fan out to separate goroutines for processing. We create a `sync.WaitGroup` to wait for all the goroutines to finish. Once all the goroutines are completed, we merge their results into a single slice, which is the final outcome.

The `fanOut()` function takes the input slice, the total size of the slice, and the fan-out size - the size of the sub-slices. We create an output slice with the same number of sub-slices. We then loop over the input slice, starting at 0 and going up in `fanOutSize`. Inside the loop, we add a goroutine to the `WaitGroup` and calculate the start and end indices of the current chunk. We then create a new slice with the data from the current chunk and store it in the `output` slice at the appropriate index.

In the `main()` function, we create a slice of 100 integers and pass it to the `fanOut()` function, along with the length of the slice and a fan-out size of 10. The `fanOut()` function splits the input slice into 10 sub-slices of size 10 each and processes them in parallel using goroutines, then returns the results.

With this fanning out technique, we can process large amounts of data in parallel and achieve optimal performance in our code.


[Next Chapter](12_Chapter12.md)