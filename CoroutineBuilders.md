## Coroutine Builders

#### launch: 

--> The launch coroutine builder is used to start a new coroutine that does not return a result. It is used for fire-and-forget tasks or tasks that don't need to return a value.

         runBlocking {
            val job = launch {
                delay(20000L)
                println("Task from Launch")
            }
            println("Task from runBlocking")
            job.join()
            println("Job Completed")
        }

If you run the above program the output would be like as

       Task from runBlocking
       Task from Launch
       Job Completed

 #### async: 

 --> It is used to start a new coroutine that performs a potentially asynchronous operation and returns a Deferred object representing the result of that operation. async is often used in scenarios where one wants to perform multiple concurrent tasks and retrieve their results asynchronously.

            runBlocking {
                val deferred: Deferred<String> = async {
                    delay(20000L)
                    "Result from async"
                }
                println("Task from runBlocking")
                val result = deferred.await()
                println("Deffered result: $result")
            }

 If you run the above program the output would be like as

           Task from runBlocking
           Deffered result: Result from async

#### runBlocking:

--> The runBlocking coroutine builder, blocks the main thread until each of the coroutine's tasks completed, that the main thread is created. We normally use runBlocking in the main thread when we are running tests to ensure the test doesn't end. It is Not Recommended in the real time scenario.

            runBlocking {
                println("Start of runBlocking")
                launch {
                    delay(20000L)
                    println("Task from nested launch")
                }
                println("End of runBlocking")
            }

If you run the above program the output would be like as 

        Start of runBlocking
        End of runBlocking
        Task from nested launch

#### withContext:

--> The withContext coroutine builder in Kotlin switches a coroutine's execution context to a different dispatcher, which defines the thread or thread pool where the coroutine will run.

          runBlocking {
              println("Start of runBlocking")
              withContext(Dispatchers.IO){
                  println("Running in IO context")
                  delay(20000L)
              }
              println("End of runBlocking")
          }

If you run the above program the output would be like as

          Start of runBlocking
          Running in IO context
          End of runBlocking
