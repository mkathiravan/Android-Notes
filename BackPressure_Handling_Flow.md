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

The buffer operator has the following options are

            buffer(capacity = 1, onBufferOverflow = BufferOverflow.SUSPEND)
            buffer(capacity = 1, onBufferOverflow = BufferOverflow.DROP_OLDEST)
            buffer(capacity = 1, onBufferOverflow = BufferOverflow.DROP_LATEST)


#### i) Buffer operator:

           val flows = flow {
                  repeat(30) {
                      Log.d(TAG, "Emitting_Values $it")
                      emit(it)
                      delay(1000)
                  }
              }

             GlobalScope.launch {
            //Using Buffer Operator
            flows.buffer(10)
                .collect { value ->
                    delay(5000)
                    Log.d(TAG, "Buffered_Value: $value")

                }
                }

#### ii) Conflate operator:

            val flows = flow {
              repeat(30) {
                  Log.d(TAG, "Emitting_Values $it")
                  emit(it)
                  delay(1000)
              }
          }

            GlobalScope.launch{
              flows.conflate().collect { conflateValue ->
                delay(5000)
                Log.d(TAG, "Conflate_value: $conflateValue")

            }
            }

#### iii) Sliding Window:

            flows.buffer(10, BufferOverflow.DROP_OLDEST).collect { value ->
                delay(5000)
                Log.d(TAG, "Sliding Value: $value")
            }

            flows.buffer(10, BufferOverflow.DROP_LATEST).collect { value ->
                delay(5000)
                Log.d(TAG, "Sliding Value: $value")
            }

            flows.buffer(10, BufferOverflow.SUSPEND).collect { value ->
                delay(5000)
                Log.d(TAG, "Sliding Value: $value")
            }

#### iv) Sample operator:

            flows.sample(2000).collect { value ->
                delay(5000)
                Log.d(TAG, "Sliding Value: $value")
            }

#### v) Debounce operator

           flows.debounce(2000).collect { value ->
                Log.d(TAG, "Deboucing_value,$value")
           }     

#### vi) Drop operator

            flows.drop(10).collect { value ->
            Log.d(TAG, "Dropping_value,$value")
            }

#### vii) Lastest operator

            flows.collectLatest { value ->
            delay(2000)
            Log.d(TAG, "Collect_Latest_value,$value")
            }
