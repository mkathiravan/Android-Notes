### StateFlow:

---> Stateflow takes an initial value through constructor and emits it immediately when someone starts collecting. Stateflow is identical to LiveData. LiveData automatically unregisters the consumer when the view goes to the STOPPED state. When collecting a StateFlow this is not handled automatically , you can use repeatOnLifeCyCle scope if you want to unregister the consumer on STOPPED state. If you want current state use stateflow(.value).

---> If we want to convert MutableStateFlow into StatFlow use **variable.asStateFlow()**

### SharedFlow:

---> StateFlow only emits last known value , whereas sharedflow can configure how many previous values to be emitted. If you want emitting and collecting repeated values , use sharedflow.

---> It does not have an initial value, and you can configure its replay cache to store a certain number of previously emitted values for new collectors.

Note on terminology: just as we use the term observer for LiveData and collector for cold flows, we use the term subscriber for SharedFlow.



<img width="712" alt="Screenshot 2024-07-05 at 9 22 51 PM" src="https://github.com/mkathiravan/Android-Notes/assets/39657409/4504bde2-4930-461f-921b-621083a6f368">

##### Example 1: Shared Flow

        val sharedFlow = MutableSharedFlow<Int>()
        launch
        {
          sharedFlow.collect{value ->
            println("Collector 1 received: $value)
        }
        launch
        {
          sharedFlow.collect{value ->
            println("Collector 2 received: $value)
        }
        launch{
          repeat(5){i ->
            sharedFlow.emit(it)
          }
        }

If you run the above program and the output like as below

           Collector1_Received 0
           Collector2_Received 0
           Collector2_Received 1
           Collector1_Received 1
           Collector2_Received 2
           Collector2_Received 3
           Collector1_Received 2
           Collector2_Received 4
           Collector1_Received 3
           Collector1_Received 4  

##### Example 2: State Flow

        val mutableStateFlow = MutableStateFlow(0)
        val stateFlow: StateFlow<Int> = mutableStateFlow

        GlobalScope.launch {
            stateFlow.collect{value ->
                Log.d("MainActivity","Collector1_Received $value")
            }
        }

        GlobalScope.launch {
            stateFlow.collect{value->
                Log.d("MainActivity","Collector2_Received $value")
            }
        }

        GlobalScope.launch {
            repeat(5){i->
                mutableStateFlow.value = i
            }
        }

If you run the above program and the output would like as below

      Collector1_Received 0
      Collector2_Received 0
      Collector2_Received 4
      Collector1_Received 4
