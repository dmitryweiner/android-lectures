### 💾 Запись и чтение файлов 📖

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

---

### Места для хранения файлов
* Private storage - внутренний каталог приложения. 
Туда может писать и читать только наше  приложение, зато без запроса разрешений.
* Shared storage - общее хранилище файлов, куда имеют доступ многие приложения.
  * EXTERNAL_STORAGE - внешняя SD-карта.

---

### Изменения в API Android
* <= 6 - можно писать на SD-карту, указав это в Manifest.xml.
* <= 10 - можно писать на карту, запросив разрешение во время выполнения.
* 11 - можно писать только в Scoped storage:
  * Фото.
  * Документы.
  * Загрузки.
---

### Добавление прав в AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<application 
    android:requestLegacyExternalStorage="true"
    ...
    >
    <!-- ... -->
</application>

```
---

### Запись в Private storage

```kotlin

```
---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

### 

---

