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

### Difference between every and CoEvery in MockK?

 ---> **every** is used to mock regular(non-suspending) functions.

 ---> **coEvery** is used to mock suspending functions.

### mockk:

 --> Creates a mock object of the specified class.

        class ExampleClass{
          fun exampleMethod(): String{
            return "Real Implementation"
          }
        }
        fun main()
        {
           val mock = mockk<ExampleClass>()
           println(mock.exampleMethod()) // output: null (default behavior for mocks)
        }

### every and retruns:

--> Stubs a method call on the mock object.

      fun main()
      {
        val mock = mockk<ExampleClass>()
        every{mock.exampleMethod()} returns "Mocked Implementation"
        println(mock.exampleMethod()) // output: Mocked Implementation
      }

### verify

--> Verifies that a method was called with specific arguments.

      fun main()
      {
        val mock = mockk<ExampleClass>()
        every {mock.exampleMethod()} returns "Mocked Implementation"
        mock.exampleMethod
        verify {mock.exampleMethod()} // Verifies that exampleMethod was called
      }

### slot

--> Captures arguments passed to a mocked method

      class Service
      {
        fun saveData(data: String)
        {
            // Save Data
        }
      }
      fun main()
      {
        val mock = mockk<Service>()
        val slot = slot<String>()
        every {mock.saveData(capture(slot))} just Runs
        mock.saveData("Test Data")
        println(slot.captured) // Output: Test Data
      }

### coEvery and coVerify

---> Used for stubbing and verifying coroutine methods.

        class CoroutineService{
            suspend fun fetchData(): String
            {
                return "Real Data"
            }
        }
        fun main() = runBlocking{
          val mock = mockk<CoroutineService>()
          coEvery{mock.fetchData()} returns "Mocked Data"
          println(mock.fetchData())// Output: Mocked Data
          coVerify{mock.fetchData()}
        }

### mockkStatic and mockkObject

---> Mocks static methods and object instances.

      Example: mockkStatic
      
        object StaticUtil{
            fun staticMethod(): String{
                return "Real static method"
            }
        }
        fun main()
        {
            mockkStatic(StaticUtil::class)
            every {StaticUtil.staticMethod()} returns "Mocked static method"
            println(StaticUtil.staticMethod()) //Output: Mocked static method
        }

      Example: mockkObject

        object Singleton()
        {
          fun sayHello() = "Hello from Singleton"
        }
        fun main()
        {
          mockkObject(Singleton)
          every{Singleton.sayHello()} returns "Mocked Hello"
          println(Singleton.sayHello) //Output: Mocked Hello
        }

### spyK

--> Creates a partial mock(spy) that calls real methods by default but can be stubbed.

        class RealClass{
            fun realMethod(): String{
              return "Real Implementation"
            }
        }
        fun main()
        {
          val spy = spyk(RealClass())
          every {spy.realMethod()} returns "Mocked Implementation"
          println(spy.realMethod()) //Output: Mocked Implementation
        }

### clearMocks

--> Resets the state of mocks, spies and stubs

        fun main()
        {
          val mock = mockk<ExampleClass>()
          every {mock.exampleMethod()} returns "Mocked Implementation"
          println(mock.exampleMethod())//Output: Mocked Implemenation

          clearMocks(mock)
          println(mock.exampleMethod())//Output: null(default behavior for mocks)
        }

### mockkConstructor

--> Mocks the constructor of a class

        class MyClass
        {
          fun sayHello()= "Hello"
        }
        fun main()
        {
          mockkConstructor(MyClass::class)
          every {anyConstructed<MyClass>().sayHello()} returns "Mocked Hello"

          val instance = MyClass()
          println(instance.sayHello()) // Output: Mocked Hello
        }

 ### mockkRelaxed

--> Creates a mock that returns default values for functions with no specified behavior.

      fun main()
      {
        val mock = mockkRelaxed<ExampleClass>()
        println(mock.exampleMethod()) //Output: default value for String
      }

### excludeRecords

--> Excludes specific methods from verification

      class MyService{
        fun doSomething() {}
        fun doSomethingElse() {}
      }
      fun main()
      {
        val mock = mockk<MyService>()
        every {mock.doSomething()} returns Unit
        every {mock.doSomethingElse()} returns Unit
        mock.doSomething()
        mock.doSomethingElse()
        excludeRecords {mock.doSomething()}
        verify{mock.doSomethingElse()}
      }
