### SQLite

![](assets/)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---

> SQLite — это быстрая и легкая встраиваемая однофайловая СУБД на языке C, которая не имеет сервера и позволяет хранить всю базу локально на одном устройстве. Для работы SQLite не нужны сторонние библиотеки или службы.

[Подробнее](https://blog.skillfactory.ru/glossary/sqlite/)
---

### Типы данных
SQLite поддерживает только четыре типа данных, которые реализованы в SQL:
* INTEGER — целое число;
* REAL — дробное число;
* TEXT — текст;
* BLOB — двоичные данные.
Также существует особое значение NULL — отсутствие данных.
---
### Создание таблиц
```sql

```
---

### Изменение записей
```sql
```
---

### Адаптер для доступа к БД
```kotlin
class DBHelper extends SQLiteOpenHelper {
    companion object {
        // версия БД
        const val DATABASE_VERSION = 1
        // название БД
        const val DATABASE_NAME = "tododb"
        // название таблицы
        const val TABLE_NAME = "todos"
        // названия полей
        const val KEY_ID = "id"
        const val KEY_TITLE = "title"
        const val KEY_IS_DONE = "is_done"
    }
    
    public DBHelper(@Nullable Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("drop table if exists " + TABLE_NAME);
        onCreate(db);
    }

    public ArrayList<Todo> getTodos() {
        ArrayList<Todo> result = new ArrayList<Todo>();
        SQLiteDatabase database = this.getWritableDatabase();
        Cursor cursor = database.query(TABLE_NAME, null, null, null,
                null, null, null);
        if (cursor.moveToFirst())
        {
            int idIndex = cursor.getColumnIndex(KEY_ID);
            int titleIndex = cursor.getColumnIndex(KEY_TITLE);
            int isDoneIndex = cursor.getColumnIndex(KEY_IS_DONE);
            do {
                Todo todo = new Todo(
                        cursor.getString(idIndex),
                        cursor.getString(titleIndex),
                        cursor.getInt(isDoneIndex) == 1);
                result.add(todo);
            } while (cursor.moveToNext());
        }
        cursor.close();
        return result;
    }

    public void addTodo(Todo todo) {
        SQLiteDatabase database = this.getWritableDatabase();
        ContentValues contentValues = new ContentValues();
        contentValues.put(DBHelper.KEY_ID, todo.getId());
        contentValues.put(DBHelper.KEY_TITLE, todo.getTitle());
        contentValues.put(DBHelper.KEY_IS_DONE, todo.getIsDone() ? 1: 0);
        database.insert(DBHelper.TABLE_NAME, null, contentValues);
        this.close();
    }

    public void updateTodo(Todo todo) {
        SQLiteDatabase database = this.getWritableDatabase();
        ContentValues contentValues = new ContentValues();
        contentValues.put(DBHelper.KEY_TITLE, todo.getTitle());
        contentValues.put(DBHelper.KEY_IS_DONE, todo.getIsDone() ? 1: 0);
        database.update(DBHelper.TABLE_NAME, contentValues, DBHelper.KEY_ID + " = ?", new String[] {todo.getId()});
        this.close();
    }

    public void removeTodo(Todo todo) {
        SQLiteDatabase database = this.getWritableDatabase();
        database.delete(DBHelper.TABLE_NAME, DBHelper.KEY_ID + " = ?", new String[] {todo.getId()});
        this.close();
    }

    public void removeAllTodos() {
        SQLiteDatabase database = this.getWritableDatabase();
        database.delete(DBHelper.TABLE_NAME, null, null);
        this.close();
    }
}
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

