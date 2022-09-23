### Разметка, работа с компонентами

![Andoroid layout](assets/layout/constraint.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/main/README.md)

[видео]()
---

### Иерархия компонентов

![](assets/layout/hierarchy.png)
---
### Виды ViewGroup
* Лейауты:
  * Constraint Layout - компоненты расположены в соответствии с правилами друг относительно друга.
  * Linear Layout - компоненты расположены линейно (горизонтально или вертикально).
  * Relative Layout - похоже на Constraint, устаревший.
  * [Подробнее](https://developer.android.com/develop/ui/views/layout/declaring-layout).
* Контейнеры:
  * RecyclerView - потенциально бесконечный список.
  * Spinner - <select><option>1</option><option>2</option><option>2</option></select>.
  * Fragment - переиспользуемый кусочек активити.
---

### Constraint Layout
* [Исчерпывающее руководство](https://developer.alexanderklimov.ru/android/layout/constraintlayout.php).
* Основная идея: для "резиновой" вёрстки позиционировать компоненты относительно родителя или соседей:

![](assets/layout/constraint-layout.png)

---

### Linear Layout
* [Руководство](http://developer.alexanderklimov.ru/android/layout/linearlayout.php).
* Идея: располагать элементы в строку либо в столбец.

![](assets/layout/linear-layout.png)
---

### Recycler View
* [Руководство](https://developer.alexanderklimov.ru/android/views/recyclerview-kot.php).
* Идея: переиспользовать элементы списка для отображения следующих элементов.

![](assets/layout/recyclerview21.gif)
---

### Палитра компонентов

![](assets/layout/views1.png) ![](assets/layout/views2.png) ![](assets/layout/views3.png)
---

### Текст

```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="TextView" />
```

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val textView = findViewById<TextView>(R.id.textView)
        textView.text = "У рыб нет зуб"
    }
}
```
---

### Кнопка
```xml
<Button
    android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Button" />
```

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val button = findViewById<Button>(R.id.button)
        button.setOnClickListener { button.text = "Ура, на меня нажали!" }
    }
}
```
---

### Как это работает?
* Функция `findViewById` ищет нужный View для навешивания обработчиков или изменения состояния.
* `setOnClickListener` навешивает обработчик на событие "клик" у найденной кнопки.
* Обработчик состоит из лямбды, изменяющей текст кнопки.
---

### Кликер
```xml
<TextView android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="TextView" />
<Button android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Button" />
```

```kotlin
class MainActivity : AppCompatActivity() {
    var i = 0
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val textView = findViewById<TextView>(R.id.textView)
        val button = findViewById<Button>(R.id.button)
        button.setOnClickListener {
            textView.text = i.toString()
            i++
        }
    }
}
```
---
 
### Поле ввода
```xml
<TextView android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="TextView" />
<Button android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Button" />
```

```kotlin
class MainActivity : AppCompatActivity() {
    var i = 0
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val textView = findViewById<TextView>(R.id.textView)
        val button = findViewById<Button>(R.id.button)
        button.setOnClickListener {
            textView.text = i.toString()
            i++
        }
    }
}
```
---
 
### Типизированные поля ввода

---
 
### Подписка на изменения поля

---
 
###

---
 
###

---
 
###

---
 
* Работа с компонентами.
    * Виды компонентов.
    * Поиск.
    * Binding.
    * Чтение введённых данных.
    * Реакция на события.
    * Тосты.
    * Диалоги.
---



### Полезные ссылки
* https://developer.alexanderklimov.ru/android/layout/constraintlayout.php
* https://developer.alexanderklimov.ru/android/views/recyclerview-kot.php