# `Retrofit`
### Библиотека для обращения в API

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

### Установка в Gradle Scripts
* build.gradle (module): 
```
dependencies {
    // ...
    implementation 'com.squareup.retrofit2:retrofit:2.6.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.3.0'
    implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.5.1'
}
```
---

### Инициализация
```kotlin
val retrofit = Retrofit.Builder() // Base URL (для примера)
    .baseUrl("https://jsonplaceholder.typicode.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()
val service = retrofit.create(APIService::class.java)
```
---

### APIService
```kotlin
interface APIService {
    @GET("posts")
    suspend fun getPosts(): List<Post>
}
```
---

### В Activity
```kotlin
val retrofit = Retrofit.Builder() // Base URL (для примера)
    .baseUrl("https://jsonplaceholder.typicode.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()
val service = retrofit.create(APIService::class.java)

val coroutineExceptionHandler = CoroutineExceptionHandler{_, throwable ->
    throwable.printStackTrace()
}

button.setOnClickListener {
    lifecycleScope.launch(Dispatchers.IO + coroutineExceptionHandler) {
        val posts = service.getPosts()
        withContext(Dispatchers.Main) {
            textView.text = posts.joinToString(" ")
        }
    }
}
```
---

---
### Полезные ссылки
* https://square.github.io/retrofit/
* http://developer.alexanderklimov.ru/android/library/retrofit.php
* https://habr.com/ru/post/520544/
* https://habr.com/ru/post/314028/
* https://medium.com/swlh/simplest-post-request-on-android-kotlin-using-retrofit-e0a9db81f11a
* https://johncodeos.com/how-to-make-post-get-put-and-delete-requests-with-retrofit-using-kotlin/
