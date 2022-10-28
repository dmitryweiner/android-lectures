## Сетевые запросы

![network requests](assets/network/api.jpg)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

### Подготовка приложения
* Добавить права в AndroidManifest.xml:
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
* Если нужно обращаться к не HTTPS серверам, надо добавить в `<application>` `usesCleartextTraffic`:
```xml
<application
        android:usesCleartextTraffic="true"
>
   <!-- ... -->
</application> 
```
---

### Запрос с помощью потоков
```kotlin
thread {
        val json = try {
            URL(url).readText()
        } catch (e: Exception) {
            return@thread
        }
        runOnUiThread { displayOrWhatever(json) }
}
```
    
---

### Запрос с помощью корутин

```kotlin
suspend fun getUserById(id: Int): User? {
    return withContext(Dispatchers.IO) {
        try {
            val client = HttpClient(CIO)
            val response: HttpResponse = client.get("https://jsonplaceholder.typicode.com/users/$id")
            client.close()
            val str = response.readText()
            val itemType = object : TypeToken<User>() {}.type
            Gson().fromJson<User>(str, itemType)
        } catch (e: Exception) {
            null
        }
    }
}

button.setOnClickListener {
    lifecycleScope.launch {
        val user = CoroutineScope(Dispatchers.IO).async {
            getUserById(1)
        }.await()
        textView.text = user?.name
    }
}
```
---
https://gist.github.com/dmitryweiner/1d004458752abce118e5b0ce2891e9da
https://gist.github.com/dmitryweiner/b27b2741ae58a047f587fedeba0c6755
