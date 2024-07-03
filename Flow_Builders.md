### Flow Builders

#### Simple Flow

   --> Collect the simple flow from the flow builder

        runBlocking {
                    // Collect the simple flow from the flow builder
                    simpleFlow().collect{values->
                        Log.d(TAG,"Normal_Flow_Builders $values")
                    }
                }
        
           fun simpleFlow() = flow {
                for(i in 1..3){
                    emit(i)
                }
            }      

####  flowOf

 --> Collect the flow using flowOfArgs

            runBlocking{
                 flowOf(1,2,3,4,5,6,7,8,9,10).collect{value->
                        Log.d(TAG,"USing flowOf builder $value")
        
                    }
            }      

#### asFlow

---> Convert the item into flow
  
            runBlocking{
                listOf(1,2,3,4,5,6,7,8,9,10).asFlow().collect{value->
                      Log.d(TAG,"ASFlow_Collecting_values $value")
                  }
            }

#### channelFlow 

---> It is used to create flows from potentially concurrent operations. It provides a SendChannel that lets you emit values into the flow from different coroutines.
          
              runBlocking{
                   simpleChannelFlow().collect{value->
                          Log.d(TAG,"Channel_Flow $value")
          
                      }
              }
          
              fun simpleChannelFlow() = channelFlow {
                  launch { send(1) }
                  launch { send(2) }
                  launch { send(3) }
                  launch { send(4) }
              }

#### emptyFlow()
 
  ---> It is used when we want to create an empty flow.
          
               runBlocking{
                  emptyFlow<Int>().collect{value->
                          Log.d(TAG,"Empty_Flow_Collection$value")
          
                      }
               }

#### flowFromIterable()

           runBlocking{
              flowFromIterable(listOf(1,2,3,4,5,6,7,8)).collect{value->
                    Log.d(TAG,"Flow_Iteratable_list$value")
    
                }
           }
