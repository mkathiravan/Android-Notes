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
