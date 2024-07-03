## Jetpack Compose
#### key Features
    i) Declarative UI: Instead of describing how to change the UI, you describe what the UI should look like for a give state.
    ii) Kotlin-Based
    iii) Composable funcitons
    iv) State Management: Compose uses state to determine how UIs should render. When the state changes, the UI automatically updates to reflect those changes.
    V) Integration with Jetpack
    Vi) Unidirectional Data Flow: Data flows in one direction, from parent to child making the flow of data easier to follow and debug.

### Key Concepts of State Management in Jetpack Compose

State Hoisting: It is a pattern where you move the state to a composable’s caller. This makes the state management more flexible and reusable. It involves

  i) Defining the state is a higher-level composable.

 ii) Passing the state and its update functions down to child composable.

—> State and MutableState are used to create observable state objects.

—> **remember** is used to retain state across recompositions.(It should be used for local state within a single composable)

—> **mutableStateOf** creates a mutable state.

—> **rememberSavable** it is similar to remember but retains state across activity or process recreation.(Screen rotations)

—> Add a preview to see the UI in Android studio without running the application.

—> **NavHostController** is responsible for managing the navigation state. It is created using rememberNavController().

—> **NavHost** sets up the navigation graph. It takes a NavController and start destination. Composable functions define the screens and their routes within the NavHost.

—> Navigating between the screens use navController.navigate(“route”) to navigate to a different screen, use navController.popBackStack() to navigate back to the previous screen.

### Navigation using jetpack compose

      Class MainActivity: ComponentActivity()
      {
         Override fun onCreate(savedInstanceState: Bundle?)
      {
           super.onCreate(savedInstanceState)
           setConten{
      		MyApp()
           }
      }
      
      }
      
      @Composable
      Fun myApp()
      {
         val navController = rememberNavController()
         MaterialTheme{
      	Surface{
      		NavGraph(navController = navController)
      	}
       }
      }
      
      @Composable
      Fun navGraph(navController: NavHostController)
      {
          NavHost(navController = navController, startDestination = “home”)
      {
         composable(“home”){
      	HomeScreen(navController = navController)
         }
         composable(“detail”){
      	DetailsScreen(navController = navController)
         }
      }
      
      }
      
      HomeScreen:
      
      @Composable
      Fun HomeScreen(navController: NavHostController)
      {
         Column(modifier = Modifier.padding(16.dp)){ 
      	Text (text = “Home Screen”) 
      	Button(onClick = 
      	{navController.navigate(“detail”)
      	}){
      		Text(“Go to Details”)
      	}
      }
      
      }
      
      DetailScreen: 
      
      @Composable
      Fun DetailScreen(navController: NavHostController)
      {
         Column(modifier = Modifier.padding(16.dp)){
      		Text(text = “Detail Screen”)
      		Button(onClick = 
      	{navController.popBackStack()}){
      		Text(“Back to Home”)
      	}
      	}
      }

### Recomposition of JetPack Compose

—> Recomposition is a fundamental concept of Jetpack Compose. It refers to the process where a composable function is re-invoked in response to state changes, updating the UI to reflect the new state. Unlike traditional Android views, where you manually update the UI components, Compose automatically handles the UI updates when the state changes, ensuring that the UI is always in sync with the data.

**Example**:

          @Composable
        	fun CounterScreen()
        {
        	val count = remember {mutableStateOf(0)}
        	Column(modifier = Modifier.padding(16.dp)){
        		Text(text = “Count: ${count.value}”)
        		Button(onClick = { count.value++})
        		{Text(“Increment”)}
        	}
        }

### User input handle using Jetpack compose

            @Composable
             	fun TextInputExample(){
            	val textState = remember {mutableStateOf(“”)}
            	Column(modifier = Modifier.padding(16.dp)){
            		OutlinedTextField(
            			value= textState.value, 
            		 onValueChange = {textState.value = it},
            		  label = {Text(“Enter text”)}
            		)
            		Text(text=“You entered: ${textState.value}”, modifier = Modifier.padding(top = 8.dp))
            	}
            }

--> The key Components of Jetpack Compose architecture include Composable functions, state management, the Modifier system, navigation with the Navigation Compose library, ViewModel, and integration with existing Android frameworks.

### Side Effects in Jetpack Compose

Side effects in Jetpack Compose refer to operations that have additional effects beyond modifying the UI state. Examples include making network requests, accessing databases, or updating shared preferences.

Key concepts of Side Effects: LaunchedEffect, rememberUpdatedState, DisposableEffect, SideEffect, produceState.

**LaunchedEffect**: It is used to launch a coroutine in the context of a composable. It is typically used for side effects that need to run in response to a change in key inputs. The coroutine will be cancelled and restarted if the key input will change.

**rememberUpdatedState**: It is used to create a state holder that updates its value whenever the input value changes. It ensures that the most recent value is used within a long-running side effect, such as coroutine.

**DisposableEffect**: It is used for side effects that need cleanup when the key input change or when the composable leaves the composition. It provides a way to perform clean-up actions by returning an onDispose lambda.

**Side Effect**: It is used to apply side effects that need to run after every successful recomposition. It is useful for synchronizing compose state with external non-composable state.

**produceState**: It is a composable that allows you to create a state based on some producer logic, typically involving suspending functions. It is useful for state that needs to be derived from asynchronous operations.

**derivedStateOf**: To create a state that is derived from other states and automatically recomposes when any of the dependencies change.

**snapshotFlow**: To convert compose state into a Flow, which can then be collected coroutine.

### What are all core UI components in Jetpack Compose?

        Text & Typography: Text, RichText, TextField
        Layouts: Column, Row, Box, ConstraintLayout.
        Buttons: Button, IconButton
        Images and Icons: Image, Icon
        List and Scrollable Components: LazyColumn, LazyRow, ScrollableColumn, ScrollableRow
        Material Design Components: TopAppBar, BottomAppBar, Card, SnackBar
        Dialog & Modals: AlertDialog, BottomSheet
        Navigation Component: Navigation
        Input Components: TextField, Checkbox, RadioButton

### What are layouts available in compose?

Column, Row, Box, ConstraintLayout, Scaffold, LazyColumn, LazyRow, ScrollableColumn, ScrollableRow

### Types of States in Compose

 a) **MutableState**: It represents a single value that can be observed and modified.  
              Ex: var count by mutableStateOf(0)
              
 B) **DerivedState**: Computes a value based on other state variables or data. It updates automatically when its dependencies change.
             Ex: val isPositive = derivedStatOf { count > 0}
             
C) **State Hoisting**: Passing state and state-modifying callbacks from parent composable to child composable.
                     
                     Ex: @Composable
            		fun ParentComponent(){
            			var count by remember {mutableStateOf(0)}
            			ChildComponent(count, onCountChanged = {newCount -> count = newCount})
            		}
                           @Composable
                            fun	ChildComponent(count: Int, onCountChanged: (Int) -> Unit){
            			Button(onClick = { onCountChanged(count+1})}{
            				Text(text = “Increment $count”)
            			}
            		}

—> **RememberCoroutineScope**: It is a function that provides a CoroutineScope that is tied to the lifecycle of the composable it is used in. This CoroutineScope can be launch coroutines that will be automatically cancelled when the composable leaves the composition. This helps in managing the lifecycle of coroutines and avoid memory leaks. 

—> Semantics provide a way to describe the meaning and role of UI elements to accessibility services, automated UI testing frameworks and other tools that interact with the UI. Using Modifier.semantics and related property you can provide meaningful descriptions and roles for your UI elements.

—> **Jetpack Compose phases**: Composition, Measurement and Layout, Drawing and Painting, Input Handling, State Management. Recomposition

 ### How to handle state in device orientation in Compose UI?
 
 Ans: ViewModels and State Management, rememberSaveable, Handle Configuration Changes manually(onSaveInstanceState and onRestoreInstanceState) 

  —> Use ViewModel when the state needs to survive configuration changes and potentially across different compostables. Use rememberSaveable for simpler state persistence within a single compostable or for UI-specific state.

—> To observe Flows in Jetpack compose you can use the collectAsState. This is the extension function allows you to convert a flow into a state that compose can observe and automatically recompose when the Flow emits a new value.

—> To observe LiveData states in Jetpack compose, you can use the observeAsState. This extension function allows you to observe changes in LiveData objects and update the UI accordingly,

—> Column -> VerticalList, Row —> HorizontalList

—> The **Box** layout is a versatile container that allows you to overlay and stack elements. It's often used for creating custom UI components or overlays.

—> The **Scaffold** layout is a higher-level layout that provides a structure for common UI elements like the app bar, floating action button, and more.

### How to passing the data between screens?
     1. Using Navigation Component

                        @Composable
                	fun NavGraph(startDestination: String = “screen1”)
                	{
                		val navController = rememberNavController()
                    		NavHost(navController = navController, startDestination = startDestination)
                		{
                			composable(“screen1”){
                					Screen1(navController)
                				}
                			composable(“screen2/{data}”, arguments = listOf(navArgument(“data”){ type = NavType.StringType}))
                				{
                					backStackEntry ->
                					val data = backStackEntry.arguments?.getString(“data”)
                					Screen2(data)
                				}
                		}
                	}
                
                
                    @Composable
                	fun Screen1(navController: NavController)
                	{
                		Button(onClick = {
                			navController.navigate(“screen2/HelloWorld”)})
                			{	
                				Text(“Go to Screen2”)
                			}
                		}
                	}
                
                  @Composable
                	fun Screen2(data: String?){
                		Text(text = “Received data: $data”)
                	}
                
                        2. Using ViewModel
                	
                	class sharedViewModel(): ViewModel(){
                		private val _data = MutableLiveData<String>()
                		val data: LiveData<String> = _data
                
                		fun setData(value: String){
                			_data.value = value
                		}
                	}
                
                @Composable
                	fun Screen1(viewModel: SharedViewModel = viewModel()){
                		Button(onClick = { viewModel.setData(“Hello from Screen1”)})
                		{
                			Text(“Go to Screen2”)
                		}
                	}
                
                 @Composable
                	 fun Screen2(viewModel: SharedViewModel = viewModel)
                	{
                		val data by viewModel.data.observeAsState()
                		Text(text = “Received Data : $data”)
                	}
