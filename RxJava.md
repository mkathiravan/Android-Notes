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

##### Difference between timer() and intervals()

----> Timer() emits just a single item after a delay.

----> interval() emit items spaced out with a given interval.


    
