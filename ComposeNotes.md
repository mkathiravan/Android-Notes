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
