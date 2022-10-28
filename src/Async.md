## Асинхронный код

![multithreading](assets/async/multithread.jpg)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

### Потоки
* Создание:
```kotlin
thread {
  // тут код, выполняющийся в другом потоке
}
```
* Запуск:
```kotlin
val myThread = thread(start = false) {
  // ..
}
myThread.start()
```
* Остановка:
```kotlin
thread?.interrupt() // поток должен слушать InterruptedException
```
---

### Общение между потоками через Handler
[Подробнее](https://developer.alexanderklimov.ru/android/theory/handler.php)

```kotlin
class MainActivity : AppCompatActivity() {
    private var mHandler: Handler? = null
    var gameOn = false
    var startTime: Long = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        startTime = System.currentTimeMillis()
        mHandler = object : Handler(Looper.getMainLooper()) {
            override fun handleMessage(msg: Message) {
                super.handleMessage(msg)
                if (gameOn) {
                    val seconds = (System.currentTimeMillis() - startTime) / 1000
                    Log.i("info", "seconds = $seconds")
                }
                mHandler!!.sendEmptyMessageDelayed(0, 1000)
            }
        }

        gameOn = true
        (mHandler as Handler).sendEmptyMessage(0)
    }
}
```
---

### Обновление UI
* Нельзя трогать интерфейс не из главного UI-потока. 
* Для этого используется конструкция `runOnUiThread`:
```kotlin
runOnUiThread {
    textView.text = i.toString()
}
```
---

### Пример таймера с остановом
```kotlin
val textView = findViewById<TextView>(R.id.textView)

var thread: Thread? = null
var i = 0

val buttonStart = findViewById<Button>(R.id.buttonStart)
val buttonStop = findViewById<Button>(R.id.buttonStop)

buttonStart.setOnClickListener {
    thread = thread {
        while (true) {
            try {
                Thread.sleep(1000)
            } catch (e: InterruptedException) {
                // We've been interrupted: no more messages.
                return@thread
            }
            runOnUiThread {
                textView.text = i.toString()
            }
            i++
        }
    }
}

buttonStop.setOnClickListener {
    thread?.interrupt()
}
```
---

### Корутина
> Coroutine (корутины), или сопрограммы — это блоки кода, которые работают по очереди. В нужный момент исполнение такого блока приостанавливается с сохранением всех его свойств, чтобы запустился другой код. Когда управление возвращается к первому блоку, он продолжает работу. В результате программа выполняет несколько функций одновременно.
---

### Разница корутин и потоков
![](assets/async/concurr.jpeg)
---

### Плюсы и минусы корутин
* Плюсы:
  * Корутины легче потоков (тратят меньше ресурсов).
  * Асинхронный код пишется в линейном виде без callback hell.
* Минусы:
  * Выполнение идёт не параллельно, а последовательно (быстрее не будет).
  * Сложнее для программиста.
----
  
![](assets/async/kotlin_coroutines_1.png)
---

### Корутины: установка
* build.gradle (Module):

```
dependencies {
    // ...
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines:1.3.9'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.1'
    implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.5.1'
}
```
---

### Scope, context, dispatcher

![](assets/async/structure.png)
---

### Dispatcher

![](assets/async/dispatcher.png)
---

### Работа с корутинами
* Запуск:

```kotlin
launch {
  // тут асинхронный код
}
```
---

### Suspend функции

```kotlin
suspend fun doDelay() {
  delay(1000)
}
```
---

### Таймер с остановом на корутинах

```kotlin
val textView = findViewById<TextView>(R.id.textView)
val buttonStart = findViewById<Button>(R.id.buttonStart)
val buttonStop = findViewById<Button>(R.id.buttonStop)

buttonStart.setOnClickListener {
    isRunning = true
    lifecycleScope.launch {
        var i = 0
        while (isRunning) {
            delay(1000)
            i++
        }
    }
}
buttonStop.setOnClickListener {
    isRunning = false
}
```
---

### Задачи
* Сделать обратный таймер с кнопками "старт" и "стоп". При нажатии на "старт" идёт от 10 до 0 и останавливается.
При нажатии на "стоп" просто останавливается, можно возобновить подсчёт с помощью "старт".
---

### Полезные ссылки
* https://metanit.com/kotlin/tutorial/8.1.php
* https://kotlinlang.org/docs/coroutines-guide.html
* https://developer.android.com/kotlin/coroutines
* https://developer.android.com/topic/libraries/architecture/coroutines
* https://itzone.com.vn/en/article/kotlin-coroutines-in-android/

---

<iframe width="560" height="315" src="https://www.youtube.com/embed/b4mBmi1QNF0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/er8dPeR8v3A" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
