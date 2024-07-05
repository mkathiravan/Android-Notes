## SOLID Principle

#### Single Responsibility Principle (SRP)

  ---> A class should have only one reason to change.
  
  ---> That one class should have only have one responsibility.

##### Scenario

  ---> OnBindViewHolder is to map an list item to a view.

  ----> UserActivity responsible for UI and user interaction. UserRespository is responsible for fetching data.


 #### Open Closed Principle (OCP)

  ----> Software entities such as classes, functions, modules should be open for extension but closed for modification.


##### Example

        sealed class Product(val name: String, val price: Double, val id: String)
        {
          abstract fun displayDetails(): String
        }
        class ElectronicProduct(name: String, price: Double, id: String): Product(name, price, id)
        {
          @override fun displayDetails(): String
          {
            return "Brand: $name, Price: $price"
          }
        }
        class ClothingProduct(name: String, price: Double, id: String): Product(name, price, id)
        {
          @override fun displayDetails(): String
          {
            return "Size: $size, price: $price"
          }
        }
  
#### Liskov Substitution Principle (LSP)

----> Child classes should never break the parent class type definition. This means that a subclass should override the methods from a parent class that does not break the functionlity of the parent class.

            open class Vehicle{
                 open fun startEngine(){
             	println("Engine started")
                 }
            }
            
            class Car: Vehicle(){
               override fun startEngine(){
                   println("Car engine started")
               }
               fun openRoof(){
            	println("Roof opened")
             }
            }
            class Bike: Vehicle()
            {
               override fun startEngine(){
            	println("Bike engine started")
            }
             fun kickStand(){
            	println("Kickstand deployed")
            }
            }
            
            fun startVehicleEngine(vehicle: Vehicle){
            	vehicle.startEngine()
            }
            
            fun main()
            {
              val car = Car()
              val bike = Bike()
              startVehicleEngine(car)
              startVehicleEngine(bike)
            }
