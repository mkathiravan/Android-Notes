
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
