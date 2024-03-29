### Android Studio

![Android studio](assets/android-studio/logo.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео](https://youtu.be/DcP5jGnij1o)
---

### Где скачать?
* [Качать здесь](https://developer.android.com/studio#downloads).
* Доступные операционные системы:
  * Linux
  * Windows
  * MacOS
---
  
### Запуск
![Песец](assets/android-studio/startup.png)
---

### Начальный экран
![Startup screen](assets/android-studio/startup-2.png)
---

### Выбор типа приложения
![Choose activity](assets/android-studio/choose-activity.png)
---

### Выбор языка
![choose language](assets/android-studio/choose-language.png)
---

### Свежесозданное приложение
![Свежесозданное приложение](assets/android-studio/app.png)
---

### Настройка эмулятора
* Tools -> Device manager -> Create device

![Create device](assets/android-studio/device.png)
---

### Настройка эмулятора
* Надо скачать эмулятор ОС. Рекомендую Android 11 (API 30):

![OS](assets/android-studio/os.png)
---

### Подключение телефона по USB
* Войти в "Developer options" в настройках. Для разных оболочек это делается по-разному,
обычно [множественными кликами на версию билда в настройках](https://www.digitaltrends.com/mobile/how-to-get-developer-options-on-android/).
* В Developer options выбрать следующие опции:
  * USB debugging.
  * Install via USB.
---
  
![developer-options](assets/android-studio/developer-options.png)

---

### Компиляция в `*.APK`
* Чтобы работала компиляция в APK, что может понадобиться для публикации приложения,
необходимо поставить следующие пакеты.
* Tools -> SDK Manager -> вкладка SDK Tools:
  * Android SDK Platform Tools
---

![SDK Tools](assets/android-studio/sdk.png)
---

### Где лежит APK?
![locate](assets/android-studio/locate.png)
---

### Debug
* При нажатии на 🪲 попадаем в режим дебага.
* Дебаггер останавливается на точках останова (красная точка слева от кода).
* Можно смотреть значения переменных.
---

![](assets/android-studio/debug.png)
---

### Логи
* Во время работы приложение может [писать в логи](https://developer.android.com/reference/android/util/Log)
  с помощью конструкции:

```kotlin
companion object {
    @JvmField val TAG: String = MyClass::class.java.simpleName
}

Log.d(TAG, "Я написал в логи!")
```
* 
* `.d` уровень логгирования DEBUG, [бывают](https://developer.android.com/studio/debug/am-logcat#WriteLogs) (по степени серьёзности):
```kotlin
Log.v //    VERBOSE;
Log.d //    DEBUG;
Log.i //    INFO;
Log.w //    WARN;
Log.e //    ERROR;
Log.a //    ASSERT;
```
* По ним можно фильтровать.
---

### Логи
![](assets/android-studio/logs.png)
---

### Полезные ссылки
* https://developer.android.com/studio/intro