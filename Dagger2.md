#### 1. Add Dagger2 Dependencies

    dependencies{
      implementation 'com.google.dagger:dagger:2.x'
      kapt 'com.google.dagger:dagger-compiler:2.x'

      //For AssistedInject
      implementation 'com.squareup.inject:assisted-inject-annotations-dagger2: 0.6.0'
      kapt 'com.squareup.inject:assisted-inject-processor-dagger2:0.6.0'
    }

#### 2. Define Dependencies with @Module and @Provides

 --> A Dagger module is a class annotated with @Module. It contains methods annotated with @Provides, which tell Dagger how to create instances of the types the module provides.

      @Module
      class AppModule
      {
         @Provides
         @Singleton
         fun provideSomeDependency(): SomeDependency()
         {
           return SomeDependency()
         }
      }

#### 3. Create Component with @Component

--> A component is an interface annotated with @Component. It connects the modules and the injection targets.

        @Singleton
        @Component(modules = [AppModule::class])
        interface AppComponent
        {
          fun inject(activity: MyActivity)
        }
        
#### 4. Inject Dependencies with @Inject

--> Use the @Inject annotation to request dependencies in a class.

        class MyActivity: AppCompatActivity()
        {
            @Inject
            lateinit var someDependency: SomeDependency

            override fun onCreate(savedInstanceState: Bundle?)
            {
              super.onCreate(savedInstanceState)
              setContentView(R.layout.activity_main)

              (application as MyApplication).appComponent.inject(this)

              // Now dependency available to use

              someDependency.doSomething()
            }  
        }

#### 5. Initialize Dagger in Application Class

--> Initialize your Dagger component in your Application class.

        class MyApplication : Application()
        {
           lateinit var appComponent: AppComponent

           override fun onCreate()
           {
             super.onCreate()
             appComponent = DaggerAppComponent.builder().appModule(AppModule()).build()
           }
        }
        

#### 6. Use Scopes with @Scope

--> Define a custom scope for the lifespan of dependencies.

        @Scope
        @Rentention(AnnotationRetention.RUNTIME)
        annotation class ActivityScope

   Apply the scope to your module and component.

        @Module
        class ActivityModule
        {
          @Provides
          @ActivityScope
          fun provideActivityDependency(): ActivityDependency{
            return ActivityDependency()
          }
        }

        @ActivityScope
        @Component(dependencies = [AppComponent::class], modules = [ActivityModule::class])
        interface ActivityComponent{
          fun inject(activity: MyActivity)
        }

#### 7. Use SubComponents with @Subcomponent

---> Define a subcomponent that inherits from a parent component.

          @ActivityScope
          @SubComponent(modules = [ActivityModule::class]
          interface ActivitySubcomponent{
            fun inject(activity: MyActivity)
          }

    Define the subcomponent in the parent component.

         @Singleton
         @Component(modules = [AppModule::class]
         interface AppComponent
         {
           fun activitySubcomponent(activityModule: ActivityModule):ActivitySubcomponent
         }

    Use the subcomponent in the activity

            class MyActivity: AppCompatActivity()
            {
              @Inject lateinit var activityDependency: ActivityDependency
      
              override fun onCreate(savedInstanceState: Bunlde?)
              {
                super.onCreate(savedInstanceState)
                setContentView(R.layout.activity_main)
      
                val activitySubcomponent = (application as MyApplication).appComponent.activitySubcomponent(ActivityModule())
      
                activitySubcomponent.inject(this)
      
                // Now activityDependency is available to use
      
                activityDependency.doSomething()
              }
            }

#### 8. Assisted Injection

--> For runtime values, use **AssistedInject**.

1. Define ViewModel with AssistedInject

        class MyViewModel @AssistedInject constructor(@Assisted private val runtimeValue: String)
         {
            fun getRuntimeValue(): String{
                return runtimeValue
               }
            
            @AssistedFactory
            interface Factory{
               fun create(runtimeValue: String): MyViewModel
           }       
         }

 2. Add Factory to the Component

        @Singleton
        @Component(modules = [AppModule::class])
        interface AppComponent
        {
            fun inject(activity: MyActivity)
            fun myViewModelFactory(): MyViewModel.Factory
        }  

3. Inject Factory in the Activity

       class MyActivity: AppCompatActivity()
       {
            @Inject lateinit var myViewModelFactory: MyViewModel.Factory

            override fun onCreate(savedInstanceState: Bundle?)
               {
                   super.onCreate(savedInstanceState)
                   setContentView(R.layout.activity_main)

                   (application as MyApplication).appComponent.inject(this)

                   val runtimeValue = "Some runtime value"
                   val myViewModel = myViewModelFactory.create(runtimeValue)

                   // Now myViewModel is available to use
                   Log.d(TAG,myViewModel.getRuntimeValue())
               }
       }


### Putting it all Together

**AppModule.kt**

        @Module
        class AppModule{
            @Provides
            @Singleton
            fun provideSomeDependency(): SomeDependency()
            {
                return SomeDependency()
            }
        }

**AppComponent.kt**

    @Singleton
    @Component(modules = [AppModule::class]
    interface AppComponent{
        fun inject(activity: MyActivity)
        fun myviewModelFactory(): MyViewModel.Factory
        fun activitySubcomponent(activityModule: ActivityModule): ActivitySubcomponent
    }

**RuntimeValueModule.kt**

     @Module
     class RuntimeValueModule(private val runtimeValue: String)
     {
         @Provides
         @RuntimeValue
         fun provideRuntimeValue(): String
         {
             return runtimeValue
         }
     }

**MyViewModel.kt**

    class MyViewModel @AssistedInject constructor(@Assisted private val runtimeValue: String)
    {
        fun getRuntimeValue(): String
        {
            return runtimeValue
        }

        @AssistedFactory
        interface Factory{
            fun create(runtimeValue: String): MyViewModel
        }
    }

**MyApplication.kt**

        class MyApplication : Application()
        {
            lateinit var appComponent: AppComponent

            override fun onCreate()
            {
                super.onCreate()
                appComponent = DaggerAppComponent.builder().appModule(AppModule()).build()
            }
        }

**MyActivity.kt**

      class MyActivity : AppCompatActivity()
      {
          @Inject
          @RuntimeValue
          lateinit var runtimeValue: String
          
          @Inject
          lateinit var myViewModelFactory: MyViewModel.Factory

          @Inject
          lateinit var activityDependency: ActivityDependency

          override fun onCreate(savedInstanceState: Bundle?)
          {
              super.onCreate(savedInstanceState)
              setContentView(R.layout.activity_main)

              val value = "Some runtime value"

              val module = RuntimeValueModule(value)

              val appComponent = DaggerAppComponent.builder().runtimeValueModule(module).build()

              appComponent.inject(this)

              val activitySubcomponent = appComponent.activitySubcomponent(ActivityModule())

              activitySubcomponent.inject(this)

              val myViewModel = myViewModelFactory.create(value)

              //Now runtimeValue and myViewModel are available to use

              Log.d(TAG, runtimeValue)
              Log.d(TAG, myViewModel.getRuntimeValue())
          }
      }
