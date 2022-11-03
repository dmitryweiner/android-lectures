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
    implementation 'com.squareup.retrofit2:retrofit:2.5.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.3.0'
}
```
---

### Инициализация
```kotlin
Retrofit retrofit = Retrofit.Builder()
    // Base URL (для примера)
    .baseUrl("https://jsonplaceholder.typicode.com/") 
    .addConverterFactory(GsonConverterFactory.create())
    .build();
service = retrofit.create(APIService.class);
```
---

### APIService
```kotlin
public interface APIService {
    @GET("posts")
    Call<List<Post>> getPosts();
}
```
---


Retrofit
public class RetrofitClient {
    static ApiService getService(){
        OkHttpClient.Builder httpClient = new OkHttpClient.Builder();
        Retrofit.Builder builder = new Retrofit.Builder()
                .baseUrl("http://127.0.0.1:5000/")
                .addConverterFactory(GsonConverterFactory.create());

        Retrofit retrofit = builder
                .client(httpClient.build())
                .build();

        return retrofit.create(ApiService.class);
    }
}
RetrofitClient.getService().fetchData(jsonObject).enqueue(new Callback<ResponseClass>() {
    @Override
    public void onResponse(Call<ResponseClass> call, Response<ResponseClass> response) {

    }

    @Override
    public void onFailure(Call<ResponseClass> call, Throwable t) {

    }
});

Moshi - JSON

Chuck - inspector

---
### Полезные ссылки
* https://square.github.io/retrofit/
* http://developer.alexanderklimov.ru/android/library/retrofit.php
* https://habr.com/ru/post/520544/
* https://medium.com/swlh/simplest-post-request-on-android-kotlin-using-retrofit-e0a9db81f11a
* https://johncodeos.com/how-to-make-post-get-put-and-delete-requests-with-retrofit-using-kotlin/
