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
