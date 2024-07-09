### Example 1:

     suspend fun showProfile() = CoroutineScope{
         val profile = fetchProfile()
         showProfileData(profile)
         launch{
             showProfileImage(fetchImage(profile))
         }
     }

The above program if there is a loader hidden after showProfile call, it will not be hidden until fetchImage is completed. So, if we add Job() inside launch it will solve this problem but it will break the structured concurrency.

    suspend fun showProfile() = CoroutineScope{
         val profile = fetchProfile()
         showProfileData(profile)
         launch(Job()){
             showProfileImage(fetchImage(profile))
         }
     }

Instead of using Job builder, we should start this coroutine in an appropriate scope.

         suspend fun showProfile(){
         val profile = fetchProfile()
         showProfileData(profile)
         viewModelScope.launch{
             showProfileImage(fetchImage(profile))
         }
     }


### Example 2:

     suspend fun sendEmail(email: List<String>) = withContext(SupervisorJob + Dispatchers.IO)
     {
          email.forEach{
             launch{
                  sendEmail(it)
             }  
          }
     }

The above program was incorrect when you are handling SupervisorJob so correct way as below

     suspend fun sendEmail(email: List<String>) = withContext(Dispatchers.IO)
     {
          supervisorScope{
          email.forEach{
             launch{
                  sendEmail(it)
             }  
          }
          }    
     }
