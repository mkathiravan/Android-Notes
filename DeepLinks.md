## How deep links verify against the host name in android

---> Deep links in Android are verified against the host name using a process called "App Links" or "Digital Asset Links"(DAL). This process ensures that a specific URL should be opened by a particular app instead of a web browser or other apps. 

Here's an overview of how this verification works:

### Steps for Deep Link Verification in Android

 **1.Define Intent Filters in the Manifest:**

   -> In  your app's AndroidManifest.xml, you need to define intent filters for the activities that should handle the deep links. This includes specifying the schema, host and path patterns for the URL's your app can handle.
