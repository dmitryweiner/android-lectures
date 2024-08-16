### 🤝 Делегаты

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)
---

### Что это такое?

Делегаты позволяют автоматически управлять свойствами класса с помощью внешних объектов, 
которые принимают на себя реализацию логики для доступа или изменения этих свойств. 
Это мощный инструмент, который помогает сократить количество шаблонного кода и сделать 
код более читаемым и структурированным.

---

### Зачем нужны делегаты?

* **Сокращение шаблонного кода:** Делегаты позволяют переместить повторяющийся код в одно место, 
что делает код более поддерживаемым.
* **Инкапсуляция логики:** Логика работы с данными инкапсулируется в объекте делегата, 
что улучшает разделение ответственности.
* **Повторное использование:** Один и тот же делегат может использоваться для различных свойств 
в разных классах.
---

### Языки, в которых есть делегаты

* Kotlin
* [C#](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/using-delegates)
* [Swift](https://developer.apple.com/documentation/swift/using-delegates-to-customize-object-behavior)
---

### Как работает

Когда вы используете делегат для свойства, Kotlin перенаправляет вызовы методов get и set 
(если свойство изменяемое) к объекту делегата. 
Делегат сам решает, как обрабатывать чтение и запись этого свойства.

---

### Простейший пример

```kotlin
class Example {
    // Свойство p делегирует свою логику объекту класса Delegate.
    var p: String by Delegate()
}

class Delegate {
    // Свойство p делегирует свою логику объекту класса Delegate.
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "Hello, ${property.name}!"
    }

    // setValue вызывается при присвоении нового значения свойству p.
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name}' in $thisRef")
    }
}

fun main() {
    val example = Example()
    println(example.p) // Вызовет Delegate.getValue
    example.p = "New value" // Вызовет Delegate.setValue
}
```
---

### Встроенные делегаты в Kotlin

* **lazy**: Используется для инициализации тяжелых объектов только при необходимости.
* **vetoable**: для проверки значений перед их присвоением (валидация).
* **observable**:отслеживания изменений, логгирование.
---

### lazy

```kotlin
val lazyValue: String by lazy {
    // Это будет выполнено только в случае чтения значения переменной
    println("Computed!")
    "Hello"
}

println(lazyValue)
// В консоли будет:
// Computed
// Hello
```
---

### observable

```kotlin
var observedValue: String by Delegates.observable("Initial value") { prop, old, new ->
    // этот код выполнится при изменении значения observedValue
    println("$old -> $new")
}
```
---

### vetoable

```kotlin
var vetoableValue: String by Delegates.vetoable("Initial value") { _, _, new ->
    // если новое значение не удовлетворяет условию, оно не будет присвоено
    new.length > 5
}
```
---

### Делегаты для коллекций

Делегаты используются для коллекций, когда нужно предоставить доступ к элементам коллекции через 
свойства класса, инкапсулируя логику работы с данными в одном месте.

```kotlin
class Example {
    val map = mapOf("name" to "John Doe", "age" to 25)
    val name: String by map
    val age: Int by map
}
```
---

### Делегаты для коллекций

```kotlin
class User(map: Map<String, Any?>) {
    val name: String by map
    val age: Int by map
    val email: String by map
}

fun main() {
    val userMap = mapOf(
        "name" to "John Doe",
        "age" to 30,
        "email" to "johndoe@example.com"
    )

    val user = User(userMap)

    println(user.name)  // Выведет: John Doe
    println(user.age)   // Выведет: 30
    println(user.email) // Выведет: johndoe@example.com
}
```
---

### Делегат при объявлении класса

В Kotlin можно использовать делегирование не только для свойств, но и при объявлении класса. 
Конструкция `class Derived(b: Base) : Base by b` представляет собой делегирование интерфейса
(interface delegation) или реализации, которое позволяет классу делегировать реализацию 
одного или нескольких интерфейсов другому объекту.

---

### Делегат при объявлении класса

```kotlin
interface Base {
    fun printMessage()
    fun printAnotherMessage()
}

class BaseImpl(val x: Int) : Base {
    override fun printMessage() { println(x) }
    override fun printAnotherMessage() { println("Another message") }
}

class Derived(b: Base) : Base by b
```
---

### Более детальный пример

```kotlin
interface Printable {
    fun print()
}

class Printer(val name: String) : Printable {
    override fun print() {
        println("Printing from $name")
    }
}

class SmartPrinter(b: Printable) : Printable by b {
    fun smartPrint() {
        println("Smart print")
        print()  // Вызов метода, делегированного из Printable
    }
}

fun main() {
    val printer = Printer("HP Printer")
    val smartPrinter = SmartPrinter(printer)
    
    smartPrinter.smartPrint() // Выведет: "Smart print" и "Printing from HP Printer"
}
```
