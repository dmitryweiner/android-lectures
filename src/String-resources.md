### Использование ресурсов в приложении

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)
---

### Доступ к строковым ресурсам
```xml
<string name="earth">Earth</string>
<string name="moon">Moon</string>
<string-array name="system">
    <item>@string/earth</item>
    <item>@string/moon</item>
</string-array>
```

```kotlin
val s = resources.getText(R.string.earth)
println(s) // Earth

val arr = resources..getStringArray(R.string.system)
println(arr.joinToString(", ")) // Earth, Moon
```
---

### Полезные ссылки
* https://stackoverflow.com/questions/3656371/is-it-possible-to-have-placeholders-in-strings-xml-for-runtime-values
* https://developer.android.com/guide/topics/resources/string-resource
