## Flow in Kotlin

---> In Kotlin, Flow is a part of the Kotlin Cororutines library and is designed to handle asynchronous data streams. It is similar to LiveData but more powerful and flexible. Flow is built on coroutines and can be used to represent a stream of values that are computed asynchornously.

#### Key Freatures of Flow:

1. **Cold Streams**: A flow doesn't start emitting values until it is collected. Each time you call collect on a Flow, a new sequence of values is emitted.

2. **Backpressure Handling**: Flow supports suspending functions and operators to ensure smooth handling of backpressure, where the producer and consumer of data might operate at different speeds.

3. **Cancellation**: A Flow respects coroutine cancellation, which means that is can be safely cancelled without causing resource leaks.



#### **i)Fetching Network Data:**

---> Traditionally network calls involved callback, leading to nested code and potential memory leaks. Flow offer a cleaner approach

                  fun fetchData(): Flow<Data>
                {
                   return flow{
                     Val response = RetrofitInstance.api.getData()
                
                   if(response.isSuccessful)
                {
                   emit(response.body()!!)
                }  else
                {
                    Throw Exception(“Network Error”)
                }   
                   }
                
                }

#### **ii) User input with Debounce:**

---> Immediate network requests on every keystroke can overload the server. Use Debounce to delay emissions until a set time has passed since the last character.

                fun searchUser(query: String): Flow<String>
                {
                   return flow{
                 	emit(query)
                   }.debounce(500)
                     .filter{it.isNotEmpty()}
                }  

#### iii) Combining Multiple Data Sources

---> You might need data from local storage and a network call to display a user profile. Use combine to merge emission form multiple flows.  

                fun getUserDetails(): Flow<UserProfile>
                {
                    val localFlow = flow {
                	emit(getUserFromLocal())
                  }
                  val networkFlow = fetchData().map{ it. toUserProfile()}
                  return combine(localFlow, networkFlow) {localUser, networkUser ->
                	networkUser ?: localUser  // Prioritise network data , fallback to local
                 }
                }

#### iv) Long-Running Operations with Progress Updates:

--->  Update the UI with progress during long-running tasks. Use flowOf and emit to periodically send progress updates.  

		fun downloadFile(): Flow<Int>
		{
		   return flow{
			for(I in 0..100){
				delay(100) 		emit(i)
			}
		  }
		}

#### v) Error-Handling and Cancellation:

 ---> Flows allow for proper error handling and cancellation using catch and onEach. Error Mgmt using catch and Cancellation use onEach.    
 
		 fun getUsers(): Flow<List<User>>
		{
		    return flow {
			val response = RetrofitInstance.api.getData()
		        if(response.isSuccessful){
				emit(response.body()!!)
			} else{
				throw Exception(“Failed to fetch users”)
			}
		 } .catch {emit (emptyList())}
		
		}

**vi) Event Handling with SharedFlow**

---> Centralized Events: Manage events like button clicks or network state changes in a single source.
	
	  private val _networkState = MutableSharedFlow<NetworkState>()
	  val networkState : SharedFlow<NetworkState> = _networkState.asSharedFlow()
	
	  fun onNetworkChange(state: NetworkState)
	  {
		_networkState.tryEmit(state)
	  } 


#### vii) Caching Data with Flow Caching Strategies:

  ---> Offline-First Approach: Improve user experience by displaying cached data while fetching fresh updates.    Flow Caching Power: Utilise operators like cache and conflateLatest for data caching. 

	   fun getPosts(): Flow<List<Post>>{
		return flow{
			val cachedPosts = getPostsFromLocal()
			if(cachedPosts.isNotEmpty()){
				emit(cachedPosts)
			}
			val networkPosts = fetchData().map{ it.toPostList()}
			emitAll(networkPosts)
		}
		.cache() //Cache the entire flow for subsequent collection
		.conflateLatest() // Only emit the latest network data if multiple emissions occur
	 }

#### viii) Testing Asynchronous Code with Flow Test Operators:

 --> Use operators like test and flowOf to create mock flows for unit testing

		   @RunWith(AndroidJUnit4::class)
		   Class MyViewModelTest
		  {
			@Test
			fun `fetchUser emits data successfully`()
		{
			val mockFlow = flowOf(listOf(User(“John Doe”)))
		        val viewModel = MyViewModel(FakeRepository(mockFlow))
			viewModel.users.test{
				val values = awaitValue()
				assertThat(values, contains(User(“John Doe”)))
		                finish()
			}
		}
			
		 }

#### ix) Streamlining Long-Running Operations with Flow Operators:
 
---> Manage multi-step operations with intermediate processing and error handling. Utilise operators like flatMapConcat, zip, and transform for complex flow manipulation.    

		fun downloadAndProcessFile(url: String) : Flow<String>
		{
		    return flow {
				val downloadedFile = downloadFile(url)
				val processedData = downloadedFile
							.flatMapConcat { bytes -> processBytes(bytes)} 
							.zipWith(getAdditionalData()) {data, additional -> combineData(data, additional)}
							.transform {it.toUpperCase()}
						emit(processedData)
			}.catch {emitException(it)}
		
		}


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


##### What are the differences between StateFlow and LiveData?

---> For LiveData you are not forced to give an initial value, it may end up writing more code in init{}; But for StateFlow you are Forced to give an initial value (including null), it may save your code a bit.

---> For LiveData even if you give an initial value, you still need to do Null Check when you access its value (see this), it's kind of annoying. But that's not gonna happen on StateFlow - it will be what it should be.

---> For LiveData you cannot easily, or elegantly observe data changes JUST inside ViewModel, you are gona use observeForever() which is also mentioned in here.

##### Encapsulate Mutable Flow

Example:   

	class ExampleViewModel
 	{
	  private val _mutableSharedFlow = MutableSharedFlow<Int>()
   	  val sharedFlow = _mutableSharedFlow.asSharedFlow()

      	  private val _mutableStateFlow = MutableStateFlow(0)
	  val stateFlow = _mutableStateFlow.asStateFlow()
  	}

##### Properly Manage Resources

Example: 

 	val scope = CoroutineScope(Dispatchers.Main)
  	val sharedFlow = MutableSharedFlow<Int> ()
   	val job = scope.launch{
    		sharedFlow.collect{value->
      		    println("Received: $value")	
		}
	}
	job.cancel()  // Later when the collector is no longer needed

#### Use buffer & repay configuration:

Example:

	val sharedFlow = MutableSharedFlow<Int> ( replay = 2 //replay the last 2 values
			extraBufferCapacity = 8 // Extra buffer capacity to avoid backpressure
 			)
	

#### Use Combine operators:

Example:

	val flow1 = MutableStateFlow(1)
 	val flow2 = MutableStateFlow(2)

        val combinedFlow = flow1.combine(flow2) {value1, value2 ->
		value1 + value2
 	}
  	launch{
   	  combinedFlow.collect{sum ->
		println("$sum")
      	  }
	}
