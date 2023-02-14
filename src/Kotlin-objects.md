### Классы, наследование, ООП

![Kotlin classes](assets/kotlin-objects/classes.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео](https://youtu.be/KDGBungg4LE)
---

### ООП
* ООП - парадигма программирования, означает Объектно Ориентированное Программирование.
* Принципы:
  * Инкапсуляция.
  * Наследование.
  * Полиморфизм.
  * Абстракция.
* [Подробнее](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5).
----

![](assets/kotlin-objects/oop.png)
---

### Классы
* [POJO](https://ru.wikipedia.org/wiki/POJO)-класс в Java:

```java
public class MyBean {

    private String someProperty;

    public String getSomeProperty() {
         return someProperty;
    }

    public void setSomeProperty(String someProperty) {
        this.someProperty = someProperty;
    }
}
```

* Тот же класс в Kotlin:

```kotlin
class MyBean(val someProperty: String)
```
---

### Класс
```kotlin
class ClassName(// как будто конструктор
    // поля класса, инициализирующиеся в конструкторе
    val fieldName: String, 
    val fieldName1: Boolean, 
    val fieldName2: Int, 
) { 
    // поля класса, инициализирующиеся как-то иначе
    val fieldName3 = 0
    
    // методы класса
    fun method() {}
}
```

---

### Класс
* Объявление класса:
```kotlin
class Person(
    val firstName: String, 
    val lastName: String, 
    var isEmployed: Boolean = true)
```
* Инициализация поля класса, которого нет в конструкторе:
```kotlin
class Customer(name: String) {
    val customerKey = name.uppercase()
    var counter = 0
}
```
* [Подробнее](https://kotlinlang.org/docs/classes.html).
---

### Создание экземпляра класса
* В Котлине нет служебного слова `new`, поэтому для создания экземпляра просто пишем `ClassName(params...)`:

```kotlin
class Customer(name: String) {
  val customerKey = name.uppercase()
}

// создание экземпляра:
val customer = Customer("Василий Пупкин")
```
* Доступ к полям класса:
```kotlin
println(customer.name) // Василий Пупкин
println(customer.customerKey) // ВАСИЛИЙ ПУПКИН
```
---

### Выполнение кода при создании экземпляра класса
Блок `init {}`
```kotlin
class Customer(name: String) {
    var customerKey: String? = null
    
    init {
        println("Меня создали!")
        customerKey = name.uppercase()
    }
}

val customer = Customer("John Smith") // "Меня создали!"
```
---

### Методы класса
```kotlin
class Person(val firstName: String, val lastName: String) { 
    fun getFullName(): String {
        return "$firstName $lastName"
    }
}

val p = Person("Peter", "Griffin")
// вызов метода:
print(p.getFullName())
```
---

### Геттеры и сеттеры
* Геттер:

```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
```
* Сеттер:

```kotlin
class Counter() {
    var counter = 0 // the initializer assigns the backing field directly
        set(value) {
            if (value >= 0)
                field = value
                // counter = value // ERROR StackOverflow: Using actual name 'counter' would make setter recursive
        }
}
```
---

### Дефолтное поведение классов
* Классы в Kotlin по умолчанию `public` и `final`, это не надо указывать отдельно.
* Чтобы дать другому классу возможность наследовать от этого, нужно добавить модификатор `open`.
* Все поля класса по умолчанию `public`, это не нужно указывать отдельно.
---

### Наследование
* Чтобы класс можно было наследовать, нужно добавить модификатор `open`:

```kotlin
open class Parent(p: Int)

class Child(p: Int) : Parent(p)
```
* Переопределение методов `override`:

```kotlin
open class Shape {
    // метод должен быть open
    open fun draw() { /*...*/ }
    fun fill() { /*...*/ }
}

class Circle() : Shape() {
    override fun draw() { /*...*/ }
}
```
---

### Абстрактные классы и методы
* Нельзя создать экземпляр абстрактного класса, необходимо наследовать этот класс и реализовать абстрактные методы:

```kotlin
abstract class Polygon {
    abstract fun draw()
}

class Rectangle : Polygon() {
    override fun draw() {
        // draw the rectangle
    }
}
```
---

### Модификаторы доступа
* `private` - видно только внутри класса.
* `protected` - видно внутри класса и всех потомков.
* `internal` - видно в пределах модуля.
* `public` - видно всем (по умолчанию).
* [Подробнее](https://kotlinlang.org/docs/visibility-modifiers.html).
---

### Интерфейсы
* Интерфейс создаётся служебным словом `interface`:

```kotlin
interface MyInterface {
    fun bar()
    fun foo() {
      // optional body
    }
}

class Child : MyInterface {
    override fun bar() {
        // body
    }
}
```

* Для имплементации интерфейса его указывают после класса через `":"`, но без `()`.

[Подробнее](https://kotlinlang.org/docs/interfaces.html)
---

### Статические методы и поля классов

* В Kotlin нет статических полей и методов, как в Java.
* Вместо этого есть классы-компаньоны, в которых лежат методы и константы, доступные
без создания экземпляра родительского класса:

```kotlin
class Example {
    companion object {
        const val CONSTANT = "something"
    }
}

println(Example.CONSTANT) // "something"
```
---

### Специальные классы
* `data` - для хранения данных.
* `sealed` - запечатанные (без возможности изменения).
* `inline` - для хранения одного значения.
* `enum` - для хранения набора констант.
* `object` - анонимный класс.
---

### data class
* В этих классах реализованы методы:
  * `equals()/hashCode()`
  * `toString()` "User(name=John, age=42)"
  * `copy()`
* [Подробнее](https://kotlinlang.org/docs/data-classes.html), [ещё](https://bimlibik.github.io/posts/kotlin-data-classes/).

```kotlin
data class User(val name: String, val age: Int)
```
---

### data class
* В основном конструкторе должен быть как минимум один параметр.
* Все параметры основного конструктора должны быть отмечены ключевыми слова val или var (рекомендуется val).
* Классы данных не могут быть отмечены ключевыми словами `abstract, open, sealed, inner`.
---

### sealed class
* Добавление модификатора sealed к суперклассу ограничивает возможность создания подклассов. 
* Все прямые подклассы должны быть вложены в суперкласс. 
* Запечатанный класс не может иметь наследников, объявленных вне класса.
* Во время компиляции известны все прямые наследники изолированного класса.
* [Подробнее](https://developer.alexanderklimov.ru/android/kotlin/sealed.php).
---

### sealed class
```kotlin
sealed interface Error // имеет реализации только в том же пакете и модуле

sealed class FileReadError(): Error // расширяется только в том же пакете и модуле
sealed class DatabaseError(): Error
sealed class RuntimeError(): Error
open class CustomError(): Error // может быть расширен везде, где виден

fun log(e: Error) = when(e) {
    is FileReadError -> { println("Error while reading file ${e.file}") }
    is DatabaseError -> { println("Error while reading from database ${e.source}") }
    RuntimeError ->  { println("Runtime error") }
    // оператор `else` не требуется, потому что мы покрыли все возможные случаи
}
```
---

### enum class
* Реализация типобезопасных перечислений:
```kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```
* Можно указать значения:
```
enum class Color(val rgb: Int) {
    RED(0xFF0000),
    GREEN(0x00FF00),
    BLUE(0x0000FF)
}
println(Color.BLUE)
```
* [Подробнее](https://bimlibik.github.io/posts/kotlin-enum-classes/).
---

### inline class
* Классы, служащие обёрткой над примитивами, для ускорения можно объявить `inline`:

```kotlin
@JvmInline
value class Password(private val s: String)

val securePassword = Password("Don't try this in production")
```
* [Подробнее](https://kotlinlang.org/docs/inline-classes.html).
---

### Анонимный класс - object
* Когда объект требуется один раз, нет смысла заводить под него класс:
```kotlin
val helloWorld = object {
    val hello = "Hello"
    val world = "World"
    override fun toString() = "$hello $world"
}
```
* Объект может наследовать другой класс:
```kotlin
window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { /*...*/ }
    override fun mouseEntered(e: MouseEvent) { /*...*/ }
})
```
* [Подробнее](https://kotlinlang.org/docs/object-declarations.html).
---

### Ленивая инициализация `by lazy`
* Есть переменная, чьё значение имеет смысл вычислять только
в момент доступа к ней.
* Для можно использовать [делегат](https://ru.wikipedia.org/wiki/%D0%A8%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%B4%D0%B5%D0%BB%D0%B5%D0%B3%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)
`by lazy`.
* Значение будет вычислено при первом обращении к свойству.
* `lazy` принимает лямбду, которая вызывается при попытке доступа:
---

### Ленивая инициализация `by lazy`
```kotlin
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}

println(lazyValue)
println(lazyValue)
```
---

### Модификатор поля `lateinit`
* Переменная не nullable, но её значение будет присвоено гарантированно ДО первого чтения значения?
* Тогда можно использовать `lateinit`:

```kotlin
public class MyTest {
    lateinit var subject: TestSubject
    init {
        subject = TestSubject()
    }
    fun test() {
        subject.method()  // dereference directly
    }
}
```
* Переменная может быть только `var`.
---

### Пример с `lateinit`
```kotlin
class LoginFragment : Fragment() {

    private lateinit var usernameEditText: EditText
    private lateinit var passwordEditText: EditText
    private lateinit var loginButton: Button
    private lateinit var statusTextView: TextView

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        usernameEditText = view.findViewById(R.id.username_edit_text)
        passwordEditText = view.findViewById(R.id.password_edit_text)
        loginButton = view.findViewById(R.id.login_button)
        statusTextView = view.findViewById(R.id.status_text_view)
    }
}
```
---

### Различия lazy и lateinit
* Делегат `lazy { ... }` используется только для val полей, lateinit только для var.
* lateinit нельзя использовать для nullable полей (со знаком ?).
* lateinit var может быть инициализировано везде в классе, by lazy { ... } только в лямбде.
* by lazy { ... } потокобезопасно в отличие от lateinit var.
* [Подробнее](https://stackoverflow.com/a/36623703/3012961).

---

### Модули
* Как и в Java, код разделяется на модули:

```kotlin
package org.example

fun printMessage() { /*...*/ }
class Message { /*...*/ }
```
* Которые потом можно импортировать либо целиком, либо по отдельности:

```kotlin
import org.example.Message // Message is now accessible without qualification
import org.example.* // everything in 'org.example' becomes accessible
```
* [Подробнее](https://kotlinlang.org/docs/packages.html).
---

### Модули
* В первой строке модуля определяется его имя:
```kotlin
package org.example
```
* Имя разбивается по доменному принципу:
![](assets/kotlin-objects/modules.png)
---

### Именованный импорт
* При импорте можно задать имя, чтобы оно не пересекалось с другим импортом:
```kotlin
import kotlin.random.Random as KRandom
import java.util.Random as JRandom
```
---

### Функции области видимости
[Подробнее](https://kotlinlang.ru/docs/scope-functions.html), [хорошие примеры](https://stackoverflow.com/a/61247877).

![](assets/kotlin-objects/context.jpg)
---

### apply
```kotlin
data class User(var name: String, var age: Int)

val user = User("Василий", 19)
val changedUser = user.apply {
    // this == user
    name = "Вкусилий" // аналогично this.name
    age = 6 // аналогично this.age
}
// apply возвращает исходный объект

println(changedUser) // User(name=Вкусилий, age=6)
```
---

### run
```kotlin
class User(val name: String, val age: Int)

val user = User("Василий", 19)
val userToPrint = user.run {
    // this == user
    // this.name == name
    // this.age == age
    // что вернёт лямбда, будет результатом run
    name + ", " + age + " лет"
}
println(userToPrint) // "Василий, 19 лет"
```
---

### also
```kotlin
data class User(var name: String, var age: Int)

val user = User("Василий", 19)
val changedUser = user.also {
    // it == user
    it.name = "Вкусилий"
    it.age = 6
}
// also возвращает исходный объект

println(changedUser) // User(name=Вкусилий, age=6)
```
---

### let
```kotlin
class User(val name: String, val age: Int)

val user = User("Василий", 19)
val userToPrint = user.let {
    // it == user
    // что вернёт лямбда, будет результатом let
    it.name + ", " + it.age + " лет"
}
println(userToPrint) // "Василий, 19 лет"
```
---

### Задачи
* https://play.kotlinlang.org/koans/Classes/Data%20classes/Task.kt
* Описать класс для хранения комплексных чисел `Complex` и функцию для сложения их `fun sum(c1: Complex, c2: Complex): Complex`.

```kotlin
class Complex /* ... */

fun sum(c1: Complex, c2: Complex): Complex {
    /* ... */
}

fun main() {
    val c1 = Complex(1, 2)
    val c2 = Complex(2, 5)
    println(sum(c1, c2)) // Complex(3, 7)
}
```
---

* Описать классы для хранения фигур: родитель `Shape`, потомки `Rectangle` и `Square`. У родителя абстрактный метод вычисления площади
и периметра. Реализовать методы в потомках, проверить.

```kotlin
abstract class Shape {
    abstract fun area(): Int
    abstract fun perimeter(): Int
}

class Square(val a: Int) /* TODO */ {
    /* TODO */
}

class Rectangle(val a: Int, val b: Int) /* TODO */ {
    /* TODO */
}

fun main() {
    val s = Square(4)
    val r = Rectangle(2, 5)
    println(s.area()) // 16
    println(r.area()) // 10
}
```
---

### Полезные ссылки
* https://kotlinlang.ru/docs/classes.html
* https://developer.alexanderklimov.ru/android/kotlin/class.php
* https://bimlibik.github.io/tags/kotlin/
