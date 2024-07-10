## UnderStanding Dagger

 ---> 4 Major components

    Module
    Component
    Provides
    Inject

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

 **vi)@ViewModelScoped**:

 --> This scope is used to create dependencies that live as long as a ViewModel. It ensures the same instance is shared within the viewModel's lifecycle.

