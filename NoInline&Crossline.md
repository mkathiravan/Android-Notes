## NoInline:

--> Assume that we do not want all of the lambdas passed to an inline function to be inlined, then we can mark as noinline keyword.

          fun guide()
          {
              println("Guide start")
              teach(
                  {
                      println("Teach abc")
                  },{
                      println("Teach xyz")
                  }
              )
              println("Guide end")
          }
          
          inline fun teach(abc: () -> Unit, noinline xyz: () -> Unit)
          {
              abc()
              xyz()
          }
          
          fun main() {
              guide()
          }

The above program the teach xyz compiler won't allocate of virtual so it is consume heavy memory overhead.

#### Cross-line:

--> In Kotlin is used to avoid non-local returns.
            
            fun guide()
            {
                println("Guide Start")
                teach{
                    println("Teach")
                    return
                }
                println("Guide end")
            }
            
            inline fun teach (crossinline abc: () -> Unit)
            {
                abc()
            }
            
            fun main() {
               guide()
            }

If you run the above program it will shows message as "'return' is prohibited here."            
