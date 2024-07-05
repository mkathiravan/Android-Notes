## Android LaunchModes

#### Single Task

---> If the task dont have an existing instance of the activity, then a new instance is created that is similar to singletop.

---> If the tasks have an existing instance of the activity and it's at the top of the activity, then system will pass the intent data to the onNewIntent() function.

---> If the tasks have an existing instance but not at the top, then it will rollback to that activity and destroy the activities on the top of the singleTask Activity.

 ##### Example

  ---> 3 Activities A,B,C and B is defined as Single Task

       Step 1: Launch A -> A
       Step 2: Launch B -> A - B
       Step 3: Launch C -> A - B - C
       Step 4: Launch B -> A - B
       Step 5: Launch B -> A


#### Single Instance

---> When you invoke the activity with SingleInstance then the system will create a new special task that will only one singleInstance activity in it.

##### Example

 ----> 3 Activities A. B, C and B is defined as SingleInstance,

        Step 1: Launch A -> Task 1: A
        Step 2: Launch B -> Task 1: A
                            Task 2: B
        Step 3: Launch C -> Task 1: A - C
                            Task 2: B
        Step 4: Launch B -> Task 1: A - C
                            Task 2: B*
