### Activity

![Activity](assets/activity/logo.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

### Жизненный цикл
![lifecycle](assets/activity/lifecycle.png)
---

### Жизненный цикл
* onCreate - активити создана и готовится показаться.
* onStart - на экране, пока не обрабатывает события пользователя.
* onResume - обрабатывает события пользователя.
* onPause - ещё на экране, не обрабатывает события.
* onStop - остановлена, не видна.
* onDestroy - приложение закрыто.
---

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedState: Bundle?) {
        Log.d(getString(R.string.app_name), "onCreate")
        super.onCreate(savedState)
        setContentView(R.layout.activity_main)
    }

    override fun onStart() {
        super.onStart()
        Log.d(getString(R.string.app_name), "onStart")
    }
    override fun onPause() {
        super.onPause()
        Log.d(getString(R.string.app_name), "onPause")
    }
    override fun onResume() {
        super.onResume()
        Log.d(getString(R.string.app_name), "onResume")
    }
    override fun onStop() {
        super.onStop()
        Log.d(getString(R.string.app_name), "onStop")
    }
    override fun onDestroy() {
        super.onDestroy()
        Log.d(getString(R.string.app_name), "onDestroy")
    }
}
```
---

### Сохранение состояния в bundle
* При повороте устройства, закрытии приложения все введённые пользователем данные пропадают.
* Чтобы так не было, необходимо сохранять текущее состояние активити в bundle.
* Для этого применяются 2 метода:
  * `onSaveInstanceState` - вызывается системой, когда приложение готово свернуться.
  * `onCreate(savedInstanceState: Bundle?)` - когда приложение разворачивается.

---
### Сохранение состояния в bundle
```kotlin
val EDIT_TEXT_KEY = "EDIT_TEXT_KEY"

override fun onSaveInstanceState(outState: Bundle) {
    val editText = findViewById<EditText>(R.id.editText)
    outState.putString(EDIT_TEXT_KEY, editText.text.toString())
    super.onSaveInstanceState(outState)
}
```
---

### Как это работает:
* Сохраняется пара ключ-значение:

```kotlin
outState.putString(
  "ключ", // любой текстовый ключ
  "значение"
)
```
* Восстанавливается значение по ключу:

```kotlin
сохранённое значение = savedState.getString("ключ")
```
---

### Тип сохраняемого значения
* Сохранять можно не только строки, но и другие разнообразные типы:
  * putBoolean - getBoolean
  * putInt - getInt
  * putIntArray - getIntArray
  * и т.д.
---

### Восстановление состояния из bundle
```kotlin
val EDIT_TEXT_KEY = "EDIT_TEXT_KEY"

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    
    if (savedInstanceState != null) {
        val editText = findViewById<EditText>(R.id.editText)
        editText.setText(savedInstanceState.getString(EDIT_TEXT_KEY))
    }
}
```
---
### Сохраняем в State
* Если параметров больше 2, разумнее завести под это отдельный класс и сохранять в него.
* Создаём Serializable класс внутри Activity:
```kotlin
lateinit var state: State
class State(
    var text: String, // эти значения для примера
    var counter: Int, // у вас могут быть любые
    var flag: Boolean
): Serializable
```
---

### Сохранение данных стейта
```kotlin
val STATE_KEY = "STATE_KEY"

override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    
    val editText = findViewById<EditText>(R.id.editText)
    
    // сохраняем текущие значения в state
    state.text = editText.text
    state.counter = // ...
    
    outState.putSerializable(STATE_KEY, state)
}
```
---

### Восстановление данных стейта
```kotlin
val STATE_KEY = "STATE_KEY"

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    state = if (savedInstanceState != null)
        // если есть сохранённое состояние, восстанавливаем его
        savedInstanceState.getSerializable(STATE_KEY) as State
    else
        // иначе устанавливаем дефолтные значения
        State(
            text = "",
            counter = 0,
            flag = false
        )
    renderState()
}

fun renderState() {
    val editText = findViewById<EditText>(R.id.editText)
    editText.setText(state.text)
}
```
---

### Всё в сборе
```kotlin
class MainActivity : AppCompatActivity() {
    val STATE_KEY = "STATE_KEY"

    lateinit var state: State
    class State(
        var text: String,
        var counter: Int,
        var flag: Boolean
    ): Serializable

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        state = if (savedInstanceState != null)
            savedInstanceState.getSerializable(STATE_KEY) as State
        else
            State(
                text = "",
                counter = 0,
                flag = false
            )

        val buttonPlus = findViewById<Button>(R.id.buttonPlus)
        buttonPlus.setOnClickListener {
            state.counter += 1
            renderState()
        }
        val buttonMinus = findViewById<Button>(R.id.buttonMinus)
        buttonMinus.setOnClickListener {
            state.counter -= 1
            renderState()
        }

        renderState()
    }

    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putSerializable(STATE_KEY, state)
    }

    fun renderState() {
        val textView = findViewById<TextView>(R.id.textViewCounter)
        textView.text = state.counter.toString()
    }
}
```
---

### Переход на другую активити
* Для начала надо создать другую активити.
* [Подробнее](https://www.fandroid.info/urok-5-kotlin-dobavlenie-vtorogo-ekrana-v-android-prilozhenie/).

![](assets/activity/img.png)

---
Имя можно выбрать исходя из назначения активити:

![](assets/activity/img_1.png)
---
### Переход на другую активити
```kotlin
val button = findViewById<Button>(R.id.button)
button.setOnClickListener {
    val intent = Intent(this, SecondActivity::class.java)
    startActivity(intent)
}
```
---
### Передача параметров
```kotlin
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("key", value)
startActivity(intent)
```
---
### Приём параметров в вызванной активити
[Подробнее](https://startandroid.ru/ru/uroki/vse-uroki-spiskom/67-urok-28-extras-peredaem-dannye-s-pomoschju-intent.html)

```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_second)

        val textView = findViewById<TextView>(R.id.textView2)
        val intent = getIntent()
        val value = intent.getStringExtra("key")
        textView.text = value
    }
}
```
---
### Задачи
* Список чисел [RecyclerView](https://dmitryweiner.github.io/android-lectures/Recycler-view.html#/) с возможностью добавления числа:
<br/><input><button>+</button><br/>
<ul style="width: 200px; height: 150px; overflow-y: scroll"><li>1 <button>➡</button></li><li>3 <button>➡</button></li><li>15 <button>➡</button></li><li>22 <button>➡</button></li><li>14 <button>➡</button></li><li>47 <button>➡</button></li><li>2 <button>➡</button></li></ul>
При нажатии на [+] в список добавляется очередное число из поля ввода.
При клике на [➡] на элементе списка происходит переход на другую активити, где отображается выбранное число.
---

### Полезные ссылки
* https://developer.alexanderklimov.ru/android/theory/lifecycle.php
* https://metanit.com/java/android/2.1.php
* https://www.digitalocean.com/community/tutorials/android-intent-handling-between-activities-using-kotlin
* https://www.fandroid.info/urok-5-kotlin-dobavlenie-vtorogo-ekrana-v-android-prilozhenie/
* https://startandroid.ru/ru/uroki/vse-uroki-spiskom/67-urok-28-extras-peredaem-dannye-s-pomoschju-intent.html
---

### Полезные ссылки
<iframe width="560" height="315" src="https://www.youtube.com/embed/njmOeFadDEI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
---

### Полезные ссылки
<iframe width="560" height="315" src="https://www.youtube.com/embed/puj9OXs2iPM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>