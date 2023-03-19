行末のセミコロンは省略可能。  
現場のコーディング規約に従う。  

コーディング規約  
変数、関数はキャメルケース   
型名、クラス名はアッパーキャメルケース  
インデントはスペース４つ  
public関数には Kotlin Doc ドキュメント(KDoc)をつける  

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
  
コメントアウト
```kotlin
// 行末までコメントアウト
/*囲った範囲をコメントアウト*/
```

変数定義
```kotlin
// varは変更可能な変数
var 変数名: 型 = 値 //　宣言
var = 変数名 // 再代入、型推論

// valは変更不可な変数
val 変数名: 型 = 値
```

値の指定

```kotlin
Int 1
Long 1L　// 尻にL
Double 1.1
Float 1.1F // 尻にF
桁区切り　1_111_111

Char 'a' シングルクウォート
String　"a" ダブルクウォート
```

バックスラッシュ記法あり  
  
配列
```kotlin
var 変数 = arrayListOf(データ,data)
val 変数 = arrayListOf(データ,data)

var ary = arrayListOf("データ","data")

println(ary) // データ　data
println(ary[0]) // データ
println(ary[1]) // data

ary[0] = "でーた" // 要素を指定して再代入
println(ary) // でーた　data
```

多次元配列

```kotlin
var ary1 = arrayListOf("田中","佐藤","鈴木")
var ary2 = arrayListOf("太郎","花子")

var aryList = arrayListOf(ary1, ary2)

println(aryList) // [[田中, 佐藤, 鈴木], [太郎, 花子]]
println(aryList[0]) // [田中, 佐藤, 鈴木]
println(aryList[1]) // [太郎, 花子]
println(aryList[0][0]) // 田中
println(aryList[0][1]) // 佐藤
```

# 配列、for文テスト
### 2023/03/19
```kotlin
fun main(){
    var ary = arrayListOf(10,20,30,40)

    for (num in ary) {
        if (num >= 10 && num < 20) {
            println("10代")
        } else if (num >= 20 && num < 30) {
            println("20代")
        } else if (num >= 30 && num < 40) {
            println("30代")
        } else {
            println("それ以外")
        }
    }
}
```
