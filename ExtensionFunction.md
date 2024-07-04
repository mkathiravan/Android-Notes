## Extension Function

---> The ability to add more functionality to the existing classes without inheriting them. This is acheived through a feature known as extension. 

### Example 1
      data class Person(val firstName: String, val lastName: String)
      
      fun Person.fullName(): String
      {
          return "$firstName $lastName"
      }
      fun main()
      {
          val person = Person("Kathiravan","Dinesh")
          val fullName = person.fullName()
          print("To get full Name $fullName")
      }

### Example 2

        class Student
        {
           fun isPassed(mark: Int): Boolean
            {
                return mark > 40
            }
        }
        
        fun Student.isExcellent(mark: Int): Boolean
        {
            return mark > 90
        }
        
        fun main() {
            val student = Student()
            val passingStatus = student.isPassed(55)
            println("Student status is $passingStatus")
            
            val excellentStatus = student.isExcellent(95)
            println("Student excellent status $excellentStatus")
        }

### Example 3

            class Myclass{
                companion object{
                    fun create(): String{
                        return "Calling create method of companion object"
                    }
                }
            }
            
            fun Myclass.Companion.helloWorld()
            {
                println("executing extension of companion object")
            }
            
            fun main() {
               Myclass.helloWorld()
            }

### Example 4:

            fun MutableList<Int>.swap(index1: Int, index2: Int): MutableList<Int>
                {
                    val tmp = this[index1]
                    this[index1] = this[index2]
                    this[index2] = tmp
                    return this
                }
            fun main() {
               val list = mutableListOf(5,10,15,20,25)
               println("Before swapping the list:$list")
               val result = list.swap(0,2)
               println("After swapping the list: $result")
            }
