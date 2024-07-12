### Understanding CoroutineScope Delegation in Kotlin:

When working with coroutines in Kotlin, it's essential to choose the right approach for delegation. Here are two common patterns:

#### CoroutineOne:

---> Delegates the CoroutineScope implementation to a new instance of CoroutineScope. It's concise and useful when you want a quick setup.

#### CoroutineTow:

---> Implements CoroutineScope and provides the coroutine context directly. Offers more contorl and flexibility, especially when you need to extend the functionality.

##### Which to use?

---> Use CoroutineOnce 
