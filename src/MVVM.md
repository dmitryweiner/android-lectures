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

### Установка библиотеки для работы с ViewModel
* build.gradle (module): 
```
dependencies {
    // ...
    implementation 'androidx.activity:activity-ktx:1.6.1'
```
---

### ViewModel
```kotlin
data class UiState(
    var value: Int = 0
)
class MainViewModel : ViewModel() {

    private val uiState = UiState()

    // изменение значения
    fun incrementValue() {
        uiState.value++
    }
    
    // получение значения
    fun getValue(): Int {
        return uiState.value
    }
}

// in Activity
val viewModel: MainViewModel by viewModels()
// изменение данных
viewModel.incrementValue()

// получение данных
viewModel.getValue()
```
---

### LiveData
> LiveData — это контейнер, который следит за жизненным циклом экрана и снабжает его данными, когда это уместно. Например, если активити находится на переднем плане и видна пользователю. Ценность такого подхода заключается в том, что LiveData не будет снабжать ваш экран данными, если он свёрнут или закрыт. Но как только экран появится перед пользователем, обновление данных возобновится.
---

### LiveData
* Класс, реализующий шину событий.
* Activity подписывается на изменение данных и получает всегда свежее значение.
* Изменение данных делается методами:
    * setValue - из основного потока.
    * postValue - из всех остальных потоков
---
```kotlin
data class UiState(
    var value: Int = 0
)
class MainViewModel : ViewModel() {

    private val uiState = MutableStateFlow<UiState>(UiState())

    // изменение значения
    fun incrementValue() {
        uiState.value++
    }
    
    // получение значения
    fun getValue(): Int {
        return uiState.value
    }
}

// in Activity
val viewModel: MainViewModel by viewModels()
// изменение данных
viewModel.incrementValue()

// получение данных
viewModel.getValue()
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
* http://www.fandroid.info/lektsiya-8-po-arhitekture-android-data-binding-mvvm/
* https://www.howtodoandroid.com/mvvm-retrofit-recyclerview-kotlin/
* https://proandroiddev.com/clean-architecture-on-android-using-feature-modules-mvvm-view-slices-and-kotlin-e9ed18e64d83
* https://dev.to/whatminjacodes/simple-example-of-mvvm-architecture-in-kotlin-4j5b
* https://developer.android.com/topic/libraries/architecture/livedata
* https://github.com/velmurugan-murugesan/Android-Example/tree/master/MvvmRetrofitRecyclerviewKotlin
