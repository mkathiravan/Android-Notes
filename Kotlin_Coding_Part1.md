### EXample 1:

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
