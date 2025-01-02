### How to write UI test cases for Compose Screen?

--> Writing UI test cases for Jetpack Compose screens involves using the "compose-test" library to verify that your commposables render correctly and interact as expected. Here's a step-by-step guide to help you get started.

##### Step 1: Create a ComposeTestRule

 Create a "ComposeTestRule" in your test class to manage the Compose test environment.
          
          class MyComposeUITest{
           @get:Rule
           val composeTestRule: ComposeTestRule = createComposeRule()
          }

##### Step 2: Set Up the Content

Use the "setContent" method to set the composable content for the test.

         @Test
        fun testMyComposable()
        {
          composeTestRule.setContent{
            MyComposable()
          }
        }

##### Step 3: Define UI Test Cases

Use the Compose testing APIs to interact with and verify the state of your composables. Here are some examples:

 1. Verify Text Content

 --> Ensure that a specific text is displayed.

          class MyComposeUITest{
            @get:Rule
            val composeTestRule = createComposeRule()
          
           @Test
           fun testMyComposable()
           {
             composeTestRule.setContent{
               MyComposable()
             }
               composeTestRule.onNodeWithText("Hello, World!").assertIsDisplayed()
           }
          }

 2. Test Button Click

--> Verify that a button click updates the UI as expected.
     
     class MyComposeUITest{
          @get:Rule
          val composeTestRule = createComposeRule()
     
         @Test
         fun testButtonClick(){
           composeTestRule.setContent{
              CounterScreen()
           }
           composeTestRule.onNodeWithText("Increment").performClick()
           composeTestRule.onNodeWithText("Count:1").assertExists()
         }
       }

 3. Interacting with Inputs

  --> Verify that text inputs fields work correctly.

class MyComposeUITest{
   @get:Rule
   val composeTestRule = createComposeRule()

   @Test
   fun testTextInput(){
     composeTestRule.setContent{
        MyTextInput()
     }
     composeTestRule.onNodeWithTag("inputField").performTextInput("Compose Testing")
     composeTestRule.onNodeWithTag("inputField").assertTextEquals("Compose Testing")
   }
}
  
