# `Room`
### Библиотека для доступа к БД SQLite

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео](https://youtu.be/4NYDU9eKkpE)
---

### Диаграмма компонентов
![](assets/room/components.png)
---

* Entity - класс для доступа к строке таблицы.
* [Data Access Object](https://ru.wikipedia.org/wiki/Data_Access_Object) - класс для доступа к коллекциям (множеству строк).
* Database - сервисный класс для хранения подключения к БД, создания таблиц.
---

### Установка в Gradle Scripts
* build.gradle (module): 
```
plugins {
    // ...
    id 'kotlin-kapt' // <- добавить
}
// ... 
dependencies {
    // ...
    implementation "androidx.room:room-ktx:2.4.3"
    kapt "androidx.room:room-compiler:2.4.3"
}
```
* gradle.properties:
```
android.useAndroidX=true // скорее всего есть
android.enableJetifier=true
```
---

### Перепишем проект
* Перепишем проект, написанный в предыдущей [лекции по SQLite](https://dmitryweiner.github.io/android-lectures/SQLite.html).
* Создадим классы, относящиеся к БД:
  * `TodoEntity.kt` - для доступа к одиночной строке таблицы `todos`.
  * `TodoDao.kt` - для наборов строк.
  * `TodoDatabase.kt` - для коннекта к базе.
---

### TodoEntity.kt
```kotlin
@Entity(tableName = "todos")
class TodoEntity {
   @PrimaryKey(autoGenerate = true)
   var id: Long? = null
   var title: String = ""
   var isDone = false
}
```
---

### TodoDao.kt
```kotlin
@Dao
interface TodoDao {
   @get:Query("SELECT * FROM todos")
   val all: List<TodoEntity>

   @Query("SELECT * FROM todos WHERE id = :id")
   fun getById(id: Long): TodoEntity

   @Insert
   fun insert(todo: TodoEntity): Long

   @Update
   fun update(todo: TodoEntity)

   @Delete
   fun delete(todo: TodoEntity)
}
```
---

### TodoDatabase.kt
```
@Database(entities = [TodoEntity::class], version = 1)
abstract class TodoDatabase : RoomDatabase() {
    abstract fun todoDao(): TodoDao
}
```
---

### Подключение к БД в Activity
```kotlin
class MainActivity : AppCompatActivity() {

    lateinit var db: TodoDatabase
    lateinit var todoDao: TodoDao

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        db = Room.databaseBuilder(
            applicationContext,
            TodoDatabase::class.java, "tododb"
        )
            // выполняет в основном UI потоке
            .allowMainThreadQueries() 
            .build()
        todoDao = db.todoDao()
    }
}
```
---

### Использование в Activity
* Добавлени записи:

```kotlin
val todoEntity = TodoEntity()
todoEntity.title = "Покормить кота"
todoDao.insert(todoEntity)
```
* Чтение всех записей:

```kotlin
val todoEntities = todoDao.all
```
* Чтение записи по ID:

```kotlin
val todoEntity = todoDao.getById(1)
```
---

### Использование в Activity
* Обновление записи:

```kotlin
todoEntity.isDone = true
todoDao.update(todoEntity)
```
* Удаление записи:

```kotlin
todoDao.delete(todoEntity)
```
---

### Минусы
```kotlin
AppDatabase db =  Room.databaseBuilder(getApplicationContext(),
       AppDatabase.class, "database")
       .allowMainThreadQueries() // <-- UI поток!!!
       .build();
```
* Запросы выполняются синхронно в основном (UI) потоке.
* Объект Database создаётся каждый раз при создании Activity.
---

### Постоянный объект Database
* Для продолжения надо поставить [библиотеку для использования корутин](https://dmitryweiner.github.io/android-lectures/Async.html#/).
* Будем хранить ссылку на Database в самом классе (см. следующий слайд):

---

### Постоянный объект Database
```kotlin
@Database(
    entities = [TodoEntity::class],
    version = 1,
    exportSchema = true
)
abstract class TodoDatabase : RoomDatabase() {

    abstract fun todoDao(): TodoDao

    companion object {

        @Volatile
        private var INSTANCE: TodoDatabase? = null

        fun getDatabase(context: Context): TodoDatabase {
            // if the INSTANCE is not null, then return it,
            // if it is, then create the database
            if (INSTANCE == null) {
                synchronized(this) {
                    // Pass the database to the INSTANCE
                    INSTANCE = buildDatabase(context)
                }
            }
            // Return database.
            return INSTANCE!!
        }

        private fun buildDatabase(context: Context): TodoDatabase {
            return Room.databaseBuilder(
                context.applicationContext,
                TodoDatabase::class.java,
                "tododb"
            )
                .allowMainThreadQueries()
                .build()
        }
    }
}
```
---

### Использование постоянного объекта DAO
```kotlin
class MainActivity : AppCompatActivity() {

    // получение постоянного инстанса 
    private val todoDatabase by lazy { 
        TodoDatabase.getDatabase(this).todoDao()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_notes)
        
        // получение всех записей
        val list = todoDatabase.all
    }
}
```
---

### Асинхронный доступ к БД
* `TodoDatabase.buildDatabase()`:
```kotlin
private fun buildDatabase(context: Context): TodoDatabase {
    return Room.databaseBuilder(
        context.applicationContext,
        TodoDatabase::class.java,
        "tododb"
    )
        // .allowMainThreadQueries() <-- удалено!
        .build()
}
```
* Пример асинхронного добавления записи: 
```kotlin
lifecycleScope.launch {
    todoDatabase.updateTodo(editedTodo)
}
```
---

### Подписка на изменения
```kotlin
class MainActivity : AppCompatActivity() {

    lateinit var adapter: RecyclerViewAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_notes)
        
        // ... инициализация adapter и RecyclerView
    
        observeNotes()
    }
    
    private fun observeNotes() {
        lifecycleScope.launch {
            todoDatabase.all.collect {
                if (it.isNotEmpty()) {
                    // сохранение свежего набора в лист
                    list.clear()
                    list.addAll(it)
                }
            }
        }
    }
}
```
---

### Миграции в Room
* [Подробнее](https://developer.android.com/training/data-storage/room/migrating-db-versions),
  [rus](https://startandroid.ru/ru/courses/architecture-components/27-course/architecture-components/540-urok-12-migracija-versij-bazy-dannyh.html).
* Автоматические:

```kotlin
// BEFORE

@Database(
    entities = [TodoEntity::class],
    version = 1,
    exportSchema = true
)

// AFTER

@Database(
    entities = [TodoEntity::class],
    autoMigrations = [
        AutoMigration (from = 1, to = 2)
    ],
    version = 2,
    exportSchema = true
)
```
---

### Миграции в Room
* Ручные:

```kotlin
@Database(
    entities = [TodoEntity::class],
    version = 2,
    exportSchema = false
)
@TypeConverters(TodoConverters::class)
abstract class TodoDatabase : RoomDatabase() {

    // ...

    companion object {

        // ...

        private val MIGRATION_1_2: Migration = object : Migration(1, 2) {
            override fun migrate(database: SupportSQLiteDatabase) {
                // The following query will add a new column called lastUpdate to the notes database
                database.execSQL("ALTER TABLE todos ADD COLUMN lastUpdate INTEGER NOT NULL DEFAULT 0")
            }
        }

        private fun buildDatabase(context: Context): NoteDatabase {
            return Room.databaseBuilder(
                context.applicationContext,
                NoteDatabase::class.java,
                "tododb"
            )
                .addMigrations(MIGRATION_1_2)
                .build()
        }
    }
}
```
---

### Что дальше?
* Добавить репозиторий и MVVM.

![](assets/room/components-mvvm.png)

* Использовать [ORM](https://ru.wikipedia.org/wiki/ORM) [ktorm](https://github.com/kotlin-orm/ktorm).
---

### Полезные ссылки
Проекты с использованием Room:
* [RoomExample](https://github.com/johncodeos-blog/RoomExample)
* [android-room-example](https://github.com/irontec/android-room-example)
* [kotlin-sqlite-todolist](https://github.com/dmitryweiner/kotlin-sqlite-todolist/tree/master/WithRoom)

Что почитать:
* [Официальная документация](https://developer.android.com/codelabs/android-room-with-a-view-kotlin)
* [startandroid.ru](https://startandroid.ru/ru/courses/architecture-components/27-course/architecture-components/529-urok-5-room-osnovy.html)
* https://johncodeos.com/how-to-use-room-in-android-using-kotlin/
