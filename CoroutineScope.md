### Can we able to create custom scope to control both GlobalScope and ViewModelScope

---> Creating a custom scope that can control both GlobalScope and ViewModelScope isn't a typical use case in kotlin coroutines, as they are designed for different lifecycle management purposes. 

---> GlobalScope is for global coroutines that live for the entire lifetime of the application, while ViewModelScope is tied to the lifecycle of a ViewModel.

However, if you want to manage coroutines that need to be aware of both application-wide and ViewModel-specific scopes, you might consider creating a custom CoroutineScope that can be used across different context while still being controlled appropriately.

Here's a conceptual approach:

**1. Define a Custom CoroutineScope:**

   Create a custom scope that can be cancelled explicitly or based on certain conditions.

**Use a Supervisor Job**     

  Utilize a SupervisorJob to handle coroutine hierarches and failure more gracefully.

**Context Management**

  Combine contexts to provide the desired behavior.


        class CustomScope : CoroutineScope
        {
            private val job = SueprvisorJob()
            override val coroutineContext: CoroutineContext
              get() = Dispatchers.Main + job

              fun cancelScope(){
                  job.cancel()
              }
        }

        class ExampleViewModel : ViewModel()
        {
            private val customScope = CustomeScope()

            init{
              customScope.launch{
                // Your coroutine work here
              }
            }
            override fun onCleared(){
                super.onCleared()
                customScope.cancelScope()
            }
        }
