## Aggregation and Composition

--> Both are used to describe the relationships between classes and objects.

#### Aggregation:

---> It represents has a relationship where one class contains reference to another class, but the child object can exist independently of the parent.

###### Example:

  ---> Android an activity can have a reference to a viewModel. The activity owns the viewModel and they have a relationship where the activity holds a reference to the viewModel, but the viewModel can exist beyond the lifetime of any specific activity.

###### Charactertics:

   i) The child object (viewModel) can exists independently.

   ii) There's a unidirectional relationship where the parent controls the creation and lifetime of the child.

   iii) Lifecycle managment of the child object is typically handled by the parent.

   
#### Composition:

 ---> It also represents a has-a relationship but with a stronger bond where the child object is part of the parent object and cannot exist without it.

###### Charactertics:

    i) The child object (Fragment) is an integrated part of the parent object (Activity)

    ii) The child object's lifecycle is tightly bound to the parent object's lifecycle.

    iii) If the parent object is destroyed, then the child object is also destroyed.


## UML Diagram(Unified Modeling Language)

---> It is used to visualize the design of a system. It can help in understanding the architecture, relationships between different components and the overall structure of the applications. There are various types of UML Diagram.

  **i)Class Diagram** : It shows the structure of the system classes, their attributes, methods  and the relationship among objects.

  **ii)Sequence Diagram**: It shows how objects interact in a particular scenario of use case. It is a type of interaction diagram that shows how processes operate with one another and in what order.

  **iii)Activity Diagram**: It represents the flow of activities or actions in a system. It is used to show the workflow from one activity to another.

  **iv)Component Diagram**: It shows how a system is split up into components and how they interact with each other. It helps in understanding the high-level structure of the system.


