## Lateinit vs Lazy

#### Lateinit 

---> We may not want to initialize our values at declaraction time but instead want to initialize and use them at any later time in our application. However before using our value we must not forgot to initialize before it can be used. Without initializing if we want to access it will throw "**UninitializedPropertyAccessException**"

#### Lazy

---> It is design pattern we can create objects only the first time that we access them. Otherwise we don't have to initialize them.

<img width="657" alt="Screenshot 2024-07-05 at 1 45 39 PM" src="https://github.com/mkathiravan/Android-Notes/assets/39657409/91f68011-99b7-4d78-aff2-7a3605057f4b">


##### Example 1: using lateinit keywork

        class Myclass{
            lateinit var name: String
            
            fun initializeName()
            {
                name = "Kotlin"
            }
            fun printName(){
                if(::name.isInitialized)
                {
                    println(name)
                }
                else
                {
                    println("Name is not initialized")
                }
            }
        }
        fun main() {
            val myClass = Myclass()
            myClass.printName()
            myClass.initializeName()
            myClass.printName()
        }

The output of the above program would be like as below

      Name is not initialized
      Kotlin
        
##### Example 2: using lazy keyword

        class MyClass
        {
            val name: String by lazy{
                println("Computed!")
                "Kotlin"
            }
            
            fun printName(){
                println(name)
            }
        }
        fun main() {
            val myClass = MyClass()
            println("Before accessing name")
            myClass.printName()
            myClass.printName()
        }

If you run the above program the output would like as below

        Before accessing name
        Computed!
        Kotlin
        Kotlin        

#### Example 3: using lazy keyword

                class ExpensiveObject{
                    init{
                        println("Expensive object initialized")
                    }
                    fun someExpensiveFunction()
                    {
                        println("Some expensive operation")
                    }
                }
                class Example
                {
                    val lazyObject: ExpensiveObject by lazy{
                        ExpensiveObject()
                    }
                }
                fun main() {
                    val obj = Example()
                    println("Object is not created yet")
                    println(obj.lazyObject.someExpensiveFunction())
                }

The output of the above program 

        Object is not created yet
        Expensive object initialized
        Some expensive operation

##### Lazy Initialization using synchronized

---> The synchronized mode is the default for kotlin's lazy function. It ensures that the instance is created by only once, even if accessed by multiple threads simulataneously.

        val instance: MyClass by lazy{
                MyClass()
        }

#### Lazy initialization using Publication

---> The publication mode allows multiple threads to initialize the property but only one of them will succeed and assign the value.

        val instance: MyClass by lazy(LazyThreadSafetyMode.PUBLICATION)
        {
                MyClass()
        }

#### Lazy initialization without Synchronization

---> The none mode does not provide any thread safety. This is useful when you are sure that the property will not be accessed by multiple threads at the same time.
        
        val instance: MyClass by lazy(LazyThreadSafetyMode.NONE)
        {
                MyClass()
        }
