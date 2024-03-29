### Разметка, работа с компонентами

![Andoroid layout](assets/layout/constraint.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео](https://youtu.be/t4NZ9OQ3epo)
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

### Система позиционирования в Constraint Layout

![](assets/layout/cl1.png) ![](assets/layout/cl2.png)
---

### Единицы измерения в Android
* Можно указывать размер текста и различные расстояния в следующих единицах:
  * px - пиксели экрана.
  * in - дюймы.
  * mm - миллиметры.
  * pt - линии (1/72 дюйма).
  * dp - независящие от плотности экрана пиксели.
  * sp - масштабируемые dp (в зависимости от выбранного размера шрифта).
* [Подробнее](https://material.io/design/layout/pixel-density.html#pixel-density-on-android),
  [ещё](https://stackoverflow.com/questions/2025282/what-is-the-difference-between-px-dip-dp-and-sp).
---

### Старайтесь указывать размер в dp
* Размер экранных элементов надо указывать в dp.
* Размер текста в полях и лейблах указывать в sp.

![](assets/layout/density.png)
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

#### [Палитра компонентов](https://developer.alexanderklimov.ru/android/views/)

![](assets/layout/views3.png) ![](assets/layout/views1.png) ![](assets/layout/views2.png)
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

> У рыб нет зуб
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
        button.setOnClickListener {
            // так можно менять текст кнопки 
            button.text = "Ура, на меня нажали!"
        }
    }
}
```
<button>Ура, на меня нажали!</button>
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
<EditText
   android:id="@+id/editText"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:ems="10"
   android:inputType="textPersonName"
   android:text="Тут какой-то введённый текст"/>
```

```kotlin
val editText = findViewById<EditText>(R.id.editText)
button.setOnClickListener {

   // прочитать введённый текст
   val text = editText.text.toString()
   
   // записать строку  в поле
   editText.setText(text.reversed())
}
```

<input type="text" value="Тут какой-то введённый текст">
---
### Типизированные поля ввода
* Можно использовать поля ввода, автоматически фильтрующие текст:
    * Число.
    * Email.
    * Телефон.
    * Дата.
* [Варианты](https://developer.alexanderklimov.ru/android/views/edittext.php).

![](assets/layout/views2.png)

---
 
### Подписка на изменения поля
* Можно подписаться на изменения EditText, если нужно реагировать на ввод в реальном времени.
* [Подробнее](https://developer.android.com/reference/android/text/TextWatcher).

```kotlin
val editText = findViewById<EditText>(R.id.editText)
editText.doAfterTextChanged {
  // в it.toString() лежит введённая строка
}
```
---

### Подписка на изменения EditText: старый способ
* Если предыдущий способ не работает, следует использовать `TextWatcher`:

```kotlin
editText.addTextChangedListener(object : TextWatcher {
 // после того, как текст редактировали
 override fun afterTextChanged(s: Editable) {   
  // в s.toString() введённая строка 
 }
 // перед тем, как текст редактировали
 override fun beforeTextChanged(s: CharSequence, start: Int, count: Int, after: Int) {
 }
 // во время ввода, изменение строки уже произошло
 override fun onTextChanged(s: CharSequence, start: Int, before: Int, count: Int) {
 }
})
```
---
 
### Переключатель
![](assets/layout/switch.png)
```xml
<Switch
   android:id="@+id/switch"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="Switch"/>
```

```kotlin
val switch = findViewById<Switch>(R.id.switch)
// текущее состояние
switch.isChecked
// реакция на переключение
switch.setOnCheckedChangeListener { 
   compoundButton, b ->
     // в b лежит новое состояние чекбокса 
     editText.setText(if (b) "Включено" else "Выключено")
}
```
---
 
### Радио-кнопки
<label><input name="filter" type="radio" value="Все" checked="">Все</label><label><input name="filter" type="radio" value="Сделанные">Четные</label><label><input name="filter" type="radio" value="Не сделанные">Нечетные</label>

```xml
<RadioGroup
   android:id="@+id/radioGroup"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:background="#333333"
   android:orientation="horizontal">
   <RadioButton
   android:id="@+id/radio1"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:layout_marginLeft="24dp"
   android:layout_weight="1" />
   <RadioButton
   android:id="@+id/radio2"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:layout_marginLeft="24dp"
   android:layout_weight="1" />
</RadioGroup>
```
---

### Радио-кнопки
[Подробнее](https://developer.alexanderklimov.ru/android/views/radiobutton.php)

```kotlin
val radioButton = findViewById<RadioButton>(R.id.radio1)
// текущее состояние
radioButton.isChecked

// реакция на события
val radioGroup = findViewById<RadioGroup>(R.id.radioGroup)
radioGroup.setOnCheckedChangeListener { 
  radioGroup, i -> /* i -- id выбранного пункта */
  when(i) {
    R.id.radioButton1 -> // делаем что-то
    R.id.radioButton2 -> // делаем что-то другое
  }
}
```
---
 
### Выпадающий список
![](assets/layout/spinner.png)

```xml
<Spinner
   android:id="@+id/spinner"
   android:layout_width="0dp"
   android:layout_height="wrap_content" />
```

```kotlin
val spinner = findViewById<Spinner>(R.id.spinner)
val options = arrayOf("камень", "ножницы", "бумага")
val adapter = ArrayAdapter(this, android.R.layout.simple_spinner_item, options)
spinner.adapter = adapter
```
---

### Выпадающий список
* Как прочитать значение:

```kotlin
// текущая выбранная опция
spinner.selectedItemPosition

// реакция на выбор
spinner.onItemSelectedListener = object : AdapterView.OnItemSelectedListener {
    override fun onNothingSelected(parent: AdapterView<*>?) {
      // ничего не выбрали
    }
    override fun onItemSelected(
        parent: AdapterView<*>?,
        view: View?,
        position: Int,
        id: Long
    ) {
        // выбранная позиция position
    }
}
```
* [Как настроить его вид и заполнить список](https://developer.alexanderklimov.ru/android/views/spinner.php).
---

#### Список, берущий значения из файла ресурсов
 
* Строки лежат в файле `res/values/strings.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="planets">
        <item>Меркурий</item>
        <item>Венера</item>
        <item>Земля</item>
        <item>Марс</item>
    </string-array>
</resources>
```

* В лейауте:

```xml
<Spinner android:entries="@array/planets" />
```
---

### Ползунок
<input type="range">

```xml
<SeekBar
   android:id="@+id/seekBar"
   android:layout_width="0dp"
   android:layout_height="wrap_content" />
```

```kotlin
val seekBar = findViewById<SeekBar>(R.id.seekBar)

// можно прочитать текущее значение SeekBar
val n = seekBar.progress
```

[Подробнее](https://developer.alexanderklimov.ru/android/views/seekbar.php)
---

### Ползунок: реакция на изменение

```kotlin
val seekBar = findViewById<SeekBar>(R.id.seekBar)

// реакция на изменение
seekBar.setOnSeekBarChangeListener(object : SeekBar.OnSeekBarChangeListener{
    override fun onProgressChanged(seekBar: SeekBar?, progress: Int, fromUser: Boolean) {
        // в progress новое значение
    }
    override fun onStartTrackingTouch(seekBar: SeekBar?) {    }
    override fun onStopTrackingTouch(seekBar: SeekBar?) {     }
})
```
---

### Задачи
* Написать кликер с <button>+</button> и <button>-</button>. Сделать так, чтобы он не заходил в отрицательные числа.

![](assets/layout/clicker.png)
  
        // 1. Счетчик не должен уходить
        // в минус

        // 2. при нажатии на [-] если значение 0 показывать Toast
        // "в минус уходить нельзя"

        // 3. Сделать текстовое числовое поле, где лежит, на сколько
        // прибавлять или убавлять основное значение
---

### Задачи
* Сделать калькулятор:

<br/>
<input type="number" value="2">
<select>
  <option>+</option>
  <option>-</option>
  <option>*</option>
  <option>/</option>
</select>
<input type="number" value="2">
= 4

     // калькулятор обновляется автоматически

* Калькулятор систем счисления:

<input type="number" value="15"> в 10-чной системе.

<input type="number" value="1111"> в 
<select>
  <option>2</option>
  <option>8</option>
  <option>16</option>
  <option>36</option>
</select>
системе.
---

* Список чисел с фильтрацией:
  <br/><input><button>+</button><br/>
  <label><input name="filter" type="radio" value="Все" checked="">Все</label><label><input name="filter" type="radio" value="Сделанные">Четные</label><label><input name="filter" type="radio" value="Не сделанные">Нечетные</label>
> 1, 13, 6, 52, 4, 14
>
При нажатии на [+] в список добавляется очередное число из поля ввода. При изменении состояния фильтра список обновляется.
---

### Задачи
https://developer.android.com/codelabs/basic-android-kotlin-training-xml-layouts
---

### Полезные ссылки
* https://developer.alexanderklimov.ru/android/layout/constraintlayout.php
* https://developer.alexanderklimov.ru/android/views/
