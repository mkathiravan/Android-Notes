
### Observable property Delegates 

---> Observable property delegation in Kotlin allows you to observe changes to a property and execute a custom lambda function whenever the property value is modified. This is achieved using the Delegates.observable() function from the Kotlin standard library.

    import kotlin.properties.Delegates
    class Person(name: String)
    {
        var name: String by Delegates.observable(name){
            property, oldValue, newValue ->
            println("Property ${property.name} changed from $oldValue to $newValue")
        }
    }
    fun main() {
        val person = Person("Kathiravan")
        person.name = "Dinesh"
    }

The output of the above program 

    Property name changed from Kathiravan to Dinesh


### Vetoable Delegate:

---> The vetoable lambda expression is executed before the property value changes and returns the boolean type which determines if the property value should change or not. If this function returns true the property value will change if it return false the value will not change.

---> It allows you to add custom validation to a property giving you the ability to accept or reject a new property value before it assigned.

      import kotlin.properties.Delegates
      class MyClass
      {
          var max: Int by Delegates.vetoable(0){
              property, oldValue, newValue -> newValue > oldValue
          }
      }
      fun main() {
          val myClass = MyClass()
          println(myClass.max)
          myClass.max = 10
          println(myClass.max)
          myClass.max = 5
          println(myClass.max)
      }

If you run the above program the output would be like as below

      0
      10
      10

### Invoke operator (Delegate Function)

---> In Kotlin, the invoke operator allows you to call an object as if it were a function. This means you can use parentheses () directly after an object reference to trigger a specific behavior defined within the object's invoke function.

##### Example 1:

        class Greeter {
            operator fun invoke(name: String) {
                println("Hello, $name!")
            }
        }
        
        fun main() {
            val greeter = Greeter()
            println(greeter("Kathiravan"))
        }

The output of the above program is

    Hello, Kathiravan!

**Benefits of the invoke operator:**

 i)Readability and Conciseness:
    It allows for a more natural and expressive syntax, making code easier to read and write.
    
 ii) DSL Creation:
    It is commonly used in creating Domain-Specific Languages (DSLs), providing a more fluent and intuitive way to interact with the DSL's       constructs.
    
iii)Function-like Objects:
    It allows objects to behave like functions, enabling functional programming paradigms.


##### Example 2: 

    fun operation(a: Int, b: Int, operation: (Int, Int) -> Int): Int
    {
        return operation(a,b)
    }
    fun sum(a: Int, b: Int): Int = a+b
    fun multiply(a: Int, b: Int): Int = a * b
    
    fun main() {
        val addFunction: (Int, Int) -> Int = ::sum
        val multiplyFunction: (Int, Int) -> Int = :: multiply
        
        println(operation(10,5, addFunction))
        println(operation(10,5, multiplyFunction))
    }
