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


#### Interface Segregation Principle (ISP)

---> It states that no client should be forced to depend on methods it does not use. This means if an interface becomes too fat then it should be split into smaller interfaces so that client implementing the interface does not implement method of no use of it.

        interface CanSwim
        {
          fun swim()
        }
        interface CanFly
        {
          fun fly()
        }
        class Duck: CanSwim, CanFly
        {
          override fun swim()
          {
            println("Duck Swimming")
          }
        }
        class Penguin: CanSwim
        {
          override fun swim()
          {
            println("Penguin Swimming")
          }
        }

#### Dependency Inversion Principle (DIP)

---> High-level modules should not depend on low-level modules. Both should depend on abstraction.

---> Abstraction should not depend upon details. Details should depend upon abstraction.

        interface PaymentProcessor
        {
            fun processPayment(amt: Double): Boolean
        }
        class PaypalPaymentProcessor : PaymentProcessor
        {
            override fun processPayment(amt: Double): Boolean
            {
                return true
            }
        }
        class StripePaymentProcessor : PaymentProcessor
        {
            override fun processPayment(amt: Double): Boolean
            {
                return true
            }
        }
        class PaymentService(private val paymentProcessor: PaymentProcessor)
        {
            fun processPayment(amt: Double): Boolean
            {
                return paymentProcessor.processPayment(amt)
            }
        }
        
        fun main() {
            val paymentProcessor = PaypalPaymentProcessor()
            val paymentService = PaymentService(paymentProcessor)
            println(paymentService.processPayment(50.00))
        }

Output: true
