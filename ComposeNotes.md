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
