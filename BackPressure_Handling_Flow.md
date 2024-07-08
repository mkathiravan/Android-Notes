## BackPressure Handling:

---> Incase the producer is emitting events continously but consumer is receiving the value once the consuming.

---> When data is produced faster than it can eb consumed.

----> **Backpressure** is a term in reactive streams which describes the behavior when consumer is not consuming events that fast as producer produces them.

---> If we are not handling the backpressure might lead to either blocking your code or outofmemory error.

##### Example 1:

      suspend fun main = corotineScope{
        val flow = flow{
              repeat(5){
                val index = it + 1
                emit(index)
              }
          }.buffer(capacity = 1, onBufferOverflow = BufferOverflow.SUSPEND)
          flow.collect{
          
          }
      }
