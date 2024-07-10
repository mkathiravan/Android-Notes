#### Creational Desgin Pattern

--> It is the process of object creation.

**i) Singleton Pattern:**

--> Only one instance and provides a global point of access to that instance. 2 points fo singelton desing pattern.

  **a)Early Instantiation:**

---> This approach ensures that the instance is created at the time of class loading, which is referred to as early instantiation.
       

Example:

         object Singleton
         {
            init{
              println("Singleton instance is created")
            }
            fun doSomething()
            {
             println("Singleton method is called")
            }
         }
         fun main()
         {
             // Accessing the Singleton instance
             Singleton.doSomething()
             Singleton.doSomething()
         }

   **b)Lazy Instantiation:**

---> Lazy instantiation means that the instance of the Singleton is created only when it is first accessed, rather than at the time of class loading.

Example:

        class Singleton private constructor()
        {
            init{
                println("Singleton instance is created")
            }
            companion object
            {
              @Volatile
              private var instance: Singleton? = null
              fun getInstance(): Singleton
              {
                  return instance ?: synchronized(this)
                  {
                    instance ?: Singleton().also {instance = it}
                  }
              }
            }
            fun doSomething()
            {
                println("Singleton method is called")
            }
        }

         fun main()
         {
            val singleton1 = Singleton.getInstance().doSomething()
            val singleton2 = Singleton.getInstance().doSomething()
            println("Are both instances the same? ${singleton1 == singleton2}")
         }

If you run the above program the output look like as below

            Singleton instance is created
            Singleton method is called
            Singleton method is called
            Are both instances the same? true
**ii)Factory Pattern:**

  ---> Defines an interface for creating objects but allows subclass to alter the type of object that will be created.

**iii)Abstract Factory Pattern**

  ---> Provides an interface for creating families of related or dependent object without specifying their concrete classes.

 **iv)Builder Pattern:**

  ---> It builds a complex object using simple objects and using a step by step approach. This type of design pattern comes under creational pattern and one of the best way to create an object.

    Example: Alert Dialog, Notification

 **v)Prototype Pattern:**

     --> Creating new objects by copying an existing object knows as the prototype rathen then creating new instances from scratch.


  #### What is Structural Design Pattern?

  ---> How classes and objects are composed to from larger structures.

   Adapter Pattern, Bride Pattern, Composite Pattern, Decorator Pattern, Facade Pattern, Flyweight Pattern, Proxy Pattern

 #### What is Behaviorual Pattern?

  ---->  How object interact and communicate each other.

   Chain of Responsibility Pattern, Command Pattern, Interpreter Pattern, Iterator Pattern, Mediator Pattern, Memento Pattern, Observer Pattern, State Pattern, Template Pattern, Visitor Pattern.


### Observer Design Pattern:

 --> It is useful when you are interested in the state of an object and want to get notified whenever there is any change.

  Example: BroadcastReceiver, RxJava, LiveData

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


#### Create Adapter Desing Pattern in Kotlin

      //MediaPlayer interface
      interface MediaPlayer
      {
        fun play(audioType: String, fileName: String)
      }
      //AdvancedMediaPlayer interface
      interface AdvancedMediaPlayer
      {
        fun playVlc(fileName: String)
        fun playMp4(fileName: String)
      }
      //VlcPlayer class implementing AdvancedMediaPlayer
      class VlcPlayer: AdvancedMediaPlayer
      {
        override fun playVlc(fileName: String)
        {
            println("Playing vlcFile : $fileName")
        }
        override fun playMp4(fileName: String)
        {
            println("Do Nothing")
        }
      }
      //Mp4Player class implementing AdvancedMediaPlayer
      class Mp4Player: AdvancedMediaPlayer
      {
        override fun playVlc(fileName: String)
        {
            println("Do nothing")
        }
        override fun playMp4(fileName: String)
        {
            println("Playing Mp4File: $fileName")
        }
      }
      // Adapter class implementing MediaPlayer
      class MediaAdapter(private val audioType: String): MediaPlayer
      {
        private val advancedMusicPlayer: AdvancedMediaPlayer = 
        
            when(audioType)
            {
                "vlc" -> VlcPlayer()
                "mp4" -> Mp4Player()
                else -> throw IllegalArgumentException("Invalid media type: $audioType")
            
           }
        override fun play(audioType: String, fileName: String)
        {
            when(audioType)
            {
               "vlc" ->  advancedMusicPlayer.playVlc(fileName)
               "mp4" ->  advancedMusicPlayer.playMp4(fileName)
               else -> println("Invalid Media")
            }
        }
      }
      
      //AudioPlayer class implementing MediaPlayer
      class AudioPlayer : MediaPlayer{
          private var mediaAdapter: MediaAdapter? = null
          
          override fun play(audioType: String, fileName: String)
          {
              when(audioType){
                  "mp3" -> println("Playing mp3 file: $fileName")
                  "vlc","mp4" -> {
                      mediaAdapter = MediaAdapter(audioType)
                      mediaAdapter!!.play(audioType, fileName)
                  }
                  else ->
                  println("Invalid media type: $audioType. Only mp3, vlc, and mp4 are supported")
              }
          }
      }
      fun main()
      {
        val audioPlayer = AudioPlayer()
        audioPlayer.play("mp3","song.mp3")

        audioPlayer.play("vlc","movie.vlc")
        audioPlayer.play("mp4","movie.mp4")
        audioPlayer.play("avi","remote.avi")
      }


---> If you run the above program and the output as

            Playing mp3 file: song.mp3
            Playing vlcFile : movie.vlc
            Playing Mp4File: movie.mp4
            Invalid media type: avi. Only mp3, vlc, and mp4 are supported

The above program explaination

**1.MediaPlayer Interface**: Defines the standard play method.

**2.AdvancedMediaPlayer Interface**: Defines more specific methods for playing VLC and MP4 Files.

3.**VlcPlayer and Mp4Player Classes**: Implement the AdvancedMediaPlayer interface for playing VLC and MP4 files respectively.

**4.MediaAdapter class:**: Implements the MediaPlayer interface and uses an instance of AdvancedMediaPlayer to play VLC and MP4 files.

**AudioPlayer class**: Implements the MediaPlayer interface and uses the MediaAdapter to play different types of media files.

**Main Function:**: Tests the functionality by creating an AudioPlayer instance and calling the play method with different media types.

    
### How to make Singleton Pattern Thread Safe?

 ---> Making the singleton pattern thread safe is essential to ensure that multiple threads do not create separate instances of the singleton class. Here's one way to implement a thread safe singleton pattern in kotlin using lazy initialization with double-checked locking.

        class Singleton private constructor()
        {
            companion object
            {
                @Volatile
                private var instance: Singleton? = null
                fun getInstance(): Singleton
                {
                    return instance ?: synchronized(this)
                    {
                        instance ?: Singleton().also {instance = it} 
                    }
                }
            }
        }

### What is Factory Design pattern and example?  

 ---> It is a creational pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created that will be created. Here's how you can implement the Factory Design Pattern in kotlin using SMS and Email notifications as examples

         interface Notification
           {
              fun notifyUser()
           }
           class SMSNotification: Notification
           {
            override fun notifyUser()
            {
              println("Sending an SMS notification")
            }
           }
           class EmailNotificaion: Notification
           {
             override fun notifyUser()
             {
               println("Sending an Email Notification")
             }
           }
           class NotificationFactory
           {
             fun createNotification(type: String): Notification?
             {
                return when(type){
                 "SMS" -> SMSNotification()
                 "Email" -> EmailNotificaion()
                  else -> null
                }
             }
           }
           fun main()
           {
             val factory = NotificationFactory()
             val smsNotification = factory.createNotification("SMS")
             smsNotification?.notifyUser()

             val emailNotification = factory.createNotification("Email")
             emailNotification?.notifyUser()

             val invalidNotification = factory.createNotification("Push")
             invalidNotification?.notifyUser() ?: println("Invalid notification type")
           }

If you run the above program and the output like as below

       Sending an SMS notification
       Sending an Email Notification
       Invalid notification type
           
### What is abstract factory design pattern and example?

--> The Abstract Factory pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. It involves multiple factory mehtods within a single factory interface, where each factory method is responsible for creating differnt types of objects.

            public interface Button
           	{
           		void render();
           	}
           
            	public interface TextField
           	{
           		void render();
           	}
           
                   public class AndroidButton implements Button
           	{
           		@Override
           		public void render(){
           			System.out.println(“Rendering Android Button”)
           		}
           	}
           
            	 public class AndroidTextField implements TextField
           	{
           		@Override
           		public void render(){
           			System.out.println(“Rendering Android TextField”)
           		}
           	}
           
           	public class IOSButton implements Button
           	{
           		@Override
           		public void render()
           		{
                  			System.out.println(“Rendering IOS Button”)
           		}
           	}
           
           	public class IOSTextField implements TextField
           	{
           		@Override
           		public void render(){
           			System.out.println(“Rendering IOS TextField”)
           		}
           	}
           
           	public interface GUIFactory
           	{
           		Button createButton();
           		TextField createTextField();
           	}
           
           
           	public class AndroidFactory implements GUIFactory
           	{
           		@Override
           		public Button createButton(){
           				return new AndroidButton();
           			}
           
                         @Override
           		public TextField createTextField(){
           			return new AndroidTextField();
           		}
           	}
           
           	public class IOSFactory implements CUIFactory
           	{
           		@Override
           		public Button createButton(){
           				return new IOSButton();
           			}
           
                         @Override
           		public TextField createTextField(){
           			return new IOSTextField();
           		}
           	}
           
            	public class Client
           	{
           		private Button button;
           		private TextField textField;
           		public Client(GUIFacotry factory)
           		{
           			button = factory.createButton()	
           			textfield = factory.createTextField()
           		}
           		public void renderUI
           		{
           			button.render()
           			textfield.render();
           		}
           
                  		public static void main(String[] args)
           		{
           			GUIFactory androidFactory = new AndroidFactory();
           			Client androidClient = new Client(androidFactory);
           			androidClient.renderUI();
           
           			GUIFactory iosFactory = new IOSFactory();
           			Client iosClient = new Client(iosFactory);
           			iosClient.renderUI();
           		}
           
           	}
