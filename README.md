# Android-Topics

   ### Dependency Inversion Principle: 

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
      Output: ****True****

   ### Private constructor using singleton 

         class Singleton private constructor()
        {
            companion object
            {
                private var instance: Singleton? = null
                fun getInstance(): Singleton
                {
                    return instance ?: synchronized(this)
                    {
                        instance ?: Singleton().also{ instance = it}
                    }
                }
            }
           
           fun dispaly()
            {
                
            }
        }
        
        fun main() {
            val singleton = Singleton.getInstance()
            singleton.dispaly()
        }


 ### Abstract Factory Pattern

        // Interfaces
        interface Button {
            fun render()
        }
        
        interface TextField {
            fun render()
        }
        
        // Concrete implementations
        class AndroidButton : Button {
            override fun render() {
                println("Rendering Android Button")
            }
        }
        
        class AndroidTextField : TextField {
            override fun render() {
                println("Rendering Android TextField")
            }
        }
        
        class IOSButton : Button {
            override fun render() {
                println("Rendering IOS Button")
            }
        }
        
        class IOSTextField : TextField {
            override fun render() {
                println("Rendering IOS TextField")
            }
        }
        
        // Abstract Factory
        interface GUIFactory {
            fun createButton(): Button
            fun createTextField(): TextField
        }
        
        class AndroidFactory : GUIFactory {
            override fun createButton(): Button {
                return AndroidButton()
            }
        
            override fun createTextField(): TextField {
                return AndroidTextField()
            }
        }
        
        class IOSFactory : GUIFactory {
            override fun createButton(): Button {
                return IOSButton()
            }
        
            override fun createTextField(): TextField {
                return IOSTextField()
            }
        }
        
        // Client
        class Client(factory: GUIFactory) {
            private val button: Button = factory.createButton()
            private val textField: TextField = factory.createTextField()
        
            fun renderUI() {
                button.render()
                textField.render()
            }
        }
        
        fun main() {
            val androidFactory = AndroidFactory()
            val androidClient = Client(androidFactory)
            androidClient.renderUI()
        
            val iosFactory = IOSFactory()
            val iosClient = Client(iosFactory)
            iosClient.renderUI()
        }

