# セマンティック Web とは

セマンティック Web とは、Web ページに記述された内容について、それが何を意味するかを表す「情報についての情報」（メタデータ）を一定の規則に従って付加し、コンピュータシステムによる自律的な情報の収集や加工を可能にする構想。
1998 年に W3C のティム・バーナーズ=リー氏が提供しました。

下記の 2 つに分類され、現時点では Google が推奨していることもあり、Schema.org と JSON-LD の組み合わせがよく利用されているようです。

- creator などの **意味・語彙・スキーマ** を定義するもの
  - 例えば、<div>山田太郎</div> というデータに対して、<div property="creator">山田太郎</div>
  - 意味・語彙・スキーマとしては、Schema.org, ダブリン・コア などが利用されます。
  - ボキャブラリーとはクローラが構造化データを認識するための鍵、分類で、「地名」「人名」「食べ物」……といったような、「在り方」そのもののことです。
    - https://schema.org/Thing
- 定義されたものをどの様に表現するかの **記法・文法・言語**
  - シンタックス[記法/文法...]とはクローラへ構造化データを渡すための書き方のことです。
  - 記法・文法・言語としては、RDFa, microdata, JSON-LD などが利用されます。
    - Microdata（マイクロデータ）：HTML にメタデータを直接記述する方式
    - JSON-LD（ジェイソン・エルディ）：JavaScript を使ってページ内に挿入する方式

HTML に関して簡単に言うと「『HTML のタグ』でわかりやすくするというのが『セマンティックウェブ』」。
https://developer.mozilla.org/ja/docs/Glossary/Semantics#html_%E3%81%A7%E3%81%AE%E3%82%BB%E3%83%9E%E3%83%B3%E3%83%86%E3%82%A3%E3%82%AF%E3%82%B9

### 恩恵は？？

- 検索エンジン最適化、クローラーが効率的に認識してくれる
  - 検索エンジンに単なる文字列としてテキストを認識させるのではなく、その文字の意味や、文脈、背景などまで理解させようとする考え方...と書いているところもある
- アクセシビリティ向上

# 構造化データと言うところに絞ると... with google

## 下記のようなツールで有効性をチェックできたりする

https://developers.google.com/search/docs/advanced/structured-data
https://developers.google.com/search/docs/advanced/structured-data/product?hl=ja#json-ld_2

## 構造化データの仕組みと表示方法 by google

https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data?hl=ja

# 参照

https://qiita.com/fabulousy1109/items/b4b4e8aeabdc574754f4
https://developer.mozilla.org/ja/docs/Glossary/Semantics
