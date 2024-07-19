## Difference between app links and deep links in android

  ### App Links:

  **Definition**: App links are a type of deep link that are specifically designed to open an app when a URL is clicked, rather than opening the URL in a web browser.

  **Verification**: App links are verified by the system using Digital Asset Links ensuring that only the intended app can open the link. This requires hosting a file on the web server and including an intent filter in the app.

   **Usage**: Commonly used for scenarios where you want to seamless transition from a web link to the app, such as a link in an email or on a website.

### Deep Links:

   **Definition**: Deep links are URLs that link directly to specific content or activities within an app.

   **Types**:

   **Traditional Deep Links**: These require the app to be installed and rely on a specific URL scheme to navigate to the desire content.

   **Deferred Deep Links**: These are used when the app is not installed. They store the deep link and trigger the appropriate content after the app is installed.

   **Usage**: Often used to navigate users directly to content within the app from notifications, web pages or other apps.

### Key Differences:

 **Verificaton**: App links require verification through Digital Asset Links, whereas traditional deep links do not.

 **User Experience**: App links provide a more secure and seamless user experience, as they ensure the link opens in the correct app.

**Installation**: App links can only work if the app is installed and verified, while traditional deep links can be handled by any app that declares support for the URL scheme.

**Example**:

 **App Link**: A link to https://www.example.com/somecontent that opens directly in the example app if installed and verified.

 **Deep Link**: A custom URL scheme like example://somecontent that opens the Example app to the specified content if the app is installed.
      

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

 - This process ensures that deep links are securely associated with your app, preventing unauthorized apps from intercepting and handling your app's intented URLs.

    
