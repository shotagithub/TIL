拡張子は.ktを用いる。  
jar（java形式）や.js(javascript形式)にコンパイル可能。  

```kotlin
// JAVA形式にコンパイル
kotlinc hoge.kt -include-runtime -d hoge.jar
// JavaScript形式にコンパイル
kotlinc hoge.kt -output hoge.js
```

コマンドライン引数を受け取る  
```kotlin
fun main(args: Array<String>) {
    println(args.contentToString())
}
```
