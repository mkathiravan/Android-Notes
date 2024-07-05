## Reified

--> **Reified** is a special type of keyword that helps kotlin developers to access the information related to a class at runtime. 

---> **Reified** can be only used with inline functions when reified keyword is used the compiler copies the functions bytecode to every section of the code where the function has been called. In this way the generic type T will be assigned to the type of the value it gets an argument.

---> To access the information about the type of a class we use a keyword called "Reified" in kotlin. In order to use reified type we need to use the inline function.

---> The "Reified" keyword in kotlin is used with inline functions to access the type information of a generic type at runtime. It allows us to check the type of a generic paramter which is not possible with normal generic function.

#### Example 1:

        inline fun <reified T> myExample(name: T)
        {
            println("Name of your website $name")
            println("Type of myclass ${T::class.java}")
        }
        fun main() {
            myExample<String>("www.google.com")
            myExample<Int>(100)
            myExample<Long>(10L)
        }

If you run the above program it will give the output as below

        Name of your website www.google.com
        Type of myclass class java.lang.String
        Name of your website 100
        Type of myclass class java.lang.Integer
        Name of your website 10
        Type of myclass class java.lang.Long
