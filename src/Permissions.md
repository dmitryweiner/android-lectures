### Получение доступа к системным функциям

![permissions](assets/permissions/request.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)
---

### Разрешения пользователя
* Приложение получает доступ к различным функциям операционной системы только 
после разрешения пользователя.
* Функции, к которым собирается обращаться приложение, описываются в Manifest.xml.
* [Список всех разрешений](https://developer.android.com/reference/android/Manifest.permission.html).
---

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.test.someapplication">
 
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.SEND_SMS" />
 
    <application
    ...
    ></application>
 
</manifest>
```
---

### Виды разрешений

*  Разрешения бывают двух видов:
  * **normal** - показываются только при установке приложения, не требуют дополнительного запроса разрешения.
  * **dangerous** - требуют явного запроса разрешения от пользователя:
  
![](assets/permissions/dialog.png)
----

normal

```
ACCESS_LOCATION_EXTRA_COMMANDS
ACCESS_NETWORK_STATE
ACCESS_NOTIFICATION_POLICY
ACCESS_WIFI_STATE
BLUETOOTH
BLUETOOTH_ADMIN
BROADCAST_STICKY
CHANGE_NETWORK_STATE
CHANGE_WIFI_MULTICAST_STATE
CHANGE_WIFI_STATE
DISABLE_KEYGUARD
EXPAND_STATUS_BAR
GET_PACKAGE_SIZE
INSTALL_SHORTCUT
INTERNET
KILL_BACKGROUND_PROCESSES
MODIFY_AUDIO_SETTINGS
NFC
READ_SYNC_SETTINGS
READ_SYNC_STATS
RECEIVE_BOOT_COMPLETED
REORDER_TASKS
REQUEST_IGNORE_BATTERY_OPTIMIZATIONS
REQUEST_INSTALL_PACKAGES
SET_ALARM
SET_TIME_ZONE
SET_WALLPAPER
SET_WALLPAPER_HINTS
TRANSMIT_IR
UNINSTALL_SHORTCUT
USE_FINGERPRINT
VIBRATE
WAKE_LOCK
WRITE_SYNC_SETTINGS
```
----

dangerous

```
READ_CALENDAR
WRITE_CALENDAR
CAMERA
READ_CONTACTS
WRITE_CONTACTS
GET_ACCOUNTS
ACCESS_FINE_LOCATION
ACCESS_COARSE_LOCATION
RECORD_AUDIO
READ_PHONE_STATE
READ_PHONE_NUMBERS 
CALL_PHONE
ANSWER_PHONE_CALLS 
READ_CALL_LOG
WRITE_CALL_LOG
ADD_VOICEMAIL
USE_SIP
PROCESS_OUTGOING_CALLS
BODY_SENSORS
SEND_SMS
RECEIVE_SMS
READ_SMS
RECEIVE_WAP_PUSH
RECEIVE_MMS
READ_EXTERNAL_STORAGE
WRITE_EXTERNAL_STORAGE
ACCESS_MEDIA_LOCATION
ACCEPT_HANDOVER
ACCESS_BACKGROUND_LOCATION
ACTIVITY_RECOGNITION
```
---

### Алгоритм запроса разрешения во время выполнения

---

```kotlin
fun onWithPermissions(view: View?) {
    if (checkPermission()) {
        writeToFile()
    } else {
        requestPermission()
    }
}

private fun requestPermission() {
    if (ActivityCompat.shouldShowRequestPermissionRationale(
            this,
            Manifest.permission.WRITE_EXTERNAL_STORAGE
        )
    ) {
        Toast.makeText(this, "Permissions should be granted", Toast.LENGTH_LONG).show()
    } else {
        ActivityCompat.requestPermissions(
            this,
            arrayOf(Manifest.permission.WRITE_EXTERNAL_STORAGE),
            PERMISSION_REQUEST_CODE
        )
    }
}

private fun checkPermission(): Boolean {
    val result = ContextCompat.checkSelfPermission(
        applicationContext,
        Manifest.permission.WRITE_EXTERNAL_STORAGE
    )
    return if (result == PackageManager.PERMISSION_GRANTED) {
        true
    } else {
        false
    }
}

override fun onRequestPermissionsResult(
    requestCode: Int,
    permissions: Array<String?>,
    grantResults: IntArray
) {
    super.onRequestPermissionsResult(
        requestCode,
        permissions,
        grantResults
    )
    if (requestCode == PERMISSION_REQUEST_CODE) {
        if (grantResults.size > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            Toast.makeText(this@MainActivity, "Permission granted", Toast.LENGTH_SHORT).show()
            writeToFile()
        } else {
            Toast.makeText(this@MainActivity, "Permission denied", Toast.LENGTH_SHORT).show()
        }
    }
}

```
---

### Вызов телефонного номера
Permission in Manifest:
```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
```
On call button click:
``` kotlin
if (ContextCompat.checkSelfPermission(this,android.Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.CALL_PHONE),
                REQUEST_CODE)
                Intent intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:" + "Your Phone_number"));
                startActivity(intent);
        } else {

// else block means user has already accepted.And make your phone call here.

}

```
---

### Полезные ссылки
* https://startandroid.ru/ru/blog/508-android-permissions.html
* https://developer.android.com/guide/topics/permissions/overview
* https://source.android.com/docs/core/permissions
