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
* Если нужно обращаться к __не HTTPS__ серверам, надо добавить в `<application>` `usesCleartextTraffic`:
```xml
<application
        android:usesCleartextTraffic="true"
>
   <!-- ... -->
</application> 
```
---

### Варианты
* Делать запрос с помощью `HttpURLConnection` из пакета `java.net` (ставить не надо).
* Использовать библиотеку [Ktor](https://ktor.io/) (надо ставить).
---

### Функция для получения данных по URL'у
```kotlin
fun getContent(url: String?, timeout: Int = 10000): String? {
    var c: HttpURLConnection? = null
    try {
        val u = URL(url)
        c = u.openConnection() as HttpURLConnection
        c.setRequestMethod("GET")
        c.setRequestProperty("Content-length", "0")
        c.setUseCaches(false)
        c.setAllowUserInteraction(false)
        c.setConnectTimeout(timeout)
        c.setReadTimeout(timeout)
        c.connect()
        val status: Int = c.getResponseCode()
        when (status) {
            200, 201 -> {
                val streamReader = InputStreamReader(c.inputStream)
                var text = ""
                streamReader.use {
                    text = it.readText()
                }
                return text
            }
        }
    } catch (ex: MalformedURLException) {
        Logger.getLogger(javaClass.name).log(Level.SEVERE, null, ex)
    } catch (ex: IOException) {
        Logger.getLogger(javaClass.name).log(Level.SEVERE, null, ex)
    } finally {
        if (c != null) {
            try {
                c.disconnect()
            } catch (ex: Exception) {
                Logger.getLogger(javaClass.name).log(Level.SEVERE, null, ex)
            }
        }
    }
    return null
}
```
---

### Парсим JSON
```kotlin
data class User(val id: String, val name: String)

fun getUserById(id: String): User? {
    val response = getContent(
        "https://jsonplaceholder.typicode.com/users/$id",
        1000
    )
    var jsonObject: JSONObject? = null
    try {
        jsonObject = JSONObject(response)
        val user = User(
            jsonObject.getString("id"),
            jsonObject.getString("name")
        )
        return user
    } catch (e: JSONException) {
        e.printStackTrace()
    }
    return null
}
```
* Альтернатива JSONObject: использовать библиотеку [Gson](https://github.com/google/gson/blob/master/UserGuide.md).
---

### Запрос с помощью потоков
```kotlin
button.setOnClickListener {
    thread {
        val user = getUserById("1")
        runOnUiThread { // выполнение в UI-треде
            // выводим имя пользователя, например
            textView.text = user.name
        }
    }
}
```
---

### Запрос с помощью корутин
```kotlin
button.setOnClickListener {
    // Dispatchers.IO для операций ввода-вывода
    lifecycleScope.launch(Dispatchers.IO) {
        val user = getUserById("1")
        // для обращения к UI меняем поток
        withContext(Dispatchers.Main) {
            textView.text = user?.name
        }
    }
}
```
---

### Слишком многословно?
* Приведённое решение страдает многословностью.
* Для удобной работы с API следует пользоваться библиотекой `Retrofit`.

---

### Задачи
* Написать приложение, отображающее форму:
  <br/><label>
  ID:
  <input>
  </label>
  <button>Получить данные!</button><br/>
* При нажатии кнопки приложение обращается по адресу https://jsonplaceholder.typicode.com/posts/:id.
* После этого приложение обращается по адресу https://jsonplaceholder.typicode.com/users/:userId,  
  где `userId` получен из предыдущих данных.
* Показать экране: из post - поля `title` и `body`, из user - поля `name`, `email`.
