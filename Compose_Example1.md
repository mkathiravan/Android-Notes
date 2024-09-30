### consumeAsFlow and receiveAsFlow in Jetpack Compose

--> ConsumeAsFlow and receiveAsFlow are utilities provided by Kotlin Coroutines to integrate channels with Flow API's. These utilities are commonly used to convert data from channels into reactive streams of Flow, making them easier to handle in a declarative way, especially when working with Compose.

**1.consumeAsFlow:**

This function is used to convert a channel into a Flow so that you can consume the channel as a stream of values in a reactive manner. This is a one-time event, once the message has been sent and consumed, it doesn't need to be available for other collectors.

Example:

          fun main() = runBlocking{
              val channel = Channel<Int>()
              launch{
                for(i in 1..5){
                  channel.send(i)
                }
                channel.close()
              }
              //Convert the channel to a Flow
              channel.consumeAsFlow().collect{value -> println("Received $value")}
          }

--> In the above example consumeAsFlow turns the channle into a Flow that can be collected, and the values are received reactively.

**UseCases:**

-- Use when you want a single consumer (e.g a one-time UI event like a Snackbar or navigation action).

-- Once collected, no other parts of the UI can collect the flow.

-- Ideal for short-lived, one-time events.

-- Typically used with SendChannel or BroadcastChannel.

**Cancellation semantics:**

-- Flow collector is cancelled when the original channel is closed with an exception.

-- Flow collector completes normally when the original channel is closed normally.

-- If the flow collector fails with an exception, the source channel is cancelled.


**2.receiveAsFlow:**

--> receiveAsFlow is used with a ReceiveChannel or a BroadcastChannel to expose the channel as a Flow.

--> The difference with consumeAsFlow is that receiveAsFlow doesn't consume the entire channel, allowing other consumers to also receive from the channel.

Example:

               fun main() = runBlocking{
                    val broadcastChannel = BroadcastChannel<Int>(5)
                    launch{
                       for(i in 1..5){
                         broadcastChannel.send(i)
                       }
                       broadcastChannel.close()
                    }
                  // Convert BroadcastChannel to a Flow using receiveAsFlow
                  broadcastChannel.receiveAsFlow().collect{value -> println("Received $value")}
               }


--> In the above examples, receiveAsFlow is used with BroadcastChannle to turn it into a Flow while allowing for multiple consumers if needed.

**UseCases:**

--> Use when you have multiple consumers for the same data stream(e.g real-time updates like chat messages, stock prices, or location data)

--> Multiple parts of the UI can collect the same flow without consuming the underlying channel.

--> Ideal for long-lived, broadcasted state or data.

**Cancellation Semantics:**

--> Flow collectors are cancelled when the original channel is closed with an exception.

--> Flow collectors complete normally when the original channel is closed normally.

--> Failure or cancellation of the flow collector does not affect the channel.
