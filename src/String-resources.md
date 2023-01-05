### 📝 Использование строковых ресурсов в приложении

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)
---

### Типы строковых ресурсов
* String - просто строка.
* String array - набор строк.
* Quantity strings - строки с числительными (для плюрализации).
---

### Использование строк
* Строки лежат в файле `res/values/strings.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello!</string>
</resources>
```

* Использование в коде:
```kotlin
val s = resources.getText(R.string.hello)
println(s) // "Hello!"
```
---

### Использование строк лейаутах

```xml
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/hello" />
```

<input type="text" value="Hello!" />
---

### Строки с переносами

---

### Массивы строк

---

### Плюрализация

---

### Шаблонные строки с параметрами

---

### Мультиязычность

---

### 

---

### 

---

### 

---

### Полезные ссылки
* https://stackoverflow.com/questions/3656371/is-it-possible-to-have-placeholders-in-strings-xml-for-runtime-values
* https://developer.android.com/guide/topics/resources/string-resource
