## Inline Functions in Kotlin

--> An inline function is a kind of function that is declared with the keyword "inline" just before the function declaration. Once a function is declared inline, the compiler does not allocate any memory for this function instead the compiler copies the code virtually at the calling place of runtime. 

--> In kotlin, the higher-order functions or lambda expressions all stored as an object so memory allocation for both function objects and classes and virtual class might introduce runtime overhead. In order to reduce the memory overload of such higher-order functions or lambda expressions. We can use the inline keyword which ultimately request the compiler do not allocate memory and simply copy the inlined code of that function at the calling site.

#### Example 1:

        inline fun higherfunc(str: String, mycall:(String) -> Unit)
        {
            mycall(str)
        }
        fun main() {
            println("Welcome to Kotlin")
            higherfunc("Welcome to Android",::print)
        }

---> If a function is marked as inline, then whenever the function is called the compiler will paste the whole body of the function there.

---> The inline function tells the compiler to copy parameters and functions to the call site.

---> Inline function instruct compiler to insert the commplete body of the function whereever that function get used in the code.

----> Inline function is less overhead and faster program execution.

#### Example 2:

        fun guide()
        {
            println("guide start")
            teach{
                println("teach")
            }
            println("guide end")
        }
        
        inline fun teach(abc:() -> Unit)
        {
            abc()
        }
        
        fun main() {
            guide()
        }        
