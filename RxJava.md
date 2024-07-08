## RxJava

---> It primary building blocks are Opeartor, Observer and Observables.

i) **Observable**: It emits a stream of data or events that can be used to perform some action and publish the results.

      Example: Observable observable = Observable.just("A","B","C","D","E","F")

ii) **Observer**: That class receives the events or data and acts upon a class that waits and whether the observable and reacts whenever the observable publish the results.

It has **4** interfaces:

        a) onSubscribeOn(): It is invoked when the observer is subscribed to the Observable.
        
        b) onNext() : When the new item is emitted from the observable.
        
        c) onError(): It is called when an error occurs and the emission of data is not successfully completed.
        
        d) onComplete(): This method is called when the observable has successfully completed emitting all items.

#### RxJava Operators:

    i) create(): It creates an observable from scratch by calling observer method programmatically.

    ii) Defer(): It does not create observable until the observer subscribes.

    iii) from() : This operator creates an observable from set of items using an iterable, which means we can pass a list or an array of items to ther observable and each item is emitted one at a time, (e.g) fromArray(), fromIteratable()

    iv) interval(): This operator creates an observable that emits a sequence of integer spaced by a particular time interval.

    v) just() : It takes a list of arguments and converts the items into observable items. Just() make only one emission.

    vi) range(): It creates an observable that emits a range of sequential integers. The function takes two arguments the starting number and length.

          Ex: Observable.range(2,5)

    vii) repeat(): It creates an observable that emits a particular item or sequence of items repeatedly.

          Ex: Observable.range(2.5).repeat(2)

    viii) timer(): It creates on observable that emits one particular item after a span of time that you specified.

          Ex: Onservable.timer(1, TimeUnit.SECONDS)


##### Difference between observable.from() and observable.just()

----> observable.just() emits only once whereas observable.from() emits n-times the length of the array.

            Ex: 
             fun main()
             {
                  Observable.just(Data.users).subscribe{ println(it) }
             }
      
           Ex: 
             fun main()
             {
                 Observable.fromIteratable(Data.users).subscribe{ println(it) } 
             }

##### Difference between timer() and intervals()

----> Timer() emits just a single item after a delay.

----> interval() emit items spaced out with a given interval.


----> **SubscribeOn()** decides on which thread source observable will run. This is useful to decide whether to run the sourcce in the main thread or background thread.

----> **observeOn** decides on which thread downstream operators will run. This is useful for switching thread between two operators.

### Subject in RxJava:

---> A subject extends an observable and implements observer at the same time. It acts as an observable to clients and registers to multiple events taking place in the app. It acts as an observer by broadcasting the event to multiple subscribers.

 ---> Subjects can acts as both an observer and an observable.

 ----> Subjects are considered as HOT observables.

  Types of subjects are as below

  i) **Publish Subject**:

    ---> It emits all the subsequent items of observable at the time of subscription.

    Example: 

          If a student entered late into the classroom, he just want to listen from that point of time when he entered the classroom. So, publish will be the best usecase.

  ii) **Replay Subject**

    ---> It emits all the items of the source observable regardless of when the subscriber subscribes.

    Example:

          If a student entered late into the classroom, he want to listen from the begining. So, here we will use replay.

  iii) **Behaviour Subject**     

      ---> It emits the most recently emitted item and all the subsequent items of the source observable when an observer subscribe to it.

      Example:

            If a student entered late into the classroom, he wants to listen to the most recent things(not from the begining) being taught             by professor so that he gets the idea of context.

  iv) **Async Subject**

        ---> It emits the last value of the source observable (and only the last value) only after that source observable completes.

        Example:

              If a student entered the classroom at any point in time, and wants to listen only to the last thing(and only the last thing)                being taught after a class is over. So we will use async.
  
