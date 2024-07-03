#### Scenario 1

        runBlocking {
                    val scope = CoroutineScope(SupervisorJob())
                        scope.launch {
                            delay(1000)
                            println("First Job")
                            throw IllegalArgumentException()
                        }
                        val secondJob = scope.launch {
                            delay(2000)
                            println("Second Job")
                        }
                        secondJob.join()
                        println("Is secondJob cancelled: ${secondJob.isCancelled}")
                        println("Is firstJob is cancelled: ${scope.isActive}")
        
        }

If you run the above program it will print "First Job" and throws java.lang.IllegalArgumentException.


#### Scenario 2

                runBlocking{
                                supervisorScope {
                                    val firstJob = launch {
                                        delay(1_000)
                                        println("firstJob")
                                        throw IllegalArgumentException()
                                    }
                
                                    val secondJob = launch {
                                        delay(2_000)
                                        println("secondJob")
                                    }
                
                                    secondJob.join()
                                    println("Is secondJob cancelled: ${secondJob.isCancelled}")
                                    println("Is firstJob is cancelled: ${firstJob.isCancelled}")
                                }
                    
                  }

--> If you run the above program it will also print "First Job" and throws java.lang.IllegalArgumentException.


#### Scenario 3

              runBlocking{
                  val exceptionHandler = CoroutineExceptionHandler {_, exception ->
                    println("CoroutineExceptionHandler caught $exception")
                }
            
                    val scope = CoroutineScope(exceptionHandler)
            
                    val firstJob = scope.launch {
                        delay(1_000)
                        println("firstJob")
                        throw IllegalArgumentException()
                    }
            
            
                    firstJob.join()
                    println("Is firstJob cancelled: ${firstJob.isCancelled}")
            
            }
          }

If run the above program it will produce the following output

         firstJob
         CoroutineExceptionHandler caught java.lang.IllegalArgumentException
         Is firstJob cancelled: true


#### Scenario 4

                runBlocking{
                     val exceptionHandler = CoroutineExceptionHandler {_, exception ->
                          println("CoroutineExceptionHandler caught $exception")
                      }
                
                          val scope = CoroutineScope(exceptionHandler)
                
                          val firstJob = scope.launch {
                              delay(1_000)
                              println("firstJob")
                              throw IllegalArgumentException()
                          }
                
                      val secondJob = launch {
                              delay(2_000)
                              println("secondJob")
                          }
                
                          firstJob.join()
                          secondJob.join()
                
                          println("Is firstJob cancelled: ${firstJob.isCancelled}")
                          println("Is SecondJob cancelled: ${secondJob.isCancelled}")
                          println("Is Scope of the coroutine: ${scope.isActive}")
                
                          println("Is firstJob completed: ${firstJob.isCompleted}")
                          println("Is SecondJob completed: ${secondJob.isCompleted}")
                          println("Is Scope of the coroutine: ${scope.coroutineContext.isActive}")
                
                          println("Is firstJob active: ${firstJob.isActive}")
                          println("Is SecondJob active: ${secondJob.isActive}")
                          println("Is Scope of the coroutine: ${scope.coroutineContext.isActive}")
                  }
                }

If run the above program it will produce the following output

        Is firstJob cancelled: true
        Is SecondJob cancelled: false
        Is Scope of the coroutine: false
        Is firstJob completed: true
        Is SecondJob completed: true
        Is Scope of the coroutine: false
        Is firstJob active: false
        Is SecondJob active: false
        Is Scope of the coroutine: false
