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
