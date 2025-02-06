### Difference between AppCompatActivity vs ComponentActivity in Android?

##### AppCompatActivity:
i) Legacy Support: Derived from FragmentActivity

ii) AppCompatActivity offers background compatibility with older Android Version (API 14+)

iii) Material Design: Provides support for modern UI components and features like Toolbar and ActionBar on older devices.

iv) Action Bar: Automatically supports action bar features(if needed)

v) Use Case: Best for apps that need background compatibility, particularly if your app targets a wide range of Android versions.

##### ComponentActivity:
i) Modeern Base Class: Introduced in newer Android versions, ComponentActivty is more lightweight and flexibile base class.

ii) No Action Bar: Does not automatically manage UI elements like ActionBar, giving you more control over your UI.

iii) Jetpack Compose: If you are using Jetpack compose for UI development, ComponentActivity is the recommended base class, as it's designed to work seamlessly with Compose.

iv) Use Case: Ideal for modern apps, especially when targeting new Android versions or building apps with Jetpack compose
