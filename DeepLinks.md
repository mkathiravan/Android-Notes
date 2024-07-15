## How deep links verify against the host name in android

---> Deep links in Android are verified against the host name using a process called "App Links" or "Digital Asset Links"(DAL). This process ensures that a specific URL should be opened by a particular app instead of a web browser or other apps. 

Here's an overview of how this verification works:

### Steps for Deep Link Verification in Android

 **1.Define Intent Filters in the Manifest:**

 - In  your app's AndroidManifest.xml, you need to define intent filters for the activities that should handle the deep links. This includes specifying the schema, host and path patterns for the URL's your app can handle.

            <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter android:autoVerify="true">
               <action  android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <category android:name="android.intent.category.BROWSABLE"/>
                <data android:scheme="https"
                    android:host="www.example.com"
                    android:pathPrefix="/path"/>
            </intent-filter>
            </activity>


 **2.Setting up Asset Links JSON File:**

    - The host website must host a Digital Asset Links file at https://<domain>/.well-known/assetlinks.json. This JSON file specifies which apps are allowed to handle URLs for that domain.

            [
               {
                  "relation":
                  ["delegate_permission/common.handle_all_urls"],
                  "target":{
                     "namespace": "android_app",
                     "package_name": "com.example.app",

                     "sha256_cert_fingerprints":[
                        "AB:CD:EF:.....12:34"
                     ]
                  }
               }
            ]

 **3.Verification by the System:**

  - When a users attempts to open a URL, the Android system verifies the deep link by checking the assetLinks.json file on the specified host. It confirms that the URL's host matches the domains listed in the intent filters and that the app is authorized to handle these URLs as declated in the assetlinks.json file.

**4.Handling Verified Links:**    

  - If the verification is successful, Android will directly open the URL in the specified app without prompting the user to choose between apps. This ensures a seamless experience and increased security, as only verified apps can handle specific URLs.

 ##### Practical Example

 --> Suppose you have an app with the package name com.example.myapp and you want it to handle URLs from https://www.example.com

  **Mainfest Configuration:**

                   <activity android:name=".MainActivity"
                android:exported="true">
                <intent-filter android:autoVerify="true">
               <action  android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <category android:name="android.intent.category.BROWSABLE"/>
                <data android:scheme="https"
                    android:host="www.example.com"
                    android:pathPrefix="/path"/>
                 </intent-filter>
                 </activity>

**Digital Asset Links File**

--> (https://www.example.com/.well-known/assetlinks.json):

          [
               {
                  "relation":
                  ["delegate_permission/common.handle_all_urls"],
                  "target":{
                     "namespace": "android_app",
                     "package_name": "com.example.app",

                     "sha256_cert_fingerprints":[
                        "AB:CD:EF:.....12:34"
                     ]
                  }
               }
            ]

**Verification Process**

---> When a user clicks on a link to https://www.example.com, Android checks the assetlinks.json file at https://www.example.com/.well-known/assetlinks.json. It verifies that com.example.myapp is listed as a trusted handler of the URL. If the verification passes, the link opens in com.example.myapp automatically.

    This process ensures that deep links are securely associated with your app, preventing unauthorized apps from intercepting and handling your app's intented URLs.

    
