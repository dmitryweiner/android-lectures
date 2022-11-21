### Реализация архитектуры MVVM

![MVVM pattern](assets/mvvm/MVVMPattern.png)

[все лекции](https://github.com/dmitryweiner/android-lectures/blob/master/README.md)

[видео]()
---
### Исправление ошибки с binding
```
> Task :app:checkDebugDuplicateClasses FAILED
Duplicate class androidx.lifecycle.ViewModelLazy found in modules lifecycle-viewmodel-2.5.1-runtime (androidx.lifecycle:lifecycle-viewmodel:2.5.1) and lifecycle-viewmodel-ktx-2.2.0-runtime (androidx.lifecycle:lifecycle-viewmodel-ktx:2.2.0)
```

* build.gradle (module): 
```
dependencies {
    // ...
    implementation "androidx.lifecycle:lifecycle-viewmodel:2.5.1"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.5.1"
}
```
---

### Полезные ссылки
* http://www.fandroid.info/lektsiya-8-po-arhitekture-android-data-binding-mvvm/
* https://www.howtodoandroid.com/mvvm-retrofit-recyclerview-kotlin/
* https://proandroiddev.com/clean-architecture-on-android-using-feature-modules-mvvm-view-slices-and-kotlin-e9ed18e64d83
* https://dev.to/whatminjacodes/simple-example-of-mvvm-architecture-in-kotlin-4j5b
* https://developer.android.com/topic/libraries/architecture/livedata
* https://github.com/velmurugan-murugesan/Android-Example/tree/master/MvvmRetrofitRecyclerviewKotlin
