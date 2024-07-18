### Data Layer:

        interface ApiInterface {
            @GET("/users")
            suspend fun getUsers(): List<UserDto>
        }


         object RetrofitInstance {
          private val retrofit by lazy {
              Retrofit.Builder().baseUrl("https://jsonplaceholder.typicode.com")
                  .addConverterFactory(GsonConverterFactory.create()).build()
          }
          val apiService: ApiInterface by lazy {
              retrofit.create(ApiInterface::class.java)
          }
      }

      data class UserDto(val id: Int, val name: String, val username: String, val email: String)
      {
          fun toUser() : User
          {
              return User(id, name, username, email)
          }
      }

      class UserRepositoryImpl(private val apiInterface: ApiInterface): UserRepository{
      override suspend fun getUsers(): Result<List<User>> {
        return try {
           val remoteUsers = apiInterface.getUsers().map { it.toUser() }

            Result.success(remoteUsers)
        }catch (e: IOException)
        {
         Result.failure(e)   // Network
        }catch (e: HttpException)
        {
            Result.failure(e)
        }catch (e: Exception)
        {
            Result.failure(e)
        }

        //return apiInterface.getUsers().map { it.toUser() }
    }
    }


### Domain Layer

    data class User(val id: Int, val name: String, val username: String, val email: String)

    interface UserRepository {
        suspend fun getUsers(): Result<List<User>>
      }

    class GetUsersUseCase(private val userRepository: UserRepository) {
    suspend fun execute(): Result<List<User>>
    {
        return userRepository.getUsers()
    }
    }  

### Presentation Layer

                class UserViewModel(private val getUsersUseCase: GetUsersUseCase): ViewModel() {
                private val _users = MutableLiveData<List<User>?>()
                val users: LiveData<List<User>?> get() = _users
            
                private val _error = MutableLiveData<String>()
                val error: LiveData<String> get() = _error
            
                init {
                    fetchUsers()
                }
            
                fun fetchUsers()
                {
                    viewModelScope.launch {
                        val result = getUsersUseCase.execute()
                        if(result.isSuccess){
                            _users.value = result.getOrNull()
                        }else if(result.isFailure)
                        {
                            _error.value = handleFailure(result.exceptionOrNull())
                        }
            
                    }
                }
            
                private fun handleFailure(exception: Throwable?) : String{
                    return when(exception)
                    {
                        is IOException -> "Network Error, please try again"
                        is HttpException -> "Server Error: ${exception.code()}"
                        else -> "Unexpected error occured"
                    }
                }
            }



            @Suppress("UNCHECKED_CAST")
            class UserViewModelFactory(private val getUsersUseCase: GetUsersUseCase) : ViewModelProvider.Factory{
            
                override fun <T : ViewModel> create(modelClass: Class<T>): T {
                    if(modelClass.isAssignableFrom(UserViewModel::class.java))
                    {
                        return UserViewModel(getUsersUseCase) as T
                    }
                    throw IllegalArgumentException("Unknown ViewModel class")
                }
            }



            class MainActivity : ComponentActivity() {

                  private lateinit var userViewModel: UserViewModel
              
                  override fun onCreate(savedInstanceState: Bundle?) {
                      super.onCreate(savedInstanceState)
                      setContentView(R.layout.activity_main)
              
                      val apiService = RetrofitInstance.apiService
                      val userRepository = UserRepositoryImpl(apiService)
                      val getUsersUseCase = GetUsersUseCase(userRepository)
                      val viewModelFactory = UserViewModelFactory(getUsersUseCase)
              
                      userViewModel = ViewModelProvider(this, viewModelFactory).get(UserViewModel::class.java)
              
                      userViewModel.users.observeForever {
                          Log.d("List_of_user","List_of_user ${it.toString()}")
                      }
                      userViewModel.error.observeForever {
              
                      }
              
                  }
              }
