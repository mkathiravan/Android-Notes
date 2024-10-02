## UnderStanding Dagger

 ---> 4 Major components

    Module
    Component
    Provides
    Inject

**What is Dependency Injection, and why is it important in Android Development?**

--> Dependency Injection is a design pattern where objects receive their dependencies from an external source rather than creating them within the class. It improves code modularity, testability, and scalability by allowing dependencies to be swapped easily(for example, during testing).


**What is Dagger, and how does it implement Dependency Injection in Android?**

--> Dagger is a popular DI framework for Android, which generates code at compile-time to resolve dependencies. It provides dependency graphs and injects dependencies into Android components like Activities, Fragments, Services, etc.

  It uses annotations like @Inject, @Module, @Component, etc., to wire dependencies.


 **How does Hilt simplify Dependency Injection compared to Dagger?**

 ---> Hilt is a DI library built on top of Dagger to make it easier to implement DI in Android apps. It provides default bindings for common Android components like Activity, Fragment, ViewModel and Application, which reduces boilerplate code and simplifies the setup process compared to Dagger.


**What is a Componenet in Dagger, and What is its role in Dependency Injection?**

--> In Dagger, a Component is an interface that acts as a bridge between the @Module that provides dependencies and the classes where the dependencies are injected. It tells Dagger where to get the dependencies and where to inject them.


**What is the difference between @Singleton and @Scope annotations in Dagger/Hilt?**

--> @Singleton is a specific scope that ensures a single instance of a dependency is created and shared across the application. 

--> @Scope is a general term for custom scopes in DI which allows us to define different lifecycles for different components(e.g, @ActivityScope, @FragmentScope), controlling how long dependencies are retained.


**How does the @Inject annotation work in constructors, fields, and methods in Dagger?**

--> The @Inject annotation tells Dagger how to provide instances of a class. In constructors, it signals Dagger to use that constructor for dependency creation. In fields, Dagger will inject the necessary dependency automatically, and in methods, it's used to inject dependencies after an object has been created.


**What is the purpose of a @Binds in Dagger, and when you would use it?**

--> @Binds is used to bind an implementation of an interface to its interface type. It's preferred over @Provides when there's only one create implementation of an interface because it is more efficient as it avoids creating an extra method at runtime.


**Explain how Hilt handles dependency injection in Android's ViewModel?**

--> Hilt simplifies injecting dependencies into ViewModel by using the @HiltViewModel annotation. This allows Hilt to automatically inject the necessary dependencies into ViewModel. Additionally, you can use the SavedStateHandle in combination with Hilt to retain state during process death.


**What is a Subcomponent in Dagger, and how does it differ from a Component?**

--> A Subcomponent is a type of Dagger Component that is dependent on another parent Component. It can inherit dependencies from its parent and also provide its own scoped dependencies. Subcomponent is often used when you want to create different DI graphs for different parts of your application, such as a specific feature module.


**Can you explain how Dagger manages dependencies lifecycles using custom scopes?**

--> Dagger allows you to define custom scopes, which ensure that dependencies live as long as the scope they are tied to. For example, using an @ActivityScope, Dagger will ensure the same instance of the dependency is provided throughout the activity's lifecycle. If you use a different scope like @FragmentScope, dependencies tied to this scope will live as long as the fragment does.


**What is the purpose of @EntryPoint in Hilt, and when is it used?**

--> @EntryPoint is used to access Hilt dependencies in classes that Hilt cannot inject into directly, such as ContentProvider, BroadcastReceiver, and WorkManager. It allows Hilt to inject dependencies where regular injection is not possible


**How does Dagger/Hilt handle multibinding and how is it usefult?**

--> Dagger and Hilt support multibinding, which allows injecting collections of objects (e.g, Set, Map) into a class. It is useful when multiple implementations or instances of a dependency need to be injected and managed together.


**How does Hilt work with WorkManager and other Android components not directly supported by Hilt?**

--> Hilt can inject dependencies into classes like WorkManager using @EntryPoint, and with AssistedInject for cases where only part of the dependencies are provided by Hilt and the rest come from Android(like in Worker).


**How do @Inject constructors simplify dependency provision in Hilt compared to manually providing dependencies with @Provides?**

--> Hilt can automatically inject dependencies if a class has an @Inject constructor, which reduces the need to write explicit @Provides methods. This makes DI easier and eliminates much of the boilerplate code.


--> To understand it better in a basic way, think module as a provider of dependency and consider an activity or any other class as a consumer. Now to provide dependency from provider to consumer we have a bridge between them, in Dagger, Component work as that specific bridge.

---> Now, a module is a class and we annotate it with @Module for Dagger to understand it as Module.

---> A component is an interface, which is annotated with @Component and takes modules in it. (But now, this annotation is not required in Dagger-Hilt)

--> Provides are annotation which is used in Module class to provide dependency and,

--> Inject is an annotation that is used to define a dependency inside the consumer.

![image](https://github.com/mkathiravan/Android-Notes/assets/39657409/3fd76258-8ea2-42e3-a0fa-e198477f91ca)


### Qualifier

---> Consider an example where we have two functions returning String values. But while providing it via Dagger, how would dagger know which class needs which string values as they both are of the same type.

##### Example:

    @Provides
    @Singleton
    fun provideApiKey() = "MyApiKey"

    @Provide
    @Singleton
    fun providerLibraryKey() = "MyLibraryKey"

So, if we try to build the above function Dagger-Hilt would never build successfully as the dagger would consider the same type.

--> Error is: java.lang.String is bould multiple times.

To solve the problem quailifier annotation you use to identify a specific binding.

    @Qualifier
    @Retention(AnnotationRetention.BINARY)
    annotation class ApiKey

    @Qualifier
    @Retention(AnnoationRetention.BINARY)
    annoation class LibraryKey

 --> In the application module

    @ApiKey
    @Provides
    @Singleton
    fun provideApiKey(): String = "MyApiKey"

    @LibraryKey
    @Provides
    @Singleton
    fun provideLibraryKey(): String = "MyLibraryKey"

---> To inject them individually

     @ApiKey
     @Inject
     lateinit var apiKey: String

     @LibraryKey
     @Inject
     lateinit var libraryKey: String


### What is the use of Scope in Hilt dependency injection?

--> In Hilt, a dependency injection library for Android, Scopes are used to define the lifecycle and visibility of dependencies. They help manage how long an instance of a dependency should live and ensure that the same instance is provided within a defined scope. Here are common Hilt scopes:

  **i)@Singleton**: 

 --> This scopes ensures that a single instance of the dependency is created and shared throughout the entire application. The instance lives as long as the application is running.

Example:

  a) Create a class that you want to be singleton-scoped.

          class NetworkClient @Inject constructor()
          {
            fun doNetworkCall()
          }

  b) Create a Hilt Module to provide the NetworkClient dependency

         @Module
         @InstallIn(SingletonComponent::class)
         object NetworkModule
         {
           @Provides
           @Singleton
           fun provideNetworkClient(): NetworkClient{
              return NetworkClient()
           }
         }

  c) Use the Singleton-Scoped Dependency: Now, you can inject NetworkClient into any class that Hilt can inject into, such as as Activity or ViewModel

         @AndroidEntryPoint
         class MainActivity : AppCompatAcitivty()
         {
           @Inject
           lateinit var networkClient: NetworkClient

            override fun onCreate(savedInstanceState: Bundle?)
            {
              super.onCreate(savedInstanceState)
              setContentView(R.layout.activity_main)
              networkClient.doNetworkCall()
            }
         }

  d) Injecting into a ViewModle

        @HiltViewModel
        class MainViewModel @Inject constructor(private val networkClient: NetworkClient): ViewModel()
        {
          fun makeNetworkCall()
          {
            netowrkClient.doNetworkCall()
          }
        }

        
  **ii)@ActivityRetainedScope**:

  --> This scope is used for dependencies that need to live as long as an activity and its configuration changes(such as screen rotations). The instance is retained across configuration changes but is destroyed when the activity is finished.

  Example:

  a) Create User Repository class

          class UserRepository @Inject constructor()
          {
             private val users = mutableListOf<String>()
             fun addUser(user: String)
             {
               users.add(user)
             }
             fun getUsers(): List<String>
             {
               return users
             }
          }

  b) Create a module to provide the UserRepository dependency

           @Module
           @InstallIn(ActivityRetainedComponent::class)
           object UserRepositoryModule
           {
              @Provides
              @ActivityRetainedScoped
              fun provideUserRepository(): UserRepository
              {
                retrun UserRepository()
              }
           }

  c) You can inject UserRepository into any class that Hilt can inject into within the activity scope, such as ViewModel

           @HiltViewModel
           class UserViewModel @Inject constructor(private val userRepository: UserRepository): ViewModel()
           {
              fun addUser(user: String) {
                userRepository.addUser(user)
              }
              fun getUsers():List<String>
              {
                return userRepository.getUsers()
              }
           }

  d) Injecting into an Activity

           @AndroidEntryPoint
           class UserActivity: AppCompatActivity()
           {
             private val viewModel: UserViewModel by viewModels()
             override fun onCreate(savedInstanceState: Bundle?)
             {
              super.onCreate(savedInstanceState)
              setContentView(R.layout.activity_user)
              viewModel.addUser("Kathirava")
              val users = viewModel.getUsers()
              println("Users: $users")
             }
           }

  e) Initialize Hilt in your applicaton class

         @HiltAndroidApp
         class MyApplication : Application()
         
**iii)@ActivityScoped**: 

 --> This scope ensures that the same instance of a dependency is used within a single activity. A new instance is created for each new activity.

 a) Create a UserPreferences class

       class UserPreferences @Inject constructor()
       {
         private val preferences = mutableMapOf<String, String> ()
         fun setPreference(key: String, value: String)
         {
           preferences[key] = value
         }
         fun getPreference(key: String) : String?{
           return preferences[key]
         }
       }

  b) Create Hilt Module

       @Module
       @InstallIn(ActivityComponent::class)
       object UserPreferenesModule{
         @Provides
         @ActivityScoped
         fun provideUserPreferences(): UserPreferences
         {
           return UserPreferences()
         }
       }

  c) Injecting into an Activity

       @AndroidEntryPoint
       class SettingsActivity: AppCompatActivity()
       {
         @Inject
         lateinit var userPreferences: UserPreferences
         override fun onCreate(savedInstanceState: Bundle?)
         {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_settings)
           userPreferences.setPreference("theme","dark")

           val theme = userPreferences.getPreference("theme")
           println("Selected theme $theme")
         }
       }

   d) Injecting into a Fragment

       @AndroidEntryPoint
       class SettingFragment: Fragment(R.layout.fragment_settings)
       {
         @Inject
         lateinit var userPreferences: UserPreferences

         override fun onViewCreated(view: View, savedInstanceState: Bundle?){
             super.onViewCreated(view, savedInstanceState)

             userPreferences.setPreference("notifications","enabled")
             val notifications = userPreferences.getPreference("notifications")
             println("Notifications preference: $notifications")
         }
       }
**iv)@FragmentScoped**:

 --> This scope provides a single instance of a dependency for a specific fragment. A new instance is created for each new fragment instance.

 a) Create a FragmentLogger class

       class FragmentLogger @Inject constructor()
       {
         private val logs = mutableListOf<String>()
         fun log(message: String)
         {
           logs.add(message)
         }
         fun getLogs(): List<String>
         {
           return logs
         }
       }

  b) Create a Module to provide the FragmentLogger Dependency

        @Module
        @InstallIn(FragmentComponent::class)
        object FragmentLoggerModule
        {
          @Provides
          @FragmentScoped
          fun providesFragmentLogger(): FragmentLogger
          {
            return FragmentLogger()
          }
        }

  c) you can inject FragmentLogger into any class that Hilt can inject into within the fragment scope

       @AndroidEntryPoint
       class MainFragment: Fragment(R.layout.fragment_main)
       {
         @Inject
         lateinit var fragmentLogger: FragmentLogger
         override fun onViewCreated(view: View, savedInstanceState: Bundle?)
         {
           super.onViewCreated(view, savedInstanceState)
           fragmentLogger.log("fragment Created")

           //Observe or use the logs
           var logs = fragmentLogger.getLogs()
           println("Logs: $logs")
         }
       }

   ##### Example with Activity and Multiple Fragments

        @AndroidEntryPoint
        class MainActivity: AppCompatActivity()
        {
           override fun onCreate(savedInstanceState: Bundle?){
             super.onCreate(savedInstanceState)
             setContentView(R.layout.activity_main)
             if(savedInstanceState == null)
             {
               supportFragmentManager.beginTransaction().replace(R.id.fragment_container, FirstFragment()).commit()
             }
           }
        }

       @AndroidEntryPoint
       class FirstFragment : Fragment(R.layout.fragment_first)
       {
         @Inject
         lateinit var fragmentLogger: FragmentLogger
         override fun onViewCreated(view: View, savedInstanceState: Bundle?)
         {
            super.onViewCreated(view, savedInstanceState)
            fragmentLogger.log("First Fragment Created")

            //Navigate to Second Fragment
            view.findViewById<Button>(R.id.button_next).setOnClickListener{
               parentFragmentManager.beginTransaction().replace(R.id.fragment_container, SecondFragment()).addToBackStack(null).commit()
            }
         }
       }

       @AndroidEntryPoint
       class SecondFragment : Fragment(R.layout.fragment_first)
       {
         @Inject
         lateinit var fragmentLogger: FragmentLogger
         override fun onViewCreated(view: View, savedInstanceState: Bundle?)
         {
            super.onViewCreated(view, savedInstanceState)
            fragmentLogger.log("Second Fragment Created")

            // Observer or use the logs
            val logs = fragmentLogger().getLogs()
            println("Second Fragment Logs: $logs")
         }
       }

**v)@ViewScoped**:

 --> This scope provides a single instance of a dependency for a specific view.

 a) Create a UserManager class

         class UserManager @Inject constructor()
         {
           private val userName: String = "Guest"
           fun setUserName(name: String)
           {
             userName = name
           }
           fun getUserName(): String
           {
             return userName
           }
         }

  b) To provide the UserManager dependency

       @Module
       @InstallIn(ViewComponent::class)
       object UserManagerModule{

          @Provides
          @ViewScoped
          fun provideUserManager(): UserManager
          {
              return UserManager()
          }
       }

  c) you can inject UserManager into a custom view class

         @AndroidEntryPoint
         class UserProfileView @JvmOverloads constructor(
         context: Context, attrs: ArributeSet? = null, defStyleAttr: Int = 0): LinearLayout(context, attrs, defStyleAttr)
         {
            @Inject
            lateinit var userManager: UserManager

            init{
               inflate(context, R.layout.view_user_profile, this)
            }

           userManager.setUserName("Kathir")
           val userNameTextView: TextView = findViewById(R.id.user_name_text_view)
           userNameTextView.text = userManager.getUserName()
         }

         override fun onAttachedToWindow()
         {
           super.onAttachedToWindow()
           println("UserProfileView attached with user: ${userManager.getUserName()}")
         }

         override fun onDetachedFromWindow()
         {
           super.onDetachedFromWindow()
           println("UserProfileView detached")
         }
       

 **vi)@ViewModelScoped**:

 --> This scope is used to create dependencies that live as long as a ViewModel. It ensures the same instance is shared within the viewModel's lifecycle.

  a) Create a Repository class

         class Repository @Inject constructor()
         {
             private val data = mutableListOf<String>()
             fun addData(item: String)
             {
               data.add(item)
             }
             fun getData(): List<String>
             {
                return data
             }
         }

  b) you can inject Repository into a ViewModel

       @HiltViewModel
       class MainViewModel @Inject constructor(private val repository: Repository): ViewModel
       {
         fun addItem(item: String)
         {
            repository.addData(item)
         }
         fun getItems(): List<String>
         {
            return repository.getData()
         }
       }

  c) Use the ViewModel in an Activity or Fragment

        @AndroidEntryPoint
        class MainActivity : AppCompatActivity()
        {
           private val viewModel: MainViewModel by viewModels()

           override fun onCreate(savedInstanceState: Bundle?)
           {
             super.onCreate(savedInstanceState)
             setContentView(R.layot.activity_main)

             viewModel.addItem("Hello Hilt")

             val data = viewModel.getItems()
             println("Data :$data")
           }
        }
