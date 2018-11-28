# ApplicationProvider

Retrieve the android application from anywhere

```
val application = ApplicationProvider.application
```

You do not need to override the Application now

# Before

```kotlin
class MyApplication : Application() {
    override fun onCreate(){
        Stetho.initializeWithDefaults(application)
    }
}
```

# After

## Using a provider

```kotlin
class StethoInitializer : ProviderInitializer() {
    override fun initialize(): (Application) -> Unit = {
        Stetho.initializeWithDefaults(application)
    }
}
```

```xml
<provider
     android:name=".timber.TimberInitializer"
     android:authorities="${applicationId}.StethoInitializer" />
```

## Using an initializer

```
val InitializeStetho by lazy {
    ApplicationProvider.listen { application ->
        Stetho.initializeWithDefaults(application)
    }
}

class MainActivity : AppCompatActivity() {

    init {
        InitializeStetho
    }

    override fun onCreate(savedInstanceState: Bundle?) {
    ...
    }
}
```