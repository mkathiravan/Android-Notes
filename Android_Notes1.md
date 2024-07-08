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

