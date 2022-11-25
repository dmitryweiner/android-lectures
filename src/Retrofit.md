# `Retrofit`
### Библиотека для обращения в API

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

### Retrofit
* Библиотека для удобного обращения к REST API.
* Поддерживает асинхронность (корутины) с версии 2.6.
* [Документация](https://square.github.io/retrofit/).
* [Руководство](http://developer.alexanderklimov.ru/android/library/retrofit.php).
---

### Перепишем задачу из [предыдущей лекции](https://dmitryweiner.github.io/android-lectures/Network.html#/) на Retrofit
* Написать приложение, отображающее форму:
  <br/><label>
  ID:
  <input><br/>
  </label>
  <button>Получить данные!</button>
* При нажатии кнопки приложение обращается в API:
  https://jsonplaceholder.typicode.com/posts/:id
* Полученные результаты показываются на экране. Поля `title` и `body`:
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
* Корутины отдельно ставить не надо, только `lifecycle-runtime` для 
[lifecycle-контекста](https://developer.android.com/topic/libraries/architecture/coroutines) корутин.
---

### Инициализация
```kotlin
val retrofit = Retrofit.Builder() 
    // Base URL (для примера)
    .baseUrl("https://jsonplaceholder.typicode.com/")
    // Подключаем конвертер JSON'ов
    .addConverterFactory(GsonConverterFactory.create())
    .build()
val service = retrofit.create(APIServiceInterface::class.java)
```
---

### APIServiceInterface
* Необходимо создать интерфейс, описывающий запросы к API:

```kotlin
interface APIServiceInterface {

    // соответствует запросу 
    // https://jsonplaceholder.typicode.com/posts
    @GET("posts")
    suspend fun getPosts(): List<Post>
        
    // https://jsonplaceholder.typicode.com/posts/1
    @GET("posts/{id}")
    suspend fun getPostById(@Path("id") id: Int): Post?
}
```
* Класс `Post` тот же, что и в [предыдущей лекции](https://dmitryweiner.github.io/android-lectures/Network.html#/9).
---

### Класс APIService
* Создадим класс APIService, реализующий шаблон [Singleton](https://ru.wikipedia.org/wiki/%D0%9E%D0%B4%D0%B8%D0%BD%D0%BE%D1%87%D0%BA%D0%B0_(%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)).
* В нём будет лежать экземпляр класса для доступа к API.
* И ссылка на API (base URL).
---

### Класс APIService
```kotlin
class APIService {
    companion object {
        const val BASE_URL = "https://jsonplaceholder.typicode.com/"

        @Volatile
        private var INSTANCE: APIServiceInterface? = null

        fun getService(): APIServiceInterface {
            if (INSTANCE == null) {
                synchronized(this) {
                    INSTANCE = buildService()
                }
            }
            return INSTANCE!!
        }

        private fun buildService(): APIServiceInterface {
            val retrofit = Retrofit.Builder()
                .baseUrl(BASE_URL)
                .addConverterFactory(GsonConverterFactory.create())
                .build()
            return retrofit.create(APIServiceInterface::class.java)
        }
    }
}
```
---
### Как пользоваться APIService?
* Для начала надо создать экземпляр класса:

```kotlin
val apiService = APIService.getService()
// или даже
val apiService: APIServiceInterface by lazy { APIService.getService() }
```
* Запросы:

```kotlin
// получение поста с ID = 1
// https://jsonplaceholder.typicode.com/posts/1
val post = apiService.getPostById(1)

// получение всех постов
// https://jsonplaceholder.typicode.com/posts
val posts = apiService.getPosts()
```
---

### Использование в Activity
```kotlin
class MainActivity : AppCompatActivity() {
    
    private val apiService = APIService.getService()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val button = findViewById<Button>(R.id.button)
        val textView = findViewById<TextView>(R.id.textView)
        val editText = findViewById<EditText>(R.id.editText)

        button.setOnClickListener {
            lifecycleScope.launch(Dispatchers.IO + coroutineExceptionHandler) {
                val post = apiService.getPostById(id)
                withContext(Dispatchers.Main) {
                    textView.text = post?.body
                }
            }        
        }
    }
}
```
---

### Варианты реализации запросов в APIServiceInterface (1)
* GET 	/posts:
```kotlin
@GET("posts")
suspend fun getPosts(): List<Post>
```
* GET 	/posts/1:
```kotlin
@GET("posts/{id}")
suspend fun getPostById(@Path("id") id: Int): Post?
```
---

### Варианты реализации запросов в APIServiceInterface (2)
* GET 	/posts/1/comments:
```kotlin
@GET("posts/{id}/comments")
suspend fun getCommentsByPostId(@Path("id") id: Int): List<Comment>
```
* GET 	/comments?postId=1:
```kotlin
@GET("сomments")
suspend fun getCommentsByPostId(@Query("postId") postId: Int): List<Comment>
```
---

### Варианты реализации запросов в APIServiceInterface (3)
* POST 	/posts:
```kotlin
@POST("posts")
suspend fun createPost(@Body post: Post): Post?
```
* PUT 	/posts/1:
```kotlin
@PUT("posts/{id}")
suspend fun updatePost(@Path("id") id: Int, @Body post: Post): Post?
```
---

### Отправка заголовков
```kotlin
@Headers({
    "Accept: application/json",
    "User-Agent: Retrofit-Sample-App"
})
@GET("users/{username}")
suspend fun getUser(@Path("username") username: String): User?
```
---

### Обработка ошибок
Обработчик:
```kotlin
fun <T> retrofitErrorHandler(res: Response<T>): T {
    if (res.isSuccessful) {
        return res.body()!!
    } else {
        val errMsg = res.errorBody()?.string()?.let {
            // обработка ошибочного ответа зависит от сервера
            JSONObject(it).getString("error") 
        } ?: run {
            res.code().toString()
        }
        throw Exception(errMsg)
    }
}
```
---

### Обработка ошибок
```kotlin
@POST("posts")
suspend fun createPost(@Body post: Post): Response<Post>
```

```kotlin
try {
  retrofitErrorHandler(apiService.createPost(newPost))
} catch (e: Exception) {
  Toast.makeText(applicationContext,
     "Error: ${e.message}",
     Toast.LENGTH_SHORT).show();
}
```
---

### Полезные ссылки
* [Репозиторий, где это уже сделано](https://github.com/dmitryweiner/kotlin-api/tree/main/kotlin-api-retrofit),
  [коммит](https://github.com/dmitryweiner/kotlin-api/commit/2be88c7a96beab8a0392d9a48830ca5510e86930).
* https://square.github.io/retrofit/
* http://developer.alexanderklimov.ru/android/library/retrofit.php
* https://habr.com/ru/post/520544/
* https://habr.com/ru/post/314028/
* https://medium.com/swlh/simplest-post-request-on-android-kotlin-using-retrofit-e0a9db81f11a
* https://johncodeos.com/how-to-make-post-get-put-and-delete-requests-with-retrofit-using-kotlin/
