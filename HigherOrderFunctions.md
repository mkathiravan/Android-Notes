## Higher-Order Function

--> A function which can accept a function as a parameter or can return a function is called Higher-Order function. 

--> They enable more abstract and reusable code by allowing you to pass behaviors or actions as arguments, leading to more flexible and concise programming patterns.

#### Passing lambda expression as a parameter to higher-order function

--> We can pass a lambda expression as a parameter to Higher-order function. There are 2 types

     i) lambda expression which return unit
     ii) lambda expression which return any of the value like Integer, String etc

#### Lambda expression which return Unit

        var lambda = {println("Welcome to kotlin")}
        fun higherfunc(lmbd:() -> Unit)
        {
            lmbd()
        }
        fun main() {
            higherfunc(lambda)
        }

#### Lambda expression which return Integer value

        val lambda = {a:Int, b: Int -> a+b}
        fun higherfunc(lmbd: (Int, Int) -> Int)
        {
            var result = lmbd(4,2)
        	println("The sum of two number is $result")
            
         }
        fun main() {
           higherfunc(lambda)
        }

## Passing function as a parameter to Higher-Order function

#### Passing function which returns Unit

        fun printMe(s: String): Unit
        {
            println(s)
        }
        
        fun higherfunc(str: String, myfunc: (String) -> Unit)
        {
            myfunc(str)
        }
        
        fun main() {
            higherfunc("Welcome to Kotlin",::printMe)
        }

#### Passing function which returns Integer value

        fun add(a:Int, b:Int): Int{
            val sum = a+b
            return sum
        }
        fun higherfunc(addfunc: (Int,Int) -> Int)
        {
            val result = addfunc(3,6)
            println("The sum of two numbers is: $result")
        }
        
        fun main() {
            higherfunc(::add)
        }

## Returning a function from Higher-Order function

#### A function returning another function

          fun mul(a: Int, b: Int): Int{
              return a * b
          }
          fun higherfunc(): ((Int,Int) -> Int)
          {
              return :: mul
          }
          fun main() {
              val multiply = higherfunc()
              val result = multiply(2,4)
              println("The multiplication of two number is: $result")
          }
