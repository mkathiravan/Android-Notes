## Scope Functions

#### also:

---> It is used for additional configuration of the objects

      fun main() {
          val numbers = mutableListOf(1,2,3,4,5).also{
              println("Original list $it")
              it.add(7)
          }
          println("Modify the list $numbers")
      }

#### apply:

---> It is used for object configuration and initialisations.

      data class User(var name: String, var email: String)
      
      fun main() {
          val user = User("Jane", "jane@example.com").apply {
          name = "Kathiravan"
          email = "kathir@abc.com"
      }
      println(user)
      }

#### with:

---> It allows you to work within the object's context and returns the result of the last expression in the block.

      data class Person(val name: String, val age: Int)
      
      fun main() {
          val person = Person("John", 30)
          val info = with(person) {
          "Name: $name, Age: $age"
      }
      println(info)
      
      }

#### run:

---> It handle object configuration and computation result.

      fun main() {
          val result = "Hello, world!".run {
          this.toUpperCase()
          this.reversed()
      }
      println(result) // Prints "!dlrow ,olleH"
      }


#### let:

---> It will handle the nullable object.
      
      fun main() {
          val nullableString: String? = "Kotlin is awesome!"
      nullableString?.let { 
          println(it.length) // Prints the length of the non-null string
      }
      }
