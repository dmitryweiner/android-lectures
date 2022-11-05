### Тосты, диалоги, всплывающие меню

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)
---

### Тосты
![](assets/layout/toast.png)
* Используется, чтобы показать пользователю сообщение поверх текущего экрана.
* Как вызывать:
```kotlin
val toast = Toast.makeText(
    applicationContext, // контекст 
    "Ура, я тост!",     // текст тоста
    Toast.LENGTH_SHORT  // длительность показа
)
toast.show()
```
---

### Тосты
* У тостов можно настраивать место [появления, вид](https://developer.alexanderklimov.ru/android/toast.php):
* Расположение:
```kotlin
// по центру 
toast.setGravity(Gravity.CENTER, 0, 0);

// слева сверху
toast.setGravity(Gravity.TOP or Gravity.LEFT, 0, 0) 
```
---

### Диалог

---
