--> In Kotin, a sealed class is used to represent a restricted class hierarchy. It is a type of class that can have limited set of subclasses, which are known at compile-time.

---> Sealed classes are useful when you have a closed set of types and want to ensure that no other types can be added to this set.

Here are some key points about sealed classes in kotlin.

**Declaration**: A sealed class is declared using the sealed keyword.

**Subclasses**: The subclasses of a sealed class must be defined in the same file as the sealed class itself.

**Instance Creation**: You cannot create instances of a sealed classs directly, you can only create instances of its subclasses.

**Exhaustive when statements**: When you use a when expression to handle sealed class instances, the compiler can check for exhaustiveness, meaning it can ensure that all possible subclasses are handled.

Example: 

                sealed class Shape{
                    class Circle(val radius: Double): Shape()
                    class Square(val side: Double): Shape()
                    class Rectangle(val length: Double, val width: Double): Shape()
                }
                
                fun describeShape(shape: Shape): String
                {
                    return when (shape)
                    {
                        is Shape.Circle -> "A circle with radius ${shape.radius}"
                        is Shape.Square -> "A square with side ${shape.side}"
                        is Shape.Rectangle -> "A rectangle with length ${shape.length} and width ${shape.width}"
                    }
                }
                
                fun main() {
                    val circle = Shape.Circle(3.0)
                    val square = Shape.Square(4.0)
                    val rectangle = Shape.Rectangle(5.0,2.0)
                    println(describeShape(circle))
                    println(describeShape(square))
                    println(describeShape(rectangle))
                }


**Behavior**:

**Restricted Inheritance**: 

      By using a sealed class, you control the subclassing and prevent any additional types from being added, ensuring that all possible subclasses are known and handled appropriately.

**Safety and Exhaustiveness**:

      The compiler can provide exhaustiveness checking in when expression, which enhances safety and reduces the chance of runtime errors.

**Improved Readability and Maintainability**:

      Code involving sealed classes tends to be more readable and easier to maintain because the possible types are explicity and contained within a single file.


---> Sealed class we can access within the package only.
