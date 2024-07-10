3 Different Test cases

**i)UI Tests**: UI related. Expresso is used.

**ii)Instrumented Tests**: User actions like button click. Expresso used.

**iii) Unit Tests**: JUnit, Mockito

## Mockito

i) **@Runwith**: We have annotated with MockitoJUnitRunner::class which means it provides the runner to run the test.

ii) **@Mock**: Using @Mock annotation we can mock any class mocking any class is nothing but to create a mock object of a particular class.

iii) **@Before**: It run befor each test, Incase if you want to run common code this is useful.

iv) **@Test**: To perform the test we need to annotate the functions.

v) **@After**: It is used to run after test cases completes.

vi) **@BeforeClass**: It is used when you want to run a common operation that too expensive to run multiple times.

vii)**@Afterclass**: Used for deallocation of any reference often all tests executed successfully.

viii) **when()**: It is used to configure simple return behavior for a mock or spy object.

ix) **doReturn()**: It is used when we want to return specific value when calling a method on a mock object.

x) **thenReturn()**: This method lets you define the return value when a particular method object is been called.


#### Difference between Mockito when/thenReturn & doReturn/when?

 **When/thenReturn** ---> actually invokes the methods and returns a mocked value.

     Ex:
           List mockList = mock(List.class)
           when(mockList.size()).thenReturn(3)

 **doReturn/when** --> Does not invoke the method and only returns a mocked value.

    Ex:

          List mockList = mock(List.class)
          doReturn(3).when(mockList).size()
