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

**i)SSL Pinning:**

                val certificatePinner = CertificatePinner.Builder().add("example.com","sha256").build()
                val client = OkHttpClient.Builder().certifiatePinner(certificatePinner).build()

**ii)Use Proguard or R8** 

**iii) Use Encrypted Storage**

 **iv) Avoid storing sensitive data**

 **v) Implement network security config(network.security.config.xml)**

 **vi) Use JWT Tokens**

**vii) Verify Host Name**

**viii) Use a secure communication library(OkHttp)**
