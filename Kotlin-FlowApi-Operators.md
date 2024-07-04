## Flow API operaters

#### i) Map:

--> It is used to transform the emitted items from one data type to another. It applies a given transformation function to each emitted item and result downstream.

      runBlocking {
          val numbersFlow = flowOf(1,2,3,4,5)
          val squareFlow = numbersFlow.map {number->
              number * number
          }
      
          squareFlow.collect{square->
              println(square)
          }
      }

#### ii) Filter:

--> It is used to selectively emit items that satisfy a given condition. It allows you to filter out unwanted elements from the flow and only emit those that meet the specified criteria.

      runBlocking {
      val numbers = flowOf(1,2,3,4,5,6,7,8,9,10)
      val evenNumbers = numbers.filter{ number ->
          number%2 == 0
      }
      evenNumbers.collect{ evenNumber ->
          println("Even Number:$evenNumber")
      }
      }

### iii) flatMapConcat:

--> It is used to transform each emitted item into another flow and then concatenate these resulting flows into a single, sequential flow. It maintain the order of emission ensuring that items from each transformed flow are emitted in the order they are produced.

            runBlocking {
            var listOfItems = flowOf(listOf(1,2,3), listOf("A","B","C"), listOf(7,8,9))
            val sequentialFlow = listOfItems.flatMapConcat { list->
                flow {
                    list.forEach {item->
                        kotlinx.coroutines.delay(100)
                        emit(item)
            
                    }
            
                }
            
            }
            sequentialFlow.collect{element->
                println("Element $element")
            
            }
            }
      

### iv) transform:

--> It allows you to perform a custom transformation on each item. It gives you more control over the emissions. It allows you to emit multiple items, drop items and delay emission.

                  runBlocking {
                  val numbers = flowOf(1,2,3,4,5,6,7,8)
                  val transformedFlow = numbers.transform { number->
                      emit("Original : $number")
                      kotlinx.coroutines.delay(1000)
                      emit("Transformed: ${number*10}")
                  }
                  transformedFlow.collect{item->
                      Log.d(TAG,"Transformed_Flow $item")
                  
                  }
                  }

### v) collect:

--> It is used to consume the elements emitted by the flow. It is essential for obtaining the values and performing actions on each item in the flow.

            runBlocking {
            val words = flowOf("Hello","Kathir","Welcome","Kotlin")
            words.collect{word->
                Log.d(TAG,"Collecting_data $word")
            }
            }

### vi) onEach:

 --> It is used for performing side effects on each emitted item without modifying the item itself.
 
            runBlocking {
            val numbers = flowOf(1,2,3,4,5,6,7)
            numbers.onEach {number->
                Log.d(TAG,"On_Each_Element $number")
            }.collect{
                Log.d(TAG,"Collecting_On_Each_Element $it")
            }
            }

### vii) Reduce:

--> It is used to accumulate the values emitted by the flow and produce a single result. It combines the elements sequentially and applies a given operation to each pair of elements until only one value remains.

            runBlocking {
            val numbers = flowOf(1,2,3,4,5,6)
            val sum = numbers.reduce { accumulator, value ->
                accumulator+value
            }
            println("Sum of all numbers: $sum")
            }

### viii) distinctUntilChanged:

--> It is used to suppress consecutive duplicate emission in the flow. It ensures that only distinct values are emitted and any consecutive duplicates are skipped.

            runBlocking {
            val numbers = flowOf(1,2,2,3,4,4,5,6,4,5,3)
            val distinctNumbers = numbers.distinctUntilChanged()
            distinctNumbers.collect{number->
                println("Distinct Number: $number")
            }
            }

The above program will be print as 1,2,3,4,5,6,4,5,3

### ix) debounce:

--> It is filter out rapid consecutive emission and only emit the latest value after a specified time window. It is particularly useful when dealing with events that occur frequently and you want to handle only the most recent event within a given time frame.

             runBlocking {
            val searchQueryFlow = flowOf("Kotlin","Coroutines","flow","operators")
            val debouncedSearchQueryFlow = searchQueryFlow.debounce(300)
            debouncedSearchQueryFlow.collect{query->
                println("Processing search query: $query")
                delay(500)
                println("Search results for: $query")
            }
            }   

### x) zip:

--> It is used to combine multiple flows together in a pairwise manner. It waits for each flow to emit an item, the emits a new item that contains elements from each flow in the order they were emitted.


            runBlocking {
            val names = flowOf("Alice","Bob","Charlie")
            val ages = flowOf(25,30,35)
            val combinedFlow = names.zip(ages){name, age->
                "Name: $name, Age: $age"
            }
            combinedFlow.collect{pair->
                println(pair)
            }
            }

The output of the above program is

   Name: Alice, Age: 25
   Name: Bob, Age: 30
   Name: Charlie, Age: 35
               
