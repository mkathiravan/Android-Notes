### Explain the concept of Coroutine Builders in Kotlin

 -->  Coroutine Builders are functions that are used to create and start coroutines. Example include launch, async and runBlocking. They provide a convenient way to initiate and control the execution of coroutines.

### What is structured concurrency in Kotlin Coroutines?

--> Structured concurrency is an approach to managing coroutines that ensures the proper execution and completion of all child coroutines within a specific scope. It helps in avoiding common pitfalls associated with coroutine management.

### How do you achieve parallelism using Kotlin Coroutines?

--> Parallelism in coroutines can be achieved using the async builder along with await to await the results. Multiple asynchronous tasks can be started concurrently, and their results can be combined when needed.

### Explain the concept of Coroutine Flow in Kotlin

--> Coroutine Flow is a type of cold asynchronous stream that can emit multiple values over time. It is used for handling asynchronous sequence of data, providing a reactive programming style within the coroutine context.

### What is the purpose of the CoroutineScope in Kotlin Coroutines?

--> 'CoroutineScope' is a way to define the scope of a coroutine. It provides a structured way to launch coroutines and ensures that they are properly cancelled when the scope is cancelled.

### Explain the concept of Coroutine cancellation and how it differs from thread interruption

--> Coroutine cancellation is a cooperative mechanism where a coroutine is given the opportunity to clean up resources before it is cancelled. It differs from thread interruption, which forcefully stops a thread without giving it a chance to perform cleanup.

### What is the purpose of the GlobalScope in Kotlin Coroutines?

--> 'GlobalScope' is a predefined coroutine scope that lasts for the entire application lifetime. While it can be convenient it's generally recommended to use custom coroutine scopes to ensure structured concurrency.

### How do you test code that involves Kotlin Coroutines?

--> Testing coroutine code can be done using libraries like 'kotlinx-coroutines-test', which provides utilites for testing suspending functions and coroutines in a controlled environment. You can use 'TestCoroutineDispatcher' to control the execution of coroutines.

### Explain the concept of coroutine channels in Kotlin?

--> Coroutine channels provide a way for coroutines to communicate with each other by sending and receiving elements. They offer a convenient way to implement producer-consumer patterns and facilitate the flow of data between coroutines.
