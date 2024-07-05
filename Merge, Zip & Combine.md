## Difference between Merge, Zip and combine in Kotlin Flow.

#### Merge:

--> It takes multiple flows and emits their value as soon as they are available, in no particular order.

##### Use Case:

---> When you want to collect values from multiple flows as they come without waiting for any particular flow to emit first.

##### Example:

        runBlocking {
            val flow1 = flowOf(1,2,3,4,5).onEach { delay(2000) }
            val flow2 = flowOf(6,7,8,9,10).onEach { delay(5000) }
            val mergedFlow = merge(flow1, flow2)
            mergedFlow.collect{value->
                println(value)

            }
        }

---> If you run the above program then output would be any order like as below
        
            1 
            2 
            6
            3
            4
            7
            5
            8
            9
            10

#### Zip:

---> It combines multiple flows into a single flow by pairing corresponding elements from each flow. It emits values as pairs untl one of the flow is exhaustd.

##### Use Case:

---> Use zip when you need to synchronize emissions from multiple flows, corresponding them into pairs.

          runBlocking {
              val flow1 = flowOf(1,2,3,4,5).onEach { delay(2000) }
              val flow2 = flowOf("A","B","C","D","E").onEach { delay(5000) }
              val mergedFlow = flow1.zip(flow2){a,b->
                  println("$a,$b")
              }
              mergedFlow.collect{value->
                  println(value)
              }
          }

If you run the above program then output would like as below

            1,A
            2,B
            3,C
            4,D
            5,E

#### Combine:

---> It takes multiple flows and combines their lastest values whenever any of the flows emits a new value. It emits value as soon as flow emits, using the latest value from each flow.

       runBlocking {
            val flow1 = flowOf(1,2,3,4,5).onEach { delay(2000) }
            val flow2 = flowOf("A","B","C","D","E").onEach { delay(5000) }
            val mergedFlow = flow1.combine(flow2){a,b->
                println("$a,$b")
            }
            mergedFlow.collect{value->
                println(value)
            }
        }

If you run the above program then output would like as below

        2,A
        3,A
        4,A
        4,B
        5,B
        5,C
        5,D
        5,E
