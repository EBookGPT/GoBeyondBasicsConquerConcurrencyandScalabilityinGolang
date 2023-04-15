# Chapter 12: The Epic of Pipelines and Cancellation Propagation

After the exhilarating journey through the Fanning Out and Fanning In Concurrency Patterns, it's time to embark on a new adventure in Go Beyond Basics: Conquer Concurrency and Scalability in Golang. In this chapter, we dive into the epic of Pipelines and Cancellation Propagation.

To help us navigate this chapter, we have a special guest, Rob Pike. As one of the creators of the Go programming language, Rob brings his extensive knowledge and experience to our storytelling.

Our story begins with our hero, Theseus, who is tasked with slaying the Hydra, a fearsome beast with multiple heads. However, instead of facing the Hydra head on, Theseus decides to enlist the help of his trusted companions, Aegeus and Ariadne.

Just like Theseus, we can utilize the power of teamwork in Go programming by implementing pipelines. Pipelines are a powerful concurrency pattern that allows us to break down complex tasks into multiple stages. Each stage can be executed concurrently and can communicate with the other stages through channels.

Using channels and goroutines, we can send data downstream and receive results upstream. This not only simplifies our code but also allows for efficient use of system resources.

But what happens when we need to cancel a pipeline? We don't want to waste time and resources processing data that will never be used. This is where Cancellation Propagation comes in. We can use Go's built-in context package to gracefully cancel a pipeline and clean up any resources that were allocated.

Through the tale of Theseus, Aegeus, and Ariadne, we will learn how to implement pipelines and use Cancellation Propagation in our Go programs. Rob Pike will guide us through the technical details and show us best practices for utilizing these powerful concurrency patterns.

Join us on this epic journey as we conquer concurrency and scalability with Go Beyond Basics.
# Chapter 12: The Epic of Pipelines and Cancellation Propagation

Once upon a time, in the land of Go, there lived a brave hero named Theseus. Theseus was known throughout the kingdom for his remarkable ability to conquer any challenge that came his way. He was constantly seeking new adventures, and his next quest would prove to be his most challenging yet.

The king had tasked Theseus with slaying the Hydra, a beast with multiple heads that had been terrorizing the kingdom for centuries. Theseus knew that he could not face the Hydra alone and decided to enlist the help of his trusted companions, Aegeus and Ariadne.

Theseus' plan was to use a powerful technique known as pipelines to conquer the Hydra. He knew that by breaking the task into multiple stages, he could utilize his entire team's strength and ultimately be victorious.

The first stage of the pipeline was for Aegeus to chop off the Hydra's first head. This would then pass the Hydra onto the next stage of the pipeline, where Ariadne would use her strength to cut off the next head. Finally, Theseus would take the Hydra down by chopping off the final head.

Theseus used Go programming to implement the pipeline. Each stage of the pipeline was executed concurrently using goroutines, which allowed the team to divide and conquer the Hydra's multiple heads efficiently. They used channels to communicate with one another and pass the Hydra from one stage to the next.

However, these brave warriors knew that they could not waste precious resources by processing data that would ultimately never be used. This is where the concept of Cancellation Propagation comes in. Through the use of Go's built-in context package, Theseus was able to gracefully cancel the pipeline and clean up any resources that were allocated.

As Rob Pike, their special guest and a creator of the Go programming language, watched Theseus and his companions' journey, he was amazed by how they utilized the power of pipelines and cancellation propagation.

"These techniques are essential for writing efficient and scalable code," Rob commented. "Just like Theseus and his companions, we can achieve great things in Go programming by utilizing concurrency patterns such as pipelines."

With Rob Pike's guidance, Theseus, Aegeus, and Ariadne were victorious in their quest to conquer the Hydra. By implementing pipelines and cancellation propagation in their Go programming, they showed that even the most complicated tasks could be broken down into manageable stages that could be processed efficiently and with minimal waste of resources.

And so, we too can learn from their heroic story and embark on our journey to conquer concurrency and scalability with Go Beyond Basics.
# Explanation of the Code

In our epic of Pipelines and Cancellation Propagation, we followed the journey of Theseus, Aegeus, and Ariadne as they conquered the Hydra using powerful Go programming techniques. We utilized pipelines and cancellation propagation to break down the complex task into multiple stages, process the data efficiently, and gracefully clean up resources when necessary.

Let's take a closer look at the code we used in this epic:

## Pipelines

To implement the pipeline model, we used goroutines and channels. First, we created a channel for each stage of the pipeline. The first stage sent data downstream through its channel, and each subsequent stage received data from the previous stage's channel, processed it, and sent it downstream through its own channel.

```
func firstStage(in <-chan Data, out chan<- Data) {
    // Process data and send it downstream
    out <- newData
}

func secondStage(in <-chan Data, out chan<- Data) {
    // Receive data from previous stage, process it, and send it downstream
    out <- newData
}

func main() {
    // Create channels for stages
    ch1 := make(chan Data)
    ch2 := make(chan Data)

    // Start goroutines for each stage
    go firstStage(nil, ch1)
    go secondStage(ch1, ch2)

    // Send data to the first stage and receive the final result from the last stage
    finalResult := <-ch2
}
```

We utilized the power of goroutines to execute each stage concurrently, leading to efficient processing of data. By using channels to communicate between stages, we ensured that the data always flowed in the correct direction and that our code was easy to maintain.

## Cancellation Propagation

We also used the Go context package to gracefully cancel the pipeline and clean up any resources that had been allocated. This ensured that we didn't waste precious resources processing data that would ultimately never be used.

```
func firstStage(ctx context.Context, in <-chan Data, out chan<- Data) {

    for {
        select {
        case <-ctx.Done():
            // Cleanup resources if context cancelled
            return
        case data := <-in:
            // Process the data
            out <- newData
        }
    }
}

func secondStage(ctx context.Context, in <-chan Data, out chan<- Data) {

    for {
        select {
        case <-ctx.Done():
            // Cleanup resources if context cancelled
            return
        case data := <-in:
            // Process the data
            out <- newData
        }
    }
}

func main() {
    // Create the root context
    ctx, cancelFunc := context.WithCancel(context.Background())

    // Create channels for stages
    ch1 := make(chan Data)
    ch2 := make(chan Data)

    // Start goroutines for each stage with a child context
    ctx1, cancelFunc1 := context.WithCancel(ctx)
    go firstStage(ctx1, nil, ch1)

    ctx2, cancelFunc2 := context.WithCancel(ctx1)
    go secondStage(ctx2, ch1, ch2)

    // Send data to the first stage and receive the final result from the last stage
    finalResult := <-ch2

    // Cancel the pipeline if necessary
    cancelFunc2()
    cancelFunc1()
    cancelFunc()
}
```

By creating a root context and then creating child contexts for each stage of the pipeline, we ensured that the cancellation of the pipeline propagated through each stage gracefully, allowing us to clean up any resources as needed.

Through the use of Go's powerful concurrency features, these techniques allowed Theseus, Aegeus, and Ariadne to efficiently conquer the Hydra, and we can use these same techniques to write efficient and scalable code in our own Go programs.


[Next Chapter](13_Chapter13.md)