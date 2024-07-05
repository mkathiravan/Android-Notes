
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


#### Tail Recursion:

---> It allows a function to call itself as its last action without building up a new stack frame for each call. This is to avoiding stack overflow errors for large input values.

        tailrec fun factorial(n: Int, result: Int = 1): Int
        {
            return if(n == 0){ result } else { factorial(n-1, result *n)}
        }
        fun main() {
           val number = 5
           println("Factorial of $number using tail recursion: ${factorial(number)}") 
        }

 If you run the above program the output is

        120

#### Composition Function

---> In kotlin you can create composite functions by combining two or more functions into single function. This is typically done using higher-order functions. Below are some examples demonstrating how to create and use composite functions in kotlin.

###### Example 1: Using Function Composition

        infix fun <P1, R1, R2> ((P1) -> R1).andThen(f: (R1) -> R2): (P1) -> R2
        {
            return { p1: P1 -> f(this(p1))}
        }
        
        fun multiplyBy2(x:Int): Int = x * 2
        fun add3(x: Int): Int = x + 3
        
        fun main() {
            val multiplyBy2AndAdd3 = ::multiplyBy2 andThen ::add3
            val result = multiplyBy2AndAdd3(5)
            println(result)
        }

The output of the above program is 13 .

**Explaination**      

1. infix keyword: This allows the function to be called using infix notation, meaning you can write a andThen b instead of a.andThen(b)

2. <P1, R1, R2>: P1 is the input type of the first function. R1 is the output type of the first function and the input type for the second function. R2 is the output type of the second function.

3. ((P1) -> R1) : This is receiver type of the extension function. It means this function extends any function that takes a parameter of type P1 and returns a value of type R1.

4. f:(R1) -> R2: This is the parameter of the andThen function. It is a function that takes a parameter of type R1(the output of the first function) and returns a value of type R2.

5. Return value: This extension function returns a new function. This new function takes a parameter of type P1, applies the receiver function(the first function) to it, and then applies the function f (the second function) to the result.

   { p1: P1 -> f(this(p1))}: This is a lambda expression representing the new function. this(p1) calls the receiver function with p1, and f(..) applies the second function to the result of the first function.   


##### Example 2: Using Higher-Order Functions

--> You can create a composite function by explicitly defining it within a higher-order function.

        fun <P1, R1, R2> compose(f: (P1) -> R1, g: (R1) -> R2): (P1) -> R2 {
            return { p1: P1 -> g(f(p1))}
        }
        fun multiplyBy2(x:Int): Int = x * 2
        fun add3(x: Int): Int = x + 3
        
        fun main() {
            val compositeFunction = compose(:: multiplyBy2, ::add3)
            val result = compositeFunction(5)
            println(result)
        }


##### Example 3: Function Composition with Lambda

--> You can also use lambdas directly for function composition.

        val multiplyBy2: (Int) -> Int = { it * 2}
        val add3: (Int) -> Int = { it + 3}
        
        val multiplyBy2AndAdd3 = { x: Int -> add3(multiplyBy2(x))}
        
        fun main() {
            val result = multiplyBy2AndAdd3(5)
            println(result)
        }


##### Example 4: Using the let Function for Composition

---> Kotlin's standard library provides the let function, which can be used for simple compositions.

        fun multiplyBy2(x: Int): Int = x * 2
        fun add3(x: Int): Int = x + 3
        
        fun main() {
            val result = 5.let(::multiplyBy2).let(::add3)
            println(result)
        }
