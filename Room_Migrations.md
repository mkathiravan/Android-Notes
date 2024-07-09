## Understanding Room Migrations

Room support Automated Migration for following case:

 i) @DeleteTable
 
 ii) @RenameTable

 iii) @DeleteColumn

 iv) @RenameColumn

---> If app needs more requirement other than this we need to manage manual migrations because Room not able to migrate complex schema changes. And the read struggle start from here
        
        @Entity(tableName = "User")
        data class UserModel(
            @PrimaryKey(autoGenerate = true) var id: Long = 0L,
            @ColumnInfo(name = "user_name") var userName: String? = null,
        }

 Consider we just create this table and database with version 1

    @Database(entities = [UserModel::class], version = 1)
    // create database builder
    val database = Room.databaseBuilder(context, AppDatabase::class.java, "AppDatabase")
                   .build()       


 Now we add new column in User table to manage Manual Room Database Migration

     @Entity(tableName = "UserModel")
    data class UserModel(
        @PrimaryKey(autoGenerate = true) var id: Long = 0L,
        @ColumnInfo(name = "user_name") var userName: String? = null,
        @ColumnInfo(name = "email") var email: String? = null,
    }

If we try to run app it will crash with this error 👾

java.lang.IllegalStateException: Room cannot verify the data integrity. Looks like you’ve changed schema but forgot to update the version number. You can simply fix this by increasing the version number

Now we will change Room Database version

      @Database(entities = [UserModel::class], version = 2)  

Still it will gives an error 👾

java.lang.IllegalStateException: A migration from 1 to 2 is necessary. Please provide a Migration in the builder or call fallbackToDestructiveMigration in the builder in which case Room will re-create all of the tables.

Here, If you don’t want to manual migrations and you want your database to be cleared when you upgrade the version, call fallbackToDestructiveMigration() in the database builder like below:

      val database = Room.databaseBuilder(context, AppDatabase::class.java, "AppDatabase")
                   .fallbackToDestructiveMigration() 
                   .build()

Well done! App will run without fail now.

But if you want migration without lose all your previous version data follow manual migration process below :

First create migration variable and write migration query

    private val MIGRATION_1_2 = object : Migration(1, 2) {
        override fun migrate(database: SupportSQLiteDatabase) {
            database.execSQL("ALTER TABLE UserModel ADD COLUMN 'email' TEXT")
        }
    }

Add migration variable to database builder

        val database = Room.databaseBuilder(context, AppDatabase::class.java, "AppDatabase")
                   .addMigrations(MIGRATION_1_2)                   
                   .build()

🎊 Finally, Now run app it will run without fail with migration

You can write multiple migration query in single version. Let’s say we add new table AddressModel and add new column in UserModel table.

        @Entity(tableName = "AddressModel")
        data class AddressModel(
           @PrimaryKey(autoGenerate = true) var id: Long = 0L,
           @ColumnInfo(name = "city") var city: String? = null,
           @ColumnInfo(name = "country") var country: String? = null,
        }
        
         private val MIGRATION_2_3 = object : Migration(2, 3) {
             override fun migrate(database: SupportSQLiteDatabase) {
                 database.execSQL("ALTER TABLE UserModel ADD COLUMN 'phone_no' TEXT")
                 database.execSQL("CREATE TABLE IF NOT EXISTS `AddressModel` (`id` INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, `city` TEXT, `country` TEXT)
        
             }
        }

Add this migration to database builder and don’t forgot to change version number.

🤔 What if user have an older version of app with database version 1 and upgrade to database version 3? So, we already added migration from 1–2 and 2–3. Room will trigger all migrations one by one.

We can also define manually migration for more than one version increment. Write migration variable below.

         private val MIGRATION_1_3 = object : Migration(1, 3) {
             override fun migrate(database: SupportSQLiteDatabase) {
                 database.execSQL("ALTER TABLE UserModel ADD COLUMN 'email' TEXT")
                 database.execSQL("ALTER TABLE UserModel ADD COLUMN 'phone_no' TEXT")
                 database.execSQL("CREATE TABLE IF NOT EXISTS `Address` (`id` INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, `city` TEXT,   `country` TEXT)
         
             }
         }


Add this migration to your database builder

           val database = Room.databaseBuilder(context, AppDatabase::class.java, "AppDatabase")
                   .addMigrations(MIGRATION_1_2,MIGRATION_2_3,MIGRATION_1_3  )                   
                   .build()


#### Best Practices 📖

 ----> While migrating Room Database manually follow these 3 steps to get rid of any errors or failure

 i) Increase database version number
 
 ii) Write migration query in variable
 
 iii) Add migration variable in database builder                   
