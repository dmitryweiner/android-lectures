AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.myapplication">
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    
    <!-- ... -->
</manifest>
```

thread {
        val json = try {
            URL(url).readText()
        } catch (e: Exception) {
            return@thread
        }
        runOnUiThread { displayOrWhatever(json) }
    }
    
    
```kotlin    
button3.setOnClickListener {
    lifecycleScope.launch {
        val jsonStr = CoroutineScope(Dispatchers.IO).async {
            val jsonStr = URL("https://jsonplaceholder.typicode.com/posts/1").readText()
            return@async jsonStr
        }.await()
        textView.text = jsonStr
    }
}
```
    If you need to test with plain http don't forget to add to your manifest: android:usesCleartextTraffic="true"

https://gist.github.com/dmitryweiner/1d004458752abce118e5b0ce2891e9da

https://gist.github.com/dmitryweiner/b27b2741ae58a047f587fedeba0c6755
