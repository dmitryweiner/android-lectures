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

// in Activity:
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

### Трансформация
* Transformations.map получает на вход два параметра:
  * LiveData.
  * Преобразующую функцию.
* Когда меняется первый, Transformations.map выдаёт новое значение LiveData.
* Если преобразующая функция выдаёт LiveData, надо использовать Transformations.switchMap.
---

### Трансформация

```kotlin
class MainViewModel : ViewModel() {

    // тут хранится исходный список
    private val list = MutableLiveData<MutableList<Int>>(mutableListOf<Int>())
    
    // тут лежат только чётные значения,
    // обновляются автоматически
    val filteredList: LiveData<MutableList<Int>> = Transformations.map<MutableList<Int>, MutableList<Int>>(list) {
      return@map it.filter { number -> number % 2 == 0 }.toMutableList()
    }

    fun addElement(number: Int) {
        list.value?.add(number)
    }
}

// in Activity:
val viewModel: MainViewModel by viewModels()

viewModel.filteredList.observe(this, Observer {
   textView.text = it.joinToString(", ")
})
```
---

### Трансформация из нескольких источников
```kotlin
class MainViewModel : ViewModel() {

    // тут хранится исходный список
    private val list = MutableLiveData<MutableList<String>>(mutableListOf<String>())

    // тут хранится фильтрующее значение
    private val filter = MutableLiveData<String>("")

    // тут лежат только строки, которые содержат в себе строку filter
    val filteredList = Transformations.switchMap(list) { list ->
        Transformations.map(filter) { filter ->
            list.filter { it.contains(filter) }
        }
    }

    fun addElement(s: String) {
        list.value?.add(s)
        // эту строчку пришлось добавить, потому что иначе
        // LiveData не понимает, что изменения были
        list.postValue(list.value)
    }

    fun setFilter(s: String) {
        filter.value = s
    }
}

// in Activity:
val viewModel: MainViewModel by viewModels()

viewModel.filteredList.observe(this, Observer {
   textView.text = it.joinToString(", ")
})
```
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

### Идея репозитория

---

### Room и MVVM
https://www.geeksforgeeks.org/how-to-build-a-simple-note-android-app-using-mvvm-and-room-database/
https://github.com/umangburman/MVVM-Room-Kotlin-Example
https://github.com/agustiyann/ToDoList-Room-MVVM
---

### Retrofit и MVVM
https://www.howtodoandroid.com/mvvm-retrofit-recyclerview-kotlin/
https://github.com/velmurugan-murugesan/Android-Example/tree/master/MvvmRetrofitRecyclerviewKotlin
https://proandroiddev.com/clean-architecture-on-android-using-feature-modules-mvvm-view-slices-and-kotlin-e9ed18e64d83
---

### Полезные ссылки
* https://developer.android.com/topic/libraries/architecture/livedata
* https://www.kodeco.com/10391019-livedata-tutorial-for-android-deep-dive
* http://www.fandroid.info/lektsiya-8-po-arhitekture-android-data-binding-mvvm/
* https://dev.to/whatminjacodes/simple-example-of-mvvm-architecture-in-kotlin-4j5b
* https://www.howtodoandroid.com/mvvm-retrofit-recyclerview-kotlin/
* https://proandroiddev.com/clean-architecture-on-android-using-feature-modules-mvvm-view-slices-and-kotlin-e9ed18e64d83    
* https://github.com/velmurugan-murugesan/Android-Example/tree/master/MvvmRetrofitRecyclerviewKotlin

---

<iframe width="560" height="315" src="https://www.youtube.com/embed/bCH12ycXPeo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
