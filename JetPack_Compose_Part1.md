
#### Unnecessary Recompositions:

---> **Mutable Data in Composbale**: Avoid passing mutable data (like mutable lists) directly to composables. Compose treats mutations as changes, triggering unnecessary recompositions. Use immutable data structures(like List or Map) or leverage remember to store derived values based on the original value.

Bad Example:

        @Composable
        fun MyList(items: MutableList<String>){}

Good Example:

        @Composable
        fun MyList(items: List<String>)
        {
           val displayedItems = remember(items) {items.toList()}
        }

#### Expensive Calculations in Composable Body:  

 --> Composable functions are often recomposed during the rendering process. Move expensive calculations outside composables or use remember to cache them.

Bad Example:

      @Composable
      fun myText(text: String)
      {
        val formattedText = text.toUpperCase() // Expensive conversation
        Text(formattedText)
      }

 Good Example

     @Composable
     fun myText(text: String)
     {
       val formattedText = remember(text) {text.toUpperCase()}
       Text(formattedText)
     }


#### Excessive Recomposition:

- Unnecessary recompositions can degrade performance.

Bad Example

       @Composbale
       fun MyScreen()
       {
          val count = remember {mutableStateOf(0)} 
          Column{
             Button(onClick = {count.value++}{
               Text("Increment")      
             } 
             Text("Count: ${count.value}")
            // This entire column will recompose every time count changes
            HeavyComposbale()
          }
       }

       @Composbale
       fun HeavyComposbale(){
          // Some heavy composable that does not need to recompose frequently     
       }

Good Example

              @Composable
              fun MyScreen()
              {
                val count = remember {mutableStateOf(0)}
                Column{
                    Button(onClick = {count.value++}{
                        Text("Increment")
                    }
                TextCounter(count.value)
                HeavyComposable()
                }
              }

            @Composable
            fun TextCounter(count: Int){
              Text("Count": $count")      
            }

           @Composable
           fun HeavyComposable(){
            // Some heavy composable that does not need to recompose frequently 
           }


#### Heavy Operations on the Main Thread:

- Performing resource-intensive tasks on the main thread can cause UI jank.

Bad Example:

        @Composable
        fun LoadDataScreen()
        {
           val data = loadData()
           Text(data)
        }

        fun loadData(): String()
        {
          return "Data loaded"
        }

Good Example:

        @Composable
        fun LoadDataScreen()
        {
           val data by produceState<String>(initialValue = null){
               value = loadData()
           } 
           Text(data ?: "Loading...")         
        }

        suspend fun loadData(): String
        {
          delay(2000)
          return "Data loaded"       
        }

#### Unnecessary State Hoisting:

 - Hoisting state unnecessarily can lead to complex and hard-to-manage state.

Bad Example:

        @Composable
        fun ParentComposable()
        {
          val textState = remember {mutableStateOf("Initial")}
          ChildComposable(textState)
        }

        @Composable
        fun ChildComposable(textState: MutableState<String>){
           TextField(value = textState.value, onValueChange = {textState.value = it})
        }

Good Example:

        @Composable()
        {
          ChildComposable()
        }

        @Composable
        fun ChildComposable()
        {
           val textState = remember {mutableStateOf("Initial")}
           TextField(value = textState.value, onValueChange = {textState.value =  it}
        }
