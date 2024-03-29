# クラス継承テスト
### 2023/03/19

```ruby
class Human

  def initialize(human_name, human_age)
    @human_state = {}
    
    @human_state[:name] = human_name
    @human_state[:age] = human_age
  end

  def eat(food)
    puts "#{@human_state[:name]}は#{food}を食べる"
  end

  def walk(place)
    puts "#{@human_state[:name]}は#{place}を歩く"
  end
end

class HumanGreat < Human
  def great(who)
   puts "hello #{who}"
  end
end

taro = Human.new("太郎", 18)
taro.eat("家系ラーメン")
taro.walk("東白楽")


hanako = HumanGreat.new("花子", 15)
hanako.eat("カレー")
hanako.walk("横須賀")
hanako.great("world")
```

# セッションとは？
### 2023/03/15
セッションはステートフルな（ページを移動しても状態を維持する）仕組み。  
通常のふるまいはステートレス。  
カート機能などの実装には必須な知識となる。  
railsではデフォルトだとCookieとしてクライアントのブラウザに情報を保持する。  
railsのセッションデータはビューとコントローラーでの使用を推奨する。  
  
データを入れる記述

```ruby
session[:name] = params[:user][:name]
```

という具合に。  
２段ネストの際にはモデルを紐づける必要があるという制約が発生する。  

```ruby
session[:user] = User.new() 
session[:user][:name] = "田中"
```

セッションデータの参照  
cookieの仕様でネストの２段目以降はシンボル指定できない。

```ruby
@user[:user][:name] = session[:user]["name"] 
# session[:user]内のnameをシンボル指定できない。
```

セッションの更新
```ruby
session[:user]["name"] = user_params[:name]
# 更新時もsession[:user]内のnameをシンボル指定できない。
```

保持データが全て消える御法度

```ruby
session[:user] = user_params
# 既存情報にparamsの内容を再代入してしまうため
```

セッションの削除
```ruby
session[:user].clear
```

セッションがブラウザを閉じた時に自動削除されるかはブラウザによるので、  
セキュリティリスクを排除するために、不要になったタイミングで能動的に削除する必要がある。


# payjpのAPIについて
### 2023/03/14
クレジットカードの決済処理を事業者の代わりに行うオンライン決済代行サービス  
現在のバージョンはv2  
現在出回っているブログの説明などではv1についての記述が多く、  
自前のフォームに決済機能をつけられるように見えるが、　　
それはあくまでv1時代の話。  
v2は法改正により代行会社が用意した　APIを用いなければならなくなったため、  
既存のv1の実装手順は使えない。  
公式リファレンス通りにエレメントの生成をして要素にマウントする方法をベースとして、
フォームの装飾などについても公式リファレンスに従う必要がある。

# 配列、多次元配列と連想配列について
### 2023/03/14
配列は変数の箱が連なるようなイメージでそれぞれの箱を要素と呼ぶ。  
要素には添字(index)と呼ばれる番号が自動的に割り当てられ、  
indexを指定することで配列の要素の値を取り出すことができる。  
  
多次元配列は配列の中にさらに配列が入っているもの。  
イメージとしては住所。  
国という配列があり、その中に県などの情報が入っていて、  
更に県の中には市、市の中には番地が入っているといった具合。  

連想配列はハッシュなどと呼ばれるもので、  
データとそれに対応する名前をセットにして一つの要素として扱う。  
配列との違いは順番ではなく、キーで管理する点。  
データをバリュー、対応する名前をキーとして管理。  
このようなキーとバリューで管理する方式をキーバリューストアという。



# integer float doubleの違いについて
### 2023/03/14
integerは比較的大きな整数を扱う型で、９桁ないし１０桁の精度で整数を扱うことができます。  
floatは単精度浮動小数点数、doubleは倍精度浮動小数点数と呼ばれ、  
共に小数点以下の値を扱うことのできる形ですが、扱える値の範囲に差があります。  
doubleはfloatの倍の範囲を扱うことができます。

# MVCの具体的な役割について
### 2023/03/13
Mはモデルの略です。  
役割としてはデータベースのデータのやり取りを処理します。  
クライアントのリクエストをコントローラーが制御してモデルにデータの取り出しを指示します。  
その指示に基づいてデータベースからデータを取り出しコントローラーに送信します。
また、データを保存する命令が送られてきた際には、送られてきたパラメータ内の値を保存して良いのかを判断する役割も担います。  
バリデーションなどのビジネスロジックの記述はモデル内に行います。  
  
Vはビューの略です。  
役割としてはクライアントが実際に目にする画面の処理を行います。  
レイアウトやメニュー、操作性などの視覚的な部分の責任を負います。  
  
Cはコントローラーの略です。  
役割としてはモデルとビューの制御や橋渡しを行います。  
ルーティングされたリクエストを受け取り、内容に基づきモデルにデータの取り出しを指示。  
それを受け取りビューに送ります。  
Ruby on Railsでは通常はApplicationControllerを継承したコントローラに記述を行っていきます。  

# オブジェクト指向って？
### 2023/03/10
オブジェクト指向プログラミングは役割をもったモノごとにクラスを分割して
その関係性を定義し組み合わせることでシステムを作り上げる考え方。
同じ記述を何度も書く必要がなくなり、開発時間の短縮やオブジェクトを分割することでチーム開発がしやすくなるなどのメリットもある。

# Actiove Record
### 2023/03/10

ActiveRecordは簡単にいってしまえばRubyとSQLの翻訳機である。
基本的にDBを操作するのであればSQLが必要であるが、
ActiveRecordが間に入り、ActiveRecord由来のメソッドを記述することで
比較的簡単にDBの操作を行うことができる。
また、どのSQLを使っても記述を統一することができるというメリットもある。


# ２つのテーブルのFKに一致する特定のレコードの取り出し方
### 2023/02/27

購入者が商品をダウンロードできるといった分岐を作りたい時には、  
コントローラー側で外部キーの整合性がとれるレコードを拾ってくる必要がある。  

```ruby
def show
    @product.find(params[:id]
    products = @product.id # 現在の商品ページのidを取得
    user = current_user.id # 現在のユーザーのidを取得

    # 上記二つを取得した上で外部キーでレコードを絞り込む
    @order = Order.where(user_id: user).where(product_id: products)
end
```

このような記述にしビュー側で. 

```ruby
if @order.present?
```

と記述するとユーザーがその商品を購入したか否かを判定できる。  

```ruby
@user.order.user_id
@product.order.product_id
```
このようなビューの記述だとレコードが絞れないor参照する情報が違うので分岐に失敗する。  
外部キーを意識することがいかに大切かがわかる。  
ちなみに、  

```ruby
@order = Order.where("user_id = user").where("product_id = products")
```

みたいな記述でもいけるが、  
SQLインジェクションのリスクが上がるので、  
シンボルの使用推奨。



# 配列内の重複しない値のみを足すプログラム
### 2023/02/21

ダウンロードリンク作成の時には
link_toではなくbutton_to!!

link_toだとうまく動作しないので注意

# 配列内の重複しない値のみを足すプログラム
### 2023/02/16

````ruby
def lone_sum(ary)
  newary = ary.uniq
  puts newary.sum
end

lone_sum([1, 3, 3])

````

メソッドを使わない場合は　　

①配列空の配列を新たに作る　　

②中身入りの配列.each do　で中身が重複しない場合に空の配列に値を入れていく記述を行う　　

③新たに作った配列.each doの中で配列の数だけ足し算を行う記述をする　　

上記記述を行うと２０行オーバーの記述になってしまう。

# 2つの文字列の末尾の文字を比較(大文字小文字区別なし)して、一致する場合はTrue、一致しない場合はFalseになるというプログラム
### 2023/02/14
初めは変数に代入して比較したりしていたが、結局のところ下記の処理に落ち着いた。  
下記の記述が試した中では一番圧縮できた記述。

```` ruby
# メソッドを定義
def end_other(a, b)
# 各文字列をsliceメソッドで後ろ3文字取り出してdowncaseメソッドで小文字に変換。
# さらに条件式を作り値の比較を行い分岐させる。
  if (a.slice(-3,3)).downcase == (b.slice(-3,3)).downcase  
    puts "ture"
  else
    puts "false"
  end
end

end_other('HiabC', 'abc')
````

# ActiveStrageで複数画像を扱うときのための記述
### 2023/02/13

モデル内のアソシエーションの記述
has_one_attached :image
から
has_many_attached :imagesに変える
モデルのバリデーションの記述を複数形に
ビューファイルのフォームのname属性を複数形にして配列[]を作る
ストロングパラメーターは複数形にして配列[]を許可
ビューのイメージタグでimages[0]で配列を取得するようにする
jsファイル内の記述も複数形の取得や配列の指定などを行う。今後２枚目以降を表示するためにdataで番号で管理を行う。
投稿枚数を制限するためにはフロント（js）とサーバー（モデル）で制限をかける必要がある


# プレビュー機能の作成手順
### 2023/02/12

① 投稿機能に対応するjsファイルの作成、application,jsにrequire記述

② HTML読み込み後にJavaScriptが動くように記述
    投稿フォームを切り出している場合はaddEventListener('load')ではなくaddEventListener('DOMContentLoaded')
    切り出した場合にはフォームのidを取得したのちif文で(!定数) return null;と記述し切り出したフォームが読み込まれた場合のみにJavaScriptが動くようにする

③ ビューファイルにプレビューを表示するための　divでブロックレベル要素を作成してidを記述  div id=""

④ jsで上記プレビューのidを取得

⑤ input要素を取得して変化した際に呼び出される関数を定義

    document.querySelector('input[type="file"][name="○○[image]"]')
    addEventListener('change', function(e))
    
⑥ 上記イベント内で画像を取得する

    const file = e.target.files[0];
    
⑦ createObjectURL()メソッドで取得した画像情報のURLを生成

    const blob = window.URL.createObjectURL(file);

⑧ createElement()メソッドを使いdiv要素と表示画像を生成する
    
    const previewWrapper = document.createElement('div');
    previewWrapper.setAttribute('class', 'preview');

    const previewImage = document.createElement('img');
    previewImage.setAttribute('class', 'preview-image');
    
⑨ img要素の属性に変数blobを設定する

    previewImage.setAttribute('src', blob);
    
⑩ appendChild()メソッドで要素を入れ子構造にしていく
    
    previewWrapper.appendChild(previewImage);
   　　プレビューを表示する要素を取得した変数.appendChild(previewWrapper);

最後に　古いプレビューを削除する記述を行う

  ⑧の２行目で作ったpreviewクラスをquerySelectorで取得して変数に入れる。
  その後変数が存在する(true)場合にremove();
  ※changeイベント後すぐに発火

# わかりやすいActiveRecordメソッド
## new,all,findの違いを明確に！
### 2023/02/10

new,all,findはデータベースのデータをインスタンスかするコマンドという共通点がある。
ではその明確な違いは何か？
- newはテーブルそのものをインスタンス化する。
- allはテーブル内に存在するレコードを全てインスタンス化する。
- findは特定のレコードをインスタンス化する。


文章だとわかりにくいのでテーブル形式の図で。
````ruby
@sample = Sample.new

````
テーブル自体をインスタンス化するので、空のレコードも拾ってくるイメージ
| id | content | comment |
| ---| ------- | ------- |
| 1  |   内容１  | コメント１ |
| 2  |   内容２  | コメント２ |
| 3  |   内容３  | コメント３ |
|    |         |         |
|    |         |         |

````ruby
@sample = Sample.all
````
存在するレコードをインスタンス化するので空のレコードは存在しないイメージ
| id | content | comment |
| ---| ------- | ------- |
| 1  |   内容１  | コメント１ |
| 2  |   内容２  | コメント２ |
| 3  |   内容３  | コメント３ |

````ruby
@sample = Sample.find(1)
````
指定したレコードをインスタンス化するのでもちろん空のレコードは存在しない
| id | content | comment |
| ---| ------- | ------- |
| 1  |   内容１  | コメント１ |


# nilを使う際の注意点
### 2023/02/09

nilは何も存在しないこと。  
例えば
````ruby
if sample == nil
````
の様にするとsample自体が存在するか否かを判定する。  
sampleに中身が存在するかを判定したい場合には
````ruby
if sample[0] == nil
````
といった記述にしなければならない。

ただ、冗長な記述になる可能性があるので  
中身があるかないかを判定したいだけならば、
````ruby
if sample.present?
````
とするのがベター。
