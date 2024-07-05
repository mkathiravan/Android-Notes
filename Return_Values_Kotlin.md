## Return Mulitple values from Kotlin Functions

### 1. Using Data class:

--> Returning multiple values from a function.
        
        data class Person(var name: String, var age: Int, var gender: Char)
        
        fun fetchPersonDetails(): Person
        {
            val name = "Kathir"
            val age = 37
            val gender = 'M'
            return Person(name, age, gender)
        }
        
        fun main() {
            val(name, age, gender) = fetchPersonDetails()
            println("$name, $age, $gender")
        }

### 2. Using Pair:

  ---> It can return two values from the function.

  ##### Example 1:

        fun fetchPersonDetails(): Pair<String, Int>
        {
            val name = "Kathir"
            val age = 37
            return Pair(name, age)
        }
        
        fun main() {
            val(name, age) = fetchPersonDetails()
            println("$name, $age")
        }

  ##### Example 2:

          fun main() {
            val(x,y) = Pair(1,"Kotlin")
            println(x)
            println(y)
        }

 ##### Example 3:

       fun main() {
          val pair = Pair("Hello Kathiravan", 1988)
          println(pair.first)
          println(pair.second)
      }

##### Example 4: Pair using extension function

      fun main() {
          var obj1 = Pair(1,2)
          val list1: List<Any> = obj1.toList()
          println(list1)
          
          var obj2 = Pair("Hello","Kathir")
          val list2: List<Any> = obj2.toList()
          println(list2)
          
          val obj3 = Pair("Kotlin",2024)
          val list3: List<Any> = obj3.toList()
          println(list3)
      }

### 3. Using Triple:

---> It has generic triple types that can return values from the function.

      fun fetchPersonDetails(): Triple<String, Int, Char>
          {
              val name = "Kathir"
              val age = 37
              val gender = 'M'
              return Triple(name, age, gender)
          }
      fun main() {
          val(name, age, gender) = fetchPersonDetails()
          println("$name, $age, $gender")
      }

### 4. Using return an array

      fun fetchPersonDetails(): Array<Any>
          {
              val name = "Kathir"
              val age = 18
              val gender = 'M'
              return arrayOf(name, age, gender)
          }
      
      fun main() {
          val(name, age, gender) = fetchPersonDetails()
          println("$name, $age, $gender")
      }
