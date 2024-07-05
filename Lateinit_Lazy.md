## Lateinit vs Lazy

#### Lateinit 

---> We may not want to initialize our values at declaraction time but instead want to initialize and use them at any later time in our application. However before using our value we must not forgot to initialize before it can be used. Without initializing if we want to access it will throw "**UninitializedPropertyAccessException**"

#### Lazy

---> It is design pattern we can create objects only the first time that we access them. Otherwise we don't have to initialize them.

<img width="657" alt="Screenshot 2024-07-05 at 1 45 39 PM" src="https://github.com/mkathiravan/Android-Notes/assets/39657409/91f68011-99b7-4d78-aff2-7a3605057f4b">


