![Kotlin logo](assets/kotlin/logo.png)

## Основы

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео](https://youtu.be/2PzsRfnV108)
---

### История создания
* Дата создания: июль 2011.
* Автор: [Андрей Бреслав](https://twitter.com/abreslav).
* [Котлин](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D1%82%D0%BB%D0%B8%D0%BD) &mdash; остров в Финском заливе.
* [Документация](https://kotlinlang.org/docs/home.html).

![Андрей Бреслав](assets/kotlin/breslav.png)
---

### Основные особенности
* Реализует принципы [ООП](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5).
* Статически типизирован, умеет сам выводить типы (как TypeScript).
* Null-безопасный.
* Компилируется в байт-код. Исполняется на JVM.
* Полностью функционально совместим с Java (лёгкая миграция).
---

### Почему Kotlin, а не Java
* Лаконичность.
* Легче работать с типами.
* Легче создавать классы.

![kotlin-vs-java-meme](assets/kotlin/kotlin-vs-java-meme.png)
---

![kotlin-vs-java](assets/kotlin/kotlin-vs-java.png)
---

![kotlin-vs-java](assets/kotlin/kotlin-vs-java-2.png)
---

![kotlin-vs-java](assets/kotlin/kotlin-vs-java-3.png)
---

### Объявление переменных
* val для неизменных переменных:
```kotlin
val a = 1
```
* var для изменяемых переменных:
```kotlin
var i = 1
i++
```
* **Всегда** лучше предпочитать val для [иммутабельности данных](https://ru.wikipedia.org/wiki/%D0%9D%D0%B5%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D0%B9_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82).
---

### Область видимости
* Переменная не видна до того, как объявлена:
```kotlin
println(j) // Unresolved reference: j
val j = 1
```
* [Подробнее](https://kotlinlang.org/spec/scopes-and-identifiers.html).
---

### Область видимости
* Переменная видна внутри `{блока}`, в котором она объявлена:
```kotlin
if (i != 2) {
    val j = 1
    println(j)
}
println(j) // Unresolved reference: j
```
* Внутри `{блока}` видны переменные, объявленные снаружи:
```kotlin
val j = 1
if (j != 2) {
    println(j) // 1
}
```
---

### Типы данных
* Целые: Byte (8), Short (16), Int (32), Long (64).
* Дробные: Float (32), Double (64).
* Boolean.
* Char, String.
* Array.
* Object.
* [Подробнее](https://kotlinlang.org/docs/basic-types.html).
---

### Литералы типов

```kotlin
val i = 1 // Int
val l = 3000000000L // Long
val b: Byte = 1 // Byte

val f = 1.0f // Float
val d = 1.0 // Double

val c = 'c' // Char
val s = "str" // String

val bool = true // Boolean
```

[Подробнее](https://kotlinlang.org/docs/numbers.html)
---

### Автоматический вывод типа
* Если переменной при объявлении присваивается значение, тип указывать **не надо**:
```kotlin
val a = 1 // Int
val s = "123" // String
```
---

### Строки и шаблоны
* Char:
```kotlin
val ch = '1' // Char
```
* Строка:
```kotlin
val str = "123" // String
```
* Многострочная строка с переносами:
```kotlin
val text = """
    for (c in "foo")
    print(c)
"""
```
* Использование шаблонов в строках:
```kotlin
val i = 10
println("i = $i") // prints "i = 10"
```
---

### Строковые шаблоны
В шаблоне можно использовать не только переменные, но и операторы, вызывать методы и функции, однако не стоит увлекаться:
```kotlin
val i = 10
println("i = $i") // "i = 10"
println("i*i = ${i * i}") // "i*i = 100"

val s = "abc"
println("s = '$s', length = ${s.length}")
```
---

### Явная конвертация типов
```kotlin
val b: Byte = 1 // OK, literals are checked statically
// val i: Int = b // ERROR
val i1: Int = b.toInt()
```
* toByte(): Byte
* toShort(): Short
* toInt(): Int
* toLong(): Long
* toFloat(): Float
* toDouble(): Double
* toChar(): Char
---

### Проверка и приведение типа
* Проверка типа:
```kotlin
if (x is String) {
    print(x.length)
}
```

* Проверка сразу на несколько типов:
```kotlin
when (x) {
    is Int -> print(x + 1)
    is String -> print(x.length + 1)
    is IntArray -> print(x.sum())
}
```
---

### Приведение типа
* [Подробнее](https://kotlinlang.org/docs/typecasts.html).
* Приведение типа оператором `as`:
```kotlin
val x: String = y as String
```
* Такое приведение не безопасно, если в переменной лежит что-то, не совместимое с будущим типом (строка -> число). 
Если есть такие сомнения, надо пользоваться безопасным приведением типа:
```kotlin
val x: String? = y as String?
```
---

### Null safety
* Если у переменной явно указан тип, она точно не `null`. Это называется [null safety](https://kotlinlang.org/docs/null-safety.html).
```kotlin
var a: String = "abc" // Regular initialization means non-null by default
a = null // compilation error
```
* Если она может содержать `null` (nullable), это указывается при объявлении типа знаком `?`:
```kotlin
var b: String? = "abc" // can be set to null
b = null // ok
```
---

### Проверка на `null`
* Компилятор следит за nullable переменными. Такой код вызовет ошибку:

```kotlin
val b: String? = "Kotlin"
print("String of length ${b.length}") // Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type String?
```

* Можно явно проверить на `null`:

```kotlin
val b: String? = "Kotlin"
if (b != null) {
    print("String of length ${b.length}")
} else {
    print("Empty string")
}
```
---

### Оператор `?.`

* Безопасный доступ к полям, которые могут быть `null`, осуществляется через оператор `?.`:

```kotlin
val b: String? = null
println(b?.length) // null, нет исключения
```

* Этот оператор можно использовать в цепочке:
```kotlin
val departmentHead = bob?.department?.head?.name
```
* Оператор `?.` не вызывает исключений, просто возвращает null, если одна из переменных в цепочке `null`.
---

### Накричать на компилятор `!!`

* Если переменная nullable (то есть тип у неё со знаком `?`), но мы точно знаем, 
что там лежит значение, то можно обратиться к ней, используя оператор `!!`.
* Это выключит проверку компилятора на `null`.
* Однако, если там всё-таки лежит `null`, произойдёт [NPE](https://en.wikipedia.org/wiki/Null_pointer).

```kotlin
val b: String? = "Я строка"
println(b.length) // Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type String?
println(b!!.length) // 8
```
---

### Boxing

---

### Операторы
* `+, -, *, /, %` - математические операторы.
* `=` - присваивание.
* `+=, -=, *=, /=, %=` - присваивание вместе с операцией.
* `++, --` - инкремент, декремент.
* `&&, ||, !` - логические операторы.
* `==, !=` - проверка равенства по значению.
* `===, !==` - равенство по ссылке.
* `<, >, <=, >=` - сравнение.
* `[, ]` - доступ по индексу.
---

### Операторы
* `!!` - точно не null, иначе [NPE](https://ru.stackoverflow.com/questions/511085/%D0%A7%D1%82%D0%BE-%D1%82%D0%B0%D0%BA%D0%BE%D0%B5-null-pointer-exception-%D0%B8-%D0%BA%D0%B0%D0%BA-%D0%B5%D0%B3%D0%BE-%D0%B8%D1%81%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D1%82%D1%8C).
* `?.` - безопасный вызов или обращение к свойству, если не null.
* `?:` - [Элвис-оператор](https://en.wikipedia.org/wiki/Elvis_operator) 🕺.
* `::` - ссылка на класс или элемент класса.
* `..` - диапазон.
* [Полный список операторов](https://kotlinlang.org/docs/keyword-reference.html#operators-and-special-symbols).
---

### Сравнение по ссылке и по значению
* Оператор `==` сравнивает по значению (вызывает `.equals()`),
`===` сравнивает ссылки ([подробнее](https://kotlinlang.org/docs/equality.html#structural-equality)):

```kotlin
class Person(val p : String) {
  override fun equals(other: Any?): Boolean {
    if(other is Person) {
      return p == other?.p
    }
    return false
  }
}
val a = Person("abc")
val b = Person("abc")
println(a == b) // true
println(a === b) // false
```
---

### Сравнение по ссылке и по значению
* Однако, в случае с Int значения от -127 до 128 кешируются.
Значения вне этого диапазона [не кешируются](https://kotlinlang.org/docs/numbers.html#numbers-representation-on-the-jvm), а сравнение по ссылке даёт `false`.

```kotlin
val a: Int = 100
val boxedA: Int? = a
val anotherBoxedA: Int? = a

val b: Int = 10000
val boxedB: Int? = b
val anotherBoxedB: Int? = b

println(boxedA === anotherBoxedA) // true
println(boxedB === anotherBoxedB) // false
```
---

### Ветвление
* `if/else`:
```kotlin
if (a > b) {
    max = a
} else {
    max = b
}
```
* Switch:
```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```
---

### Циклы
* for:

```kotlin
for (i in 1..3) {
    println(i)
}
for (i in 6 downTo 0 step 2) {
    println(i)
}
for (item in collection) print(item)
```
* while:

```kotlin
while (x > 0) {
    x--
}

do {
    val y = retrieveData()
} while (y != null) // y is visible here!
```
---

### Функции
* Обычная функция:
```kotlin
fun double(x: Int): Int {
    return 2 * x
}
val result = double(2)
```
* Параметры по умолчанию (должны идти последними):
```kotlin
fun read(
    b: ByteArray,
    off: Int = 0,
    len: Int = b.size,
) { /*...*/ } 
read(b, 10, 5)
read(b, 10) // len = b.size
read(b) // остальные аргументы дефолтные
```
---

### Именованые аргументы
* Можно явно указывать имена аргументов функции, если хочется передать их в изменённом порядке:

```kotlin
fun pow(base, exponent) { /*...*/ }

pow(exponent = 5, base = 2)
```
---

### Лямбды
* Функцию, выполняющая одно действие, удобно записать в виде 
[лямбды](https://ru.wikipedia.org/wiki/%D0%9B%D1%8F%D0%BC%D0%B1%D0%B4%D0%B0-%D0%B2%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5):
```kotlin
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }
```
* Или короче:
```kotlin
val sum = { x: Int, y: Int -> x + y }
```
* [Подробнее](https://kotlinlang.org/docs/lambdas.html).
---

### Служебное слово `it`
* Если лямбда содержит __только один__ аргумент:
```kotlin
val square: (n: Int) -> Int = { n: Int -> n * n }
square(3) // 9
```
* Его можно заменить служебным словом `it`, опустив стрелку `->`:
```kotlin
val square: (n: Int) -> Int = { it * it }
```
* [Подробнее](https://kotlinlang.ru/docs/lambdas.html).
---

### Передача лямбды в аргументе
* Если функция принимает лямбду как аргумент и этот аргумент последний (trailing lambda), можно указывать её
вне скобок функции:

```kotlin
fun max(
    a: Int,
    b: Int,
    compare: (Int, Int) -> Boolean
): Boolean {
  return compare(a, b)
}

max(1, 2, {a, b -> a > b})
max(1, 2) {a, b -> a > b}
```
---

### Trailing lambda
* Если лямбда - единственный аргумент функции, то можно и круглые скобки не писать:

```kotlin
run { println("...") }
ints.filter { it > 0 }

// или даже так:
strings
    .filter { it.length == 5 }
    .sortedBy { it }
    .map { it.uppercase() }
```
---

### Деструктурирование параметров
* Если в параметре лямбды интересны только отдельные поля, можно их указать явно:
```kotlin
{ a -> ... } // one parameter
{ a, b -> ... } // two parameters
{ (a, b) -> ... } // a destructured pair
{ (a, b), c -> ... } // a destructured pair and another parameter
```
* Пример:
```kotlin
map.mapValues { entry -> "${entry.value}!" }
map.mapValues { (key, value) -> "$value!" }
```
* [Подробнее](https://kotlinlang.org/docs/destructuring-declarations.html#destructuring-in-lambdas).
---

### Стилистика кода
* Каждый класс или интерфейс достоин быть в отдельном файле.
* Имена классов и интерфейсов с **Б**ольшой **Б**уквы, имена переменных с *м*аленькой. Имена в [`camelCase`](https://ru.wikipedia.org/wiki/CamelCase).

```kotlin
// ❌
class verified_user(val Name: String)
val VerifiedUser = verified_user("Вася")

// ✔️
class VerifiedUser(val name: String)
val verifiedUser = VerifiedUser("Вася")
```

[Подробнее](https://kotlinlang.ru/docs/coding-conventions.html)
---

### Стилистика кода
* Имена переменным следует выбирать такие, чтоб было понятно, что там лежит.
* Следует избегать `translita v imenah peremennyh` (не все знают русский).

```kotlin
// ❌
var schetchik = 1
val dddddd = "Вася"

// ✔️
var counter = 1
val userName = "Вася"
```
---

### Стилистика кода
* Имя функции или метода должно содержать глагол, чтобы было понятно, что они делают.
* Имя не должно вводить в заблуждение читателя, надо описывать именно то, что происходит.

```kotlin
// ❌
fun sumAndNumber(s: String): String {
    return s.split("").reversed().joinToString("")
}

// ✔️
fun reverseString(s: String): String {
    return s.split("").reversed().joinToString("")
}
```
---

* Форматирование кода важно: плохо отформатированный код тяжело воспринимается и как правило содержит ошибки.
* Android Studio умеет форматировать код самостоятельно при нажатии комбинации клавиш `Ctrl + Alt + L`:

```kotlin
// ❌
fun reverseString( s:String):String
{
return s.split("") .reversed( ) .joinToString("")
}

// ✔️
fun reverseString(s: String): String {
    return s.split("").reversed().joinToString("")
}
```
---

### Задачи
* Пройти квиз https://www.w3schools.com/quiztest/quiztest.php?qtest=KOTLIN
* Ещё квиз (посложнее) https://kotlinquiz.com/
---

### Задачи
#### Написать функции, выполняющие следующую работу:
* Вернуть число в обратном порядке 123 -> 321.
* Вернуть число без повторяющихся цифр 111333456 -> 13456.
* Посчитать, сколько раз в данном числе встречается данная цифра (1355567, 5) -> 3.
* Посчитать самую длинную последовательность нулей/единиц в двоичной записи данного числа.
---

### Задачи
* Найти самый первый неповторяющийся символ в строке: 'фывфавыапрс' -> 'п'.
* Cгенерировать строку заданной длины из случайных символов, взятых из набора английскийх букв и цифр: (5) -> '2fvg6'.
* Вернуть только уникальные символы строки: 'позволяеткопироватьтекстиз' -> 'позвляеткираьс'.
---

### Полезные ссылки
* [Документация](https://kotlinlang.org/docs/home.html).
* [Игровая площадка](https://play.kotlinlang.org/).
* [Задачи с решениями](https://play.kotlinlang.org/koans/Introduction/Hello,%20world!/Task.kt).
* [Учебник по Kotlin (англ.)](https://www.youtube.com/watch?v=F9UC9DY-vIU).
* [Учебник по Kotlin (рус.)](https://www.youtube.com/watch?v=30tchn0TjaM).


