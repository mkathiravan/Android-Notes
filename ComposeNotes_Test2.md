### Difference between remember and rememberSaveable function?

The key difference between "remember" and "rememberSaveable" functions in Jetpack Compose lies in their behaviour regarding state persisitence across configuration changes, such as screen rotations. Here's a detailed breakdown:

"remember"

 - Purpose: Used to remember state within the scope of composable function.

 - Persistence: State is not retained across configuration changes.

 - Use Case: Suitable for simple state management where persisitence across configuration changes is not needed.

Example:

    @Composable 
    fun RememberExample(){
      var count by remember {mutableStateOf(0)}
      Column{
         Text("Count: $count")
         Button(onClick = {count++}){
            Text("Increment")
         }
      }
    }

 - In this example the "count" state will reset to "0" if the device orientation changes.

"rememberSaveable"

 - Purpose: Used to remember state and persist it across configuration changes and process death.

 - Persistence: State is saved and restored during configuration changes and process recreation.

 - Use Case: Ideal for state that needs to be preserved across configuration changes, like user inputs or UI state that should be retained.

  Example:

     @Composable
     fun RememberSaveableExample(){
       var count by rememberSaveable {mutableStateOf(0)}
       Column{
         Text("Count: $count")
         Button(onClick = {count++}){
            Text("Increment")
         }
       }
     }

  - In this example, the "count" state will retain its value even if the device orientation changes.

Summary:

 - "remember": Remember state within the composable but does not retain it across configuration changes.

 - "rememberSaveable": Remembers state and ensures it is saved and restored during configuration changes
