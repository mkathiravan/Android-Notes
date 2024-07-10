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
