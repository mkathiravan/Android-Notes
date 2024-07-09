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

### E-Tags(Entity Tags)

---> Entity tags (ETags) are HTTP response headers that can be used to improve the efficiency of web applications by reducing unnecessary data transfers and ensuring data consistency. ETags can be used for caching and conditional requests. 

---> In conditional requests, clients can include the ETag received in a previous response in subsequent requests using the header "If-None-Match". The API will calculate the ETag again and compare it to the value sent by the client. If the ETag values match, the API will respond with a 304 Not Modified status, which means the response is the same and no response will be sent back to the client. This saves bandwidth and reduces server load. The client can then update its cache, knowing that the previously cached response is still valid. 

---> ETag values are typically hashes of the content, the last modification timestamp, or a revision number. They are strings of ASCII characters placed between double quotes, like "675af34563dc-tr34". The ETag algorithm family is set to MD5up by default, but it can be configured differently.

<img width="703" alt="Screenshot 2024-07-09 at 2 19 34 PM" src="https://github.com/mkathiravan/Android-Notes/assets/39657409/15a293df-9fca-4d46-ad63-04b0c04f4368">
