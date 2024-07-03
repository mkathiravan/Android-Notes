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
