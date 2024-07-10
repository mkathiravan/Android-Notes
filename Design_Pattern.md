#### Creational Desgin Pattern

--> It is the process of object creation.

**i) Singleton Pattern:**

    --> Only one instance and provides a global point of access to that instance.

**ii)Factory Pattern:**

  ---> Defines an interface for creating objects but allows subclass to alter the type of object that will be created.

**iii)Abstract Factory Pattern**

  ---> Provides an interface for creating families of related or dependent object without specifying their concrete classes.

 **iv)Builder Pattern:**

 **v)Prototype Pattern:**

     --> Creating new objects by copying an existing object knows as the prototype rathen then creating new instances from scratch.


  #### What is Structural Design Pattern?

  ---> How classes and objects are composed to from larger structures.

   Adapter Pattern, Bride Pattern, Composite Pattern, Decorator Pattern, Facade Pattern, Flyweight Pattern, Proxy Pattern

 #### What is Behaviorual Pattern?

  ---->  How object interact and communicate each other.

   Chain of Responsibility Pattern, Command Pattern, Interpreter Pattern, Iterator Pattern, Mediator Pattern, Memento Pattern, Observer Pattern, State Pattern, Template Pattern, Visitor Pattern.


#### Create Observer Pattern in Kotlin

        interface Observer
        {
            fun update(data: Any)
        }
        interface Subject
        {
            fun registerObserver(observer: Observer)
            fun removeObserver(observer: Observer)
            fun notifyObserver(data: Any)
        }
        class WeatherStation: Subject
        {
          private val observers = mutableListOf(observer())
          private val temperature: Double = 0.0

          fun setTemperature(temperature: Double)
          {
            this.temperature = temperature
            notifyObserver(temperature)
          }

          override fun registerObserver(observer: Observer)
          {
            observers.add(observer)
          }

          override fun removeObserver(observer: Observer)
          {
            observers.remover(observer)
          }

          override fun notifyObserver(data: Any)
          {
            observers.forEach { it.update(data) }
          }
        }

        class Display: Observer
        {
            override fun update(data: Any)
            {
              println("data")
            }
        }

        fun main()
        {
          val weatherStation = WeatherStation()
          val display1 = Display()
          val display2 = Display()
          weatherStation.registerObserver(display1)
          weatherStation.registerObserver(display2)
          weatherStation.setTemperature(25.0)
        }
