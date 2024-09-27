### ObserveAsState

---> ObserveAsState to observe LiveData in the composable. Automatically recompose the composable when liveData changes.

#### How to setup compose in existing App?

        override fun onCreate(savedInstanceState: Bundle?)
        {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)
           val composeView = findViewById<ComposeView>(R.id.compose_view)
           composeView.setContent{
               MyApp{
                   Greeting("Compose in XML")
               }
           }
        }


### How to secure your network call in Android?

        i)SSL Pinning:

                val certificatePinner = CertificatePinner.Builder().add("example.com","sha256").build()
                val client = OkHttpClient.Builder().certifiatePinner(certificatePinner).build()

        ii)Use Proguard or R8

        iii) Use Encrypted Storage

        iv) Avoid storing sensitive data

        v) Implement network security config(network.security.config.xml)

        vi) Use JWT Tokens

        vii) Verify Host Name

        viii) Use a secure communication library(OkHttp)


### What is the difference between getContext(), getApplicationContext(), getBaseContext()?

    --> View.getContext() -> It return the context of view where the view is currently running.

    ---> View.getApplicationContext() -> It return the context of the application.

    ---> ContextWrapper.getBaseContext() -> If we want to access the context from another context within the application we can use this.


### What is the difference between Fold & Reduce in Kotlin?

 ---> Fold when you need a guaranteed result even with empty collections thanks to initial value.

         Example:

                  val numbers = listOf(1,2,3,4,5)
                  val sumWithFold = numbers.fold(10) {sum, number -> sum + number}
                  println(sumWithFold)

                  //output: 25

 ---> Reduce prefer not to specify an initial value and sure the collection has atleast one element.

        Example:

                val numbers = listOf(1,2,3,4,5)
                val sumWithReduce = numbers.reduce{sum, number -> sum + number}
                println(sumWithReduce)

                //output: 15


### Difference between viewBinding and dataBinding?

**ViewBinding**: 

   **a)Type-Safe Binding**:

 --> ViewBinding generates a binding class for each XML layout file. This class contains direct references to all the views with an ID in the layout.

 ---> This eliminates the need for findViewById calls, reducing boilerplate code.

  **b)Simplicity:**

  ---> ViewBinding is simpler to set up and use compared to DataBinding.

  ---> There is no need to use any special syntax in the XML layout files.

  **c)Performance**:

  ---> ViewBinding is generally faster than DataBinding since it does not involve any overhead associated with data binding expression or lifecycle management.

  **d)No Binding Expressions**:

---> ViewBinding does not support binding expression in XML. It only provides direct view references.

   **e)No Automatic Updates**:

---> Changes in data are not automatically reflected in the UI. You need to manually update the UI components.

**Data Binding**  

**a)Binding Expressions** :

--> DataBinding allows the use of binding expression within XML layout files. This means you can bind UI components directly to data model properties.

 **b)Two-way Data Binding** :

--> DataBinding supports two-way data binding, meaning changes in the UI can update the data model and vice versa automatically.

  **c)LifyCycle Awareness**:

--> DataBinding is lifecycle-aware meaning it can observe LiveData and automatically update the UI when the data changes.

  **d)Require More Setup**:

--> DataBinding requires enabling the data binding feature in the build.gradle file and setting up binding expressions in XML layout files.

 **e)Additional Features**:

--> DataBinding supports various advanced features like custom binding adapters, which allows you to define how a property is set on view.

**When to Use Each**

 **i)ViewBinding**: When you need simplicity and better performance. When you do not need binding expressions or automatic updates from data changes.

 **ii)DataBinding**: When you need powerful features like binding expressions, two-way data binding, and lifecycle awareness. When you want to minimize boilerplate code for updating UI components bases on data model changes.


### Difference between LiveData and Flow?

 #### LiveData:

 1. **Lifecycle Awareness:**
   - **LiveData is Lifecycle-aware:** `LiveData` is specifically designed to be lifecycle-aware, meaning it understands the lifecycle of Android components (like Activities and Fragments) and only updates observers that are in an active lifecycle state, avoiding issues like memory leaks.


2. **Mutable and Immutable:**
   - **Mutable Data:** `LiveData` can hold mutable data and can be updated using the `setValue(T)` or `postValue(T)` methods. `setValue(T)` should be called on the main thread, whereas `postValue(T)` can be called from any thread.


3. **Android-Specific:**
   - **Android Components:** `LiveData` is part of the Android Architecture Components library and is specifically tailored for Android app development.


4. **Integration with Data Binding:**
   - **Data Binding Support:** `LiveData` integrates well with Android's data binding library, making it easy to bind UI components directly to LiveData objects.


5. **Threading:** 
   - **Main Thread Safety:** LiveData is designed to be used on the main thread and ensures that observer callbacks are called on the main thread.


 #### Flow:

1. **Kotlin Coroutines Integration:**
   - **Coroutines Support:** `Flow` is a part of Kotlin's coroutine library. It's designed to work seamlessly with Kotlin coroutines, allowing you to perform asynchronous and concurrent tasks easily.


2. **Back-Pressure Handling:**
   - **Back-Pressure Support:** `Flow` supports back-pressure, meaning it can handle cases where the producer emits data faster than the consumer can process. This is crucial for applications dealing with large streams of data.


3. **Immutable Data Streams:**
   - **Immutable Data:** `Flow` emits immutable data streams, ensuring that data emitted by a `Flow` is not mutable. This can help prevent unintended modifications of the data.


4. **Thread Agnostic:**
   - **Concurrency:** `Flow` is thread-agnostic and can be used on any thread, allowing developers more flexibility in managing concurrency.


5. **Transformation Operators:**
   - **Transformation Operators:** `Flow` provides powerful operators like `map`, `filter`, and others, allowing developers to transform and manipulate the data stream easily.


6. **Interoperability with LiveData:**
   - **Interoperability:** You can easily convert `Flow` objects to `LiveData` objects and vice versa using extension functions provided in the Kotlin coroutines library, allowing for interoperability between the two.


#### Which One to Choose?


- **Use LiveData When:**
  - You are working with Android components and need lifecycle-aware data observability.
  - You prefer a simple and easy-to-use API for observing changes in your UI components.
  - You are using Android's data binding library for UI updates.


- **Use Flow When:**
  - You are working with Kotlin coroutines and need to handle asynchronous tasks.
  - You need support for back-pressure handling or want to work with large streams of data.
  - You need powerful operators for transforming and manipulating the data stream.


--> In summary, if you are primarily working within the Android ecosystem and need simple, lifecycle-aware data observability, LiveData might be a better choice. However, if you are working with Kotlin coroutines, need advanced features like back-pressure handling, or want more control over your asynchronous data streams, Flow would be the preferred option. Additionally, for applications that require both, you can easily convert between LiveData and Flow to leverage the benefits of both constructs.


### Difference between Sealed class and Final Class:

  **Sealed Class**: Used to represent a restricted class hierarchy with a known set of subclasses. Useful for modeling finite state machines and ensuring exhaustive handling in when expressions. Subclassing is restricted to the same file.

  - 1. Restricts Subclassing: Only subclasses defined within the same file can inherit from a sealed class.
  - 2. Exhaustive when Expressions: When using a sealed class in a when expression, the compiler can ensure all the possible cases are handled without needing an else clause.
  - 3. Ideal for Representing State: Commonly used to represent different states or types of a value, such as a result of an operation(success, error, loading)

    **Final Class**: In kotlin all classes are final by default meaning they cannot be subclassed unless explicitly marked as open or abstract. A final class is simply a class that cannot be inherited from.  


#### Activity based questions:

Activity A, B & C

1. Activity A
2. Activity B — order of override call methods
3. Activity C
4. Activty Back



Activity A:

  onCreate(), onStart(), onResume()

Call Activity B from Activity A

Activity A -> OnPause()
Activity B —> OnCreate(), onStart(), onResume()
Activity A -> onStop()

Call Activity C from Activity B

Activity B -> OnPause()
Activity C —> OnCreate(), onStart(), onResume()
Activity B -> onStop()

If you press back button form Activity C

Activity C -> OnPause()

Activity B -> onStart(), OnResume()

Activity C -> onStop(), onDestory() 



### Example 1:

        flowOf(2,4,6,8).onEach{
           delay(1000)
        }.flowOn(Dispatchers.Default)

### Example 2: 

        (1…5).asFlow().onEach{
           delay(1000)
        }.flowOn(Dispatchers.Default)

### Example 3: Safe Cast(as?)      


        val obj: Any = “Hello”
        val str: String? = obj as? String

### Example 4: Smart Cast(is)

        val obj: Any = “Hello”
        if(obj is String)
        {
          println(obj.length)
        }

### Example : Sealed Class

        sealed class Result
        {
         data class Success(val data: String): Result()
        
         data class Error(val message: String): Result()
        
          object Loading: Result()
        }
        
        fun processResult(result: Result)
        {
          when(result)
           {
            is Result.Success -> {print(“Success”)}
            is Result.Error -> {print(“Error”)}
        }
        }

### Example:

        WorkManager.getInstance(context).beginWith(requestWO1).then(requestWO2).enqueue()

 ---> The above code snippet is used to schedule a chain of background work requests in an Android application using WorkManager.

### 3 ways to Load Initial Data in Android

**1.Launched Effect:**

   Disadvantage: Every time a configuration change occurs (such as screen rotation), recomposition happens, causing LaunchedEffect to restart. This leads to 
   LoadInitialData being called multiple time unncessarily.

                
        class MyViewModelL : ViewModel()
        {
          private val _state = MutableStateFlow("")
          val state = _state.asStateFlow()

          fun loadInitialData()
          {
             viewModelScope.launch{
                delay(1000)
                _state.update {"Initial data"}
             }     
          }
        }

        @Composable
        fun MyScreen()
        {
           val viewModel = ViewModel<MyViewModel>()
           val state by viewModel.state.collectAsStateWithLifecycle()

           LaunchedEffect(Unit)
           {
              viewModel.loadInitialData()
           }
          Text(text = state)
        }

 **2.ViewModel Init Block:**

        Disadvantage: Testing is problematic because we lack control over the LoadInitialData function being called during initilization. This makes it difficult to write reliable tests or trigger the function at the right moment.

         class MyViewModel : ViewModel()
         {
             private val _state = MutableStateFlow("")
             val state = _state.asStateFlow()

             init
             {
                loadInitialData()
             }

         private fun loadInitialData()
          {
             viewModelScope.launch{
                delay(1000)
                _state.update {"Initial data"}
             }     
          }
             
         }

       @Composable
        fun MyScreen()
        {
           val viewModel = ViewModel<MyViewModel>()
           val state by viewModel.state.collectAsStateWithLifecycle()
           Text(text = state)
        }
