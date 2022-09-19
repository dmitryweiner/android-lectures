### Анатомия приложения

![Android anatomy](assets/android-application/android-update.gif)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

#### Приложение компилируется в 
  [байткод](https://ru.wikipedia.org/wiki/%D0%91%D0%B0%D0%B9%D1%82-%D0%BA%D0%BE%D0%B4)
  и выполняется на [JVM](https://ru.wikipedia.org/wiki/Java_Virtual_Machine):

![JVM](assets/android-application/code.png)
---

### Жизнь APK

![APK](assets/android-application/apk.png)

[подробнее](https://medium.com/android-news/virtual-machine-in-android-everything-you-need-to-know-9ec695f7313b)
---

### Структура приложения
![](assets/android-application/structure.png)
---
    
### Конфиги
* В AndroidManifest.xml задаются:
  * Разрешения приложения.
  * Название приложения и пакета.
  * Иконка приложения и подпись под ней.
  * Экраны.
* В build.gradle:
  * Инструкции для сборки.
  * Зависимости от сторонних библиотек.
---

### Манифест
* [Документация](https://developer.android.com/guide/topics/manifest/manifest-intro), [RU](http://developer.alexanderklimov.ru/android/theory/AndroidManifestXML.php).
* Примерное содержание:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
---

### Код
* Код лежит в `java/%PACKAGE_NAME%`:
![](assets/android-application/source.png)
* Код может быть как на Java, так и на Kotlin.
* Лучше разбивать его на классы и модули для улучшения читаемости.
---

### Разметка экрана
![](assets/android-application/layout.png)
---

### Ресурсы
https://developer.android.com/guide/topics/resources/providing-resources

---

### Картинки, видео.

---

### Строковые ресурсы

---

### Тесты.

---

### Полезные ссылки