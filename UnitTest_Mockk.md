---> The MockK library is a popular mocking library for Kotlin. It allows you to create mock objects and define their behavior, which is useful for unit testing. MockK is specifically designed for Kotlin, providing a more idiomatic and feature-rich experience compared to other mocking libraries.

Here's a detailed overview of the working flow of MockK, along with examples:

**Basic Concept**:

  i) Mocking: Creating a mock object of a class.

  ii) Stubbing: Defining the behavior of a mocked method.

  iii) Verification: Checking that a method was called with specific arguments.

  #### Creating a Mock Object

       class ExampleTest{
           interface Service{
               fun fetchData(): String
           }
           @Test
           fun testMock()
           {
             // Creating a mock object
             val mockService = mockk<Service>()

             // Stubbing the method
             every{ mockSerive.fetchData()} returns "Mocked Data"

             // Calling the method
             val result = mockService.fetchData()

             // Verification
             verify { mockService.fetchData()}

             // Assertions
             assert(result == "Mocked Data")
           }
       }

  #### Mocking a Class with Constructor

        class Repository{
            fun getData(): String{
              return "Real Data"
        }
        }
        class ExampleClass(private val repository: Repository){
            fun getDataFromRepository(): String{
              return repository.getData()
            }
        }
        class ExampleTest
        {
          @Test
          fun testMockClassWithConstructor(){
             // Creating a mock object
             val mockRepository = mockk<Repository>()

             // Stubbing the method
             every {mockRepository.getData()} returns "Mocked Data"

             // Creating an instance of the class with the mock
             val exampleClass = ExampleClass(mockRepository)

             // Calling the method
             val result = exampleClass.getDataFromRepository()

            // Verification
            verify {mockRepository.getData()}

            // Assertions
            assert(result == "Mocked Data")
          }
        }

  #### Capturing Arguments

        class Service{
            fun saveData(data: String){
              //save data
            }
        }
        class ExampleTest{
          @Test
          fun testCaptureArguments()
          {
            // Creating a mock object
            val mockService = mockk<Service>()

            // Creating a slot to capture the arguments
            val slot = slot<String>

            // Stubbing the method
            every {mockService.saveData(capture(slot))} just Runs

            // Calling the method
            mockService.saveData("Capture Data")

            // Verification
            verify {mockService.saveData("Captured Data")}

            // Assertions
            assert(slot.captured == "Captured Data")
          }
        }

  #### Mocking Coroutines

      class CoroutineService{
        suspend fun fetchData(): String
        {
          return "Real Data"
        }
      }
      class CoroutineTest{
        @Test
        fun testMockCoroutine() = runBlocking{
              //Creating a mock object
            val mockService = mockk<CoroutineService>()

            // Stubbing the method
             coEvery {mockService.fetchData()} returns "Mocked Data"

            // Calling the method
            val result = mockService.fetchData()

            // Verification 
            coVerify {mockService.fetchData()}

            // Assertions
            assert(result == "Mocked Data")
        }
      }
