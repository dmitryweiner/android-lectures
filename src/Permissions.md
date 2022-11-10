### Получение доступа к системным функциям

![permissions](assets/permissions/request.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)
---

### Вызов телефонного номера
On call button click:

Intent intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:" + "Your Phone_number"));
startActivity(intent);
Permission in Manifest:

 <uses-permission android:name="android.permission.CALL_PHONE" />
EDIT IN 2021

You should write it in your manifest file but at the same time you should ask in runtime.Like this code:

if (ContextCompat.checkSelfPermission(this,android.Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.CALL_PHONE),
                REQUEST_CODE)

        } else {

// else block means user has already accepted.And make your phone call here.

}
---

### Полезные ссылки
* https://developer.android.com/guide/topics/permissions/overview
* https://startandroid.ru/ru/blog/508-android-permissions.html
* https://gist.github.com/dmitryweiner/b27b2741ae58a047f587fedeba0c6755