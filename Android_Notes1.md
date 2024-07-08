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


## High-Level Design (HLD)

  **Purpose**: Provides an overview of the system architecture and design.

  **Focus**: Describes the sytem's architecture, components, modules and their interactions. It also includes the technology stack, frameworks and tools to be used.

  **Audience**: Typically for project managers, stakeholders, and senior developers.

  **Components:**

      i) System Architecuture: Overview of the architecture, such as MVP, MVVM, Clean Architecture.

      ii) Modules and Layers: How application is divided into different layers or modules.

      iii) Component Interaction: Interaction between different modules and components.

      iv) Technology Stack: The technologies, libraries and framework to be used.(Ex: Retrofit, Room)

      v) Non-Functional Requirements: Considerations like performance, scalability.
      

## (LLD)

   **Purpose**: Provide detailed view of the system implementations.

  **Focus**: Describes the implementation details of each components, including classes, methods and data structures.

  **Audience**: Primarily for developers and software engineers who will implement the system.

  **Components:**

      i) Class Diagrams: Detailed diagrams showing classes, their attributes, methods, and relationships.

      ii) Sequence Diagrams: Diagrams showing how objects interact in a particualr scenario of a use case.

      iii) State Diagrams: Representing the states of objects and their transitions.

      iv) Detailed Algorithms: Pseudocode or detailed description of the algorithms used.

      v) Database Schema: Detailed design of the databse tables, their relationships and constraints.
 
 ##### Example of HLD:

    1. Architecture Choice: Deciding to use MVVM.

    2. Modules: 

        UI Layer: Activities and Fragments.

        ViewModel Layer: ViewModel classes.

        Repository Layer: Hanldes data operations.

        Data Layer: Room Database, Retrofit for API calls.

   3. Technology Stack: Kotlin, Android Jetpack components, Retrofit, Room, LiveData

      
##### Example of LLD:

     1. Class Diagram:

         MainActivity: Handles user interaction

         MainViewModel: Provides data to the UI

         UserRepository: Manages data from API and local database.

         User: Data Model class


    2. Sequence Diagram:

        User clicks a button ->

        MainActivity calls MainViewModel  -> MainViewModel request data from UserRepository -> UserRepository fetches data from     
        API/Database -> Data is returned and displayed in MainActivity.

    3. State Diagram:

        State of a network request: idle, loading, success, error

    4. Database Schema:

        Table: Users

        Columns: id, name, email

