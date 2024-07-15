### Example 1: Basic suspendCoroutine Usage

--> Lets start with a basic example of using suspendCoroutine to handle an asynchronous operation.

        suspend fun delayedMessage(): String = suspendCoroutine{continuation ->
                  Thread{
                      // Simulate some delay
                      Thread.sleep(2000L)
                      // Resume the coroutine with a result

                    continuation.resumeWith(Result.success("Delayed Hello!")) 
                  }.start()
        }
        fun main() = runBlocking{
          val message = delayedMessage()
          println(message) //Prints "Delayed Hello!" after 2 seconds
        }

In the above example, delayedMessage is a suspending function that pauses execution using suspendCoroutine and resumes it after a delay.

### Example 2: Handling Errors

--> Continuations can also handle errors. Let's see how we can resume a coroutine with an exception.

       suspend fun failingOpeartion(): String = suspendCoroutine{continuation ->
                  Thread{
                      // Simulate some delay
                      Thread.sleep(1000L)
                      // Resume the coroutine with an exception

                    continuation.resumeWith(Result.failure(RuntimeException("Opearation falied"))) 
                  }.start()
        }
        fun main() = runBlocking{
          try{
              val result = failingOpeartion()
              println(result)
          }catch(e: Exception){
            println("Caught exception: ${e.message}") // Prints "Caught exception: Opeartion failed"
          }
        }

In the abovec example, failingOperation simulates an asynchronous operation that fails, and the coroutine is resumed with an exception.

### Example 3: Integration with Callback-based APIs

--> A common use case for continuations is integrating with existing callback-based APIs. Let's create a suspending function that wraps a callback-based API.

      fun fetchDataFromNetwork(callback: (String) -> Unit)
      {
          Thread{
              Thread.sleep(1500L)
              callback("Network response")
          }.start()
      }
      suspend fun fetchData(): String = suspendCoroutine { continuation ->
          fetchDataFromNetwork{ response ->
              continuation.resumeWith(Result.success(response))
             }
      }
      fun main() = runBlocking{
        val data = fetchData()
        println(data) // Prints "Network response" after 1.5 seconds
      }

  ### Example 4: Using Continuation Directly

  --> While uncommon, you might sometimes need to manage continuation directly. 

        fun<T> asyncOperationWithContinuation(continuation: Continuation<T>)
        {
          Thread{
            // Simulate some delay
            Thread.sleep(1000L)
            continuation.resumeWith(Result.success("Complete with continuation") as Result<T>)
          }.start()
        }

        suspend fun myContinuationFunction(): String = suspendCoroutine { continuation ->
              asyncOperationWithContinuation(continuation)
          }

        fun main() = runBlocking{
            val result = myContinuationFunction()
            println(result) // Prints "Completed with continuation" after 1 second
        }  

### Example 5: Custom Suspend Function

      suspend fun customDelay(timeMillis: Long) = suspendCoroutine<Unit>{ continuation ->
          Timer().schedule(object: TimerTask())
          {
            override fun run()
            {
              continuation.resumeWith(Result.success(Unit))
            }
          }, timeMillis)
      }
      fun main() = runBlocking{
          println("Waiting")
          customDelay(2000L)
          println("Done") //prints Done after 2 secons
      }
