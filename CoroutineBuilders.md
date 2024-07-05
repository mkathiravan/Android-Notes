## Coroutine Builders

#### launch: 

--> The launch coroutine builder is used to start a new coroutine that does not return a result. It is used for fire-and-forget tasks or tasks that don't need to return a value.

---> It launching a corroutine that does not return a result

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

---> For launching corroutine that returns a result

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

--> For blocking the current thread until the coroutine completes.

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

---> For switching the corroutine context.

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

### produce:

--> It is used to create a coroutine that produces a stream of elements. It's similar to a generator or iterator in other programming languages, allowing you to emit values from the coroutine and consume them using a receive() function.

---> For creating a corroutine that produces a stream of values.

                  runBlocking {
                      val channel: ReceiveChannel<Int> = produce {
                          for(x in 1..5)
                          {
                              send(x)
                              delay(20000L)
                          }
                      }
                      for(y in channel)
                      {
                          println("Received: $y")
                      }
                  }

If you run the above program the output would be like as

                  Received: 1
                  Received: 2
                  Received: 3
                  Received: 4
                  Received: 5

 ### actor:

 --> Actors are a combination of a Coroutine and a Channel. You can send information off to an actor. The actor operates within a Coroutine and reads from that channel to process work. Channels are the way for you to communicate safely between Coroutines.

---> For creating an actor coroutine to process message sequentially.

                  runBlocking {
                      val actor = actor<Int> {
                          for(msg in channel)
                          {
                              println("Actor_Received $msg")
                          }
                      }
                      actor.send(1)
                      actor.send(2)
                      delay(20000L)
                      actor.send(3)
                      actor.send(4)
                      actor.close()
                  }

If you run the above program the output would be like as

         Actor_Received 1
         Actor_Received 2
         Actor_Received 3
         Actor_Received 4

#### SupervisorScope

---> In Kotlin Coroutines, supervisorScope is a coroutine builder function that creates a new coroutine scope with a different error-handling strategy than coroutineScope.

---> For creating a corroutine scope with supervisorJob to manage child coroutines independently
                  
                  runBlocking {
                      supervisorScope {
                          val child1 = launch {
                              try {
                                  delay(20000L)
                                  println("Child1 completed")
                              }catch (e: CancellationException)
                              {
                                  println("Child1 was cancelled")
                              }
                          }
                  
                         val child2 = launch {
                             try {
                                 delay(20000L)
                                 println("Child2 completed")
                                 throw Exception("Child2 failed")
                             }catch (e: Exception)
                             {
                                 println("Child2 caught an exception: ${e.message}")
                             }
                  
                         }
                          child1.join()
                          child2.join()
                          Log.d(TAG,"ChildStatus_${child1.isActive} && ${child2.isActive}")
                      }
                  }

If you run the above program the output would be like as

         Child1 completed
         Child2 completed
         Child2 caught an exception: Child2 failed
