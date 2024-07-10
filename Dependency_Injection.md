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
