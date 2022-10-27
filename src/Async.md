## Асинхронный код

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()

---
```kotlin
val textView = findViewById<TextView>(R.id.textView)

var thread: Thread? = null
var i = 0

val button1 = findViewById<Button>(R.id.button1)
val button2 = findViewById<Button>(R.id.button2)

button1.setOnClickListener {
    thread = thread(start = false) {

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
    thread?.start()
}
button2.setOnClickListener {
    thread?.interrupt()
}
```
---

### Корутины: установка
* build.gradle (Module):
```
dependencies {
    // ...
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.9'
}
```
---

### Полезные ссылки
* https://developer.android.com/kotlin/coroutines
* https://metanit.com/kotlin/tutorial/8.1.php

---
<iframe width="560" height="315" src="https://www.youtube.com/embed/b4mBmi1QNF0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
