### Реализация архитектуры MVVM

![MVVM pattern](assets/mvvm/MVVMPattern.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

![MVVM architecture](assets/mvvm/mvvm-architecture.png)
---

### Идея класса ViewModel
* Activity умирает и возрождается в течение жизненного цикла.
* Состояние UI нужно хранить в классе, переживающем перезагрузку activity, - ViewModel.
* Данные должны быть отделены от представления:
    * Activity отображает данные и обрабатывает действия пользователя, передавая их во ViewModel.
    * ViewModel хранит данные, изменяет в соответствии с действиями пользователя и извещает activity, когда данные изменились.
---

![](assets/mvvm/viewmodel-lifecycle.png)
---

### Установка библиотеки для работы с ViewModel
* build.gradle (module): 
```
android {
    compileSdk 33 // <--

    defaultConfig {
        targetSdk 33 // <--
        // ...
    }
}
dependencies {
    // ...
    implementation 'androidx.activity:activity-ktx:1.6.1'
```
---

### ViewModel
```kotlin
class MainViewModel : ViewModel() {

    private var counter = 0

    fun incrementCounter() {
        counter++
    }

    fun getCounter(): Int {
        return counter
    }
}

// in Activity
val viewModel: MainViewModel by viewModels()
// изменение данных
viewModel.incrementCounter()

// получение данных
viewModel.getCounter()
```
---

### LiveData
> LiveData — это контейнер, который следит за жизненным циклом экрана и снабжает его данными, когда это уместно. Например, если активити находится на переднем плане и видна пользователю. Ценность такого подхода заключается в том, что LiveData не будет снабжать ваш экран данными, если он свёрнут или закрыт. Но как только экран появится перед пользователем, обновление данных возобновится.
---

### LiveData
* Класс, реализующий шину событий.
* Activity подписывается на изменение данных и получает всегда свежее значение.
* Изменение данных делается методами:
    * setValue(newValue) - из основного потока.
    * postValue(newValue) - из всех остальных потоков
---

```kotlin
class MainViewModel : ViewModel() {

    val counter =  MutableLiveData<Int>(
        0 // начальное значение
    )

    fun incrementCounter() {
        counter.value = counter.value!! + 1
    }
}

// in Activity
val viewModel: MainViewModel by viewModels()

// изменение данных
viewModel.incrementCounter()

// подписка на данные
viewModel.counter.observe(this, Observer {
   // в it лежит новое значение
   textView.text = it.toString()
})
```
----

```kotlin
class MainViewModel : ViewModel() {

    val counter =  MutableLiveData<Int>(
        0 // начальное значение
    )

    fun incrementCounter() {
        counter.postValue(counter.value!! + 1)
    }

    fun startTimer() {
        thread {
            while (true) {
                incrementCounter()
                Thread.sleep(500)
            }
        }
    }
}
```
---

### Хранение состояния в виде класса
* Если надо хранить несколько свойств интерфейса, удобно сложить их в класс. 
* Этот класс обычно называют state - состояние (интерфейса).

```kotlin
data class State(
    val counter: Int = 0,
    val isEven: Boolean = true,
)
```
* Тогда во ViewModel можно хранить экземпляр класса:

```kotlin
val state =  MutableLiveData<State>(
    State() // начальное значение
)
```
---

### Изменение полей состояния

```kotlin
```
---

### Идея репозитория

---

### Подключение Room и создание репозитория

---

### Подключение Retrofit и создание репозитория

---

### Подсоединение ViewModel напрямую к layout
```
<data>
    <variable
        name="user"
        type="com.project.model.User" />
</data>

...

<TextView
        android:id="@+id/firstname"
        android:text="@{user.firstname}"
        />
```
---

### Исправление ошибки с binding
```
> Task :app:checkDebugDuplicateClasses FAILED
Duplicate class androidx.lifecycle.ViewModelLazy found in modules lifecycle-viewmodel-2.5.1-runtime (androidx.lifecycle:lifecycle-viewmodel:2.5.1) and lifecycle-viewmodel-ktx-2.2.0-runtime (androidx.lifecycle:lifecycle-viewmodel-ktx:2.2.0)
```

* build.gradle (module): 
```
dependencies {
    // ...
    implementation "androidx.lifecycle:lifecycle-viewmodel:2.5.1"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.5.1"
}
```
---

### Полезные ссылки
* https://dev.to/whatminjacodes/simple-example-of-mvvm-architecture-in-kotlin-4j5b
* https://developer.android.com/topic/libraries/architecture/livedata
* https://www.howtodoandroid.com/mvvm-retrofit-recyclerview-kotlin/
* https://proandroiddev.com/clean-architecture-on-android-using-feature-modules-mvvm-view-slices-and-kotlin-e9ed18e64d83    
* [Binding](http://www.fandroid.info/lektsiya-8-po-arhitekture-android-data-binding-mvvm/)
* https://github.com/velmurugan-murugesan/Android-Example/tree/master/MvvmRetrofitRecyclerviewKotlin
* https://www.kodeco.com/10391019-livedata-tutorial-for-android-deep-dive
