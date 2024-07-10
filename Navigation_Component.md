**NavHostFragment**: It act as a container for destinations.

**NavController**: Manages app navigation within a NavHost

**NavGraph**: Defines all navigation paths within the app.

### How to pass arguments using Navigation Components?

**Step 1**: 

        Bundle bundle = new Bundle()
        bundle.putString("amount",amount)
        Navigation.findNavController(view).navigate(R.id.action, bundle)

   Receive:

         TextView tv = view.findViewById(R.id.textViewAmount)
         tx.setText(setArguments().getString("amt"))


**Step 2**: Use SafeArgs(Type Safety)

        <action id = "@+id/action_nav_to_otp"
           app:destination = "@+id/nav-verify_otp">

         <argument 
             android:name = "mobile"
             app:argType = "String"/>

         <argument
             android:name = "password"
             app:argType = "String"/>

         </action>


         val bundle = bundle("mobile" to et.getText().toString())
         Navigation.findNavController(binding.root).navigate(R.id.action, bundle)


Receive:

        override fun onActivityCreated(savedInstanceState: Bundle?)
        {
          super.onActivityCreated(savedInstanceState)
          mobileNo = arguments !!.getString("mobile").toString()
          password = arguments !!.getString("password").toString()
        }


### How to change start destination of navigation graph programmatically?

**Type1**: Using NavOptions:

                NavOptions navOptions = new NavOptions.Builder().setPopUpTo(R.id.FirstFragment, true).build()
                Navigation.findNavController(view).navigate(R.id.action_fragment, bundle, navOptions)

**Type2**: Alternative way:

                findNavController(fragment).navigate(FirstFragmentDirections.actionFirstFragmentToSecondFragment)


 ### How to go back to previous fragments?

  --> findNavController().popBackStack()

  If you are setting safe-args library in gradle file then the following fragmentDirection class automatically generated.

             val action = FirstFragmentDirections.actionFirstFragmentToSecondFragment(yourData)
             navController.navigate(action)
