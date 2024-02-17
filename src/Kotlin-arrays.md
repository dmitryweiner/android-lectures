### Массивы

![массивы](assets/arrays/array-explanation.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео](https://youtu.be/EpPdwcr5vJw)
---

* Массивы бывают:
  * Типизированные (один тип).
  * Массивы с элементами разного типа.

![Debug](assets/arrays/array-debug.png)
---

### Создание массива
* Через конструктор:

```kotlin
val arr = Array(3) { i -> i * 2 } // [2, 4, 6]
```

Первый параметр - размер. Второй параметр - лямбда для заполнения.
* Через `arrayOf`, в параметрах указываем элементы массива:

```kotlin
val arr = arrayOf(1, 2, 3)
```
---

### Массив и типы
* Типизированный:

```kotlin
val arr = arrayOf<Int>(1, 2, 3)
```

* Можно создавать методами `intArrayOf(), charArrayOf(), booleanArrayOf(), longArrayOf(), shortArrayOf(), byteArrayOf().`
* Смешанный:

```kotlin
val myArray = arrayOf(1, 2, 3, 4, 5, "зайчик", "вышел", "погулять")
```
---

### Массив как аргумент функции
* Массив можно указывать как тип переменной или аргумента:
```kotlin
fun calc(arr: Array<Int>) {
    /* ... */
}
```
---

### `Array<Int>` или `IntArray`, какая разница?
![](assets/kotlin-collections/type-mismatch.png)

Почему нельзя выполнить такое присваивание, ведь типы казалось бы одинаковые?
```kotlin
var arr = intArrayOf(0, 1, 2) // 0, 1, 2
val boxedArr = Array<Int>(3) { it } //  // 0, 1, 2
arr = boxedArr // ❌ Error!
```
---

### 📦Вoxing при создании массива
* `intArrayOf` - это массив с примитивами, это аналог `int[]`.
* `Array<Int>` - это массив с boxed-элементами `Integer[]`.
  * Каждое значение завёрнуто во встроенный объект `Integer`.

![boxed values in array](assets/arrays/boxed.png)
---

### Boxed array -> typed array
Так можно преобразовать массив:
```kotlin
var arr = intArrayOf(0, 1, 2)
val boxedArr = Array<Int>(3) { it }
arr = boxedArr.toIntArray() // ✅
```
---

### Размер массива
* Хранится в свойстве `.size`:
```kotlin
val arr = arrayOf(1, 2, 3)
println(arr.size) // 3
```
---

### Проверка, есть ли такой элемент
* Используется оператор `in`:
```kotlin
val arr = arrayOf(1, 2, 3)
println(2 in arr)      // true
println(4 in arr)      // false
println(4 !in arr)     // true
```
---

### Перебор массива
* Перебор элементов с помощью `for`:

```kotlin
val arr = arrayOf<Int>(1, 2, 3)
for(item in arr) {
  println(item)
}
```

* Перебор с помощью `.forEach`:

```kotlin
val arr = arrayOf<Int>(1, 2, 3)
arr.forEach { println(it) }
```
---

### Перебор индексов
```kotlin
val arr = arrayOf<Int>(1, 2, 3)

// через for
// Внимание! Используем свойство arr.indices
for(index in arr.indices) {
  println(index)
}

// через .forEach
arr.indices.forEach { println(it) }
```
---

### Различия onEach и forEach

![](assets/kotlin-collections/forEach.png)
---

### Трансформация с помощью map
* `map` применяет лямбду к каждому элементу массива и возвращает новый массив:

```kotlin
val arr = Array(5) { it }
// 0, 1, 2, 3, 4
val arr2 = arr.map { it * it } // возвести в квадрат
// 0, 1, 4, 9, 16
val arr3 = arr.map { it.toString() } // преобразовать в строки
// "0", "1", "2", "3", "4"
```
---

### Сортировка
* sort:

```kotlin
val numbers: IntArray = intArrayOf(7, 5, 8, 4, 9, 6, 1, 3, 2)
numbers.sort()
// 1, 2, 3, 4, 5, 6, 7, 8, 9
```
* Сортировка с компаратором sortWith:

```kotlin
data class User(val name: String, val age: Int)
val users = arrayOf(User("Вася", 19), User("Катя", 18), User("Петя", 20), User("Маша", 17))
users.sortWith(Comparator { u1, u2 -> u1.age - u2.age })
// User(name=Маша, age=17), User(name=Катя, age=18), User(name=Вася, age=19), User(name=Петя, age=20)
```

* Сорировка по полю объекта:
```kotlin
users.sortBy { it.age }
```
---

### Фильтрация
```kotlin
val numbers: IntArray = intArrayOf(7, 5, 8, 4, 9, 6, 1, 3, 2)

// все чётные
val filteredNumbers = numbers.filter { it % 2 == 0 }
// 8, 4, 6, 2

// все нечётные
val filteredNumbers2 = numbers.filter { it % 2 != 0 }
// 7, 5, 9, 1, 3
```
---

### Поиск
* find:
```kotlin
val numbers: IntArray = intArrayOf(7, 5, 8, 4, 9, 6, 1, 3, 2)
numbers.find { it % 2 == 0 } // найти первый четный
// 8
```
* findLast:
```kotlin
val numbers: IntArray = intArrayOf(7, 5, 8, 4, 9, 6, 1, 3, 2)
numbers.findLast { it % 2 == 0 } // найти последний четный
// 2
```
---

### Различия в окончаниях
* Методы, оканчивающиеся на _-ed_ возвращают новый массив, не изменяя исходный.
* Аналогичные методы без _-ed_ меняют сам массив.

```kotlin
val numbers: IntArray = intArrayOf(7, 5, 8, 4, 9, 6, 1, 3, 2)
numbers.sorted() // возвращает 1, 2, 3, 4, 5, 6, 7, 8, 9
numbers // тут лежит 7, 5, 8, 4, 9, 6, 1, 3, 2
numbers.sort() // ничего не возвращает
numbers // теперь тут лежит 1, 2, 3, 4, 5, 6, 7, 8, 9
```
---

### Задачи
#### Задачи необходимо решать в виде функции, принимающей на вход массив, возвращающей ответ
* Найти максимальную разницу между элементами массива.
```kotlin
f(arrayOf(-7, 12, 4, 6, -4, -12, 0)) // 24 
f(arrayOf()) // 0 
```
---

* Найти, сколько есть в массиве пар чисел, дающих в сумме 0.
Число из пары не может участвовать в других парах.
```kotlin
f(arrayOf(-7, 12, 4, 6, -4, -12, 0)) // 2 
f(arrayOf(-1, 2, 4, 7, -4, 1, -2)) // 3
f(arrayOf(-1, 1, 0, 1)) // 1
f(arrayOf(-1, 1, -1, 1)) // 2
f(arrayOf(1, 1, 1, 0, -1)) // 1
f(arrayOf(0, 0)) // 1 
f(arrayOf()) // 0 
```
---

* Реализовать функцию, которой на вход подаётся массив целых чисел со знаком,
   функция возвращает, сколько раз сменился знак.
```kotlin
signCount(arrayOf(1, 0, -1, -3, -5, 5, -1)) === 3
signCount(arrayOf(1, 0, 1, 3, 5, 5, 1)) === 0
signCount(arrayOf(-11, 1, 3, 5, 1)) === 1
```
---

* Написать функцию, возвращающую самый часто встречающийся символ в переданной строке:

```kotlin
mostCommon("isefe5i35fiuo34iuq") // 'i'
```

* Написать функцию, принимающую на вход строку, состоящую из слов, разделённых пробелами,
    и возвращающую самое редко встречающееся слово:

```kotlin
mostRare("ток кот кукож ток кот") // "кукож"
mostRare(
  "кот ток кот кукож ток кот ток кукож"
) // "кукож"
```
---

### Задачи
https://developer.android.com/codelabs/basic-android-kotlin-training-collections
---

### Полезные ссылки
* https://developer.alexanderklimov.ru/android/kotlin/array.php
* https://metanit.com/kotlin/tutorial/2.3.php
