#### Difference between observe and observeForever?

 - **Lifecycle Awareness**:
     - observe: Observer is tied to the lifecycle of a LifecycleOwner.
     - observeForever: Observer is not tied to any lifecycle, remaining active until explicitly removed.

  - **Automatic Removal**:
      - observe: Observer is automatically removed when the LifecycleOwner is destroyed.
      - observeForever: Observer must be manually removed, or it can lead to memory leaks.

  - **Typical Use Cases**:
      - observe: Use in UI components where the observer should only be active when the component is in a valid state.
      - observeForever: Use in non-UI components or applicable-wide services where lifecycle awareness is not necessary or desired.    
