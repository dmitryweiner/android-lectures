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
* Это очень печалит.
* Чтобы так не было, необходимо отслеживать подобные события и сохранять текущее состояние активити в bundle.
* Для этого применяются 2 метода:
  * onSaveInstanceState - вызывается системой, когда приложение готово свернуться.
  * onRestoreInstanceState - когда приложение разворачивается.

---
### Восстановление состояния из bundle
```kotlin
val editTextKey = "editText"
override fun onSaveInstanceState(outState: Bundle) {
    val editText = findViewById<EditText>(R.id.editText)
    outState.putString(editTextKey, editText.text.toString())
    super.onSaveInstanceState(outState)
}

override fun onRestoreInstanceState(savedState: Bundle) {
    super.onRestoreInstanceState(savedState)
    val editText = findViewById<EditText>(R.id.editText)
    editText.setText(savedState.getString(editTextKey))
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
### Переход на другие активити
```kotlin
val intent = Intent(this, SecondActivity::class)
startActivity(intent)
```
---
### Передача параметров

---
### Приём параметров в вызванной активити
---
### Задачи
* Список чисел с просмотром числа:
<br/><input><button>+</button><br/>
<ul style="width: 200px; height: 150px; overflow-y: scroll"><li>1</li><li>3 ➡</li><li>15 ➡</li><li>22 ➡</li><li>14 ➡</li><li>47 ➡</li><li>2 ➡</li></ul>
При нажатии на [+] в список добавляется очередное число из поля ввода.
При клике на [➡] на элементе списка происходит переход на другую активити, где отображается выбранное число.
---
### Полезные ссылки
* https://developer.alexanderklimov.ru/android/theory/lifecycle.php
* https://metanit.com/java/android/2.1.php
* https://www.digitalocean.com/community/tutorials/android-intent-handling-between-activities-using-kotlin
* <iframe width="560" height="315" src="https://www.youtube.com/embed/njmOeFadDEI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>