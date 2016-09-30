# RailsベースのJavaScript開発ガイドライン

## 索引
<!-- 以下は https://github.com/hail2u/node-gfmtoc 用の記法 -->
<!-- #toc -->

* [索引](#%E7%B4%A2%E5%BC%95)
* [はじめに](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)
* [JavaScriptを使った機能の開発手順](#javascript%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F%E6%A9%9F%E8%83%BD%E3%81%AE%E9%96%8B%E7%99%BA%E6%89%8B%E9%A0%86)
  * [0. 前提](#0-%E5%89%8D%E6%8F%90)
  * [1. エントリポイントの設置](#1-%E3%82%A8%E3%83%B3%E3%83%88%E3%83%AA%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%81%AE%E8%A8%AD%E7%BD%AE)
  * [2. エントリポイントの読み込み](#2-%E3%82%A8%E3%83%B3%E3%83%88%E3%83%AA%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%81%AE%E8%AA%AD%E3%81%BF%E8%BE%BC%E3%81%BF)
  * [3. セキュリティを担保する](#3-%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%92%E6%8B%85%E4%BF%9D%E3%81%99%E3%82%8B)
    * [$el.html()でXSSを発生させない](#elhtml%E3%81%A7xss%E3%82%92%E7%99%BA%E7%94%9F%E3%81%95%E3%81%9B%E3%81%AA%E3%81%84)
    * [テンプレート変数をJS変数として展開する際はto_json.html_safeを使う](#%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E5%A4%89%E6%95%B0%E3%82%92js%E5%A4%89%E6%95%B0%E3%81%A8%E3%81%97%E3%81%A6%E5%B1%95%E9%96%8B%E3%81%99%E3%82%8B%E9%9A%9B%E3%81%AFto_jsonhtml_safe%E3%82%92%E4%BD%BF%E3%81%86)
  * [4. 以上](#4-%E4%BB%A5%E4%B8%8A)
* [FAQ](#faq)
  * [MFApp グローバル変数はどのように定義されているのですか？](#mfapp-%E3%82%B0%E3%83%AD%E3%83%BC%E3%83%90%E3%83%AB%E5%A4%89%E6%95%B0%E3%81%AF%E3%81%A9%E3%81%AE%E3%82%88%E3%81%86%E3%81%AB%E5%AE%9A%E7%BE%A9%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%82%8B%E3%81%AE%E3%81%A7%E3%81%99%E3%81%8B)
  * [何故エントリポイントにアクション単位の if 文を設定するのですか？](#%E4%BD%95%E6%95%85%E3%82%A8%E3%83%B3%E3%83%88%E3%83%AA%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%81%AB%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E5%8D%98%E4%BD%8D%E3%81%AE-if-%E6%96%87%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B%E3%81%AE%E3%81%A7%E3%81%99%E3%81%8B)
  * [$ -> とは何ですか？](#---%E3%81%A8%E3%81%AF%E4%BD%95%E3%81%A7%E3%81%99%E3%81%8B)
  * [エントリポイントの書式は固定ですか？](#%E3%82%A8%E3%83%B3%E3%83%88%E3%83%AA%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%81%AE%E6%9B%B8%E5%BC%8F%E3%81%AF%E5%9B%BA%E5%AE%9A%E3%81%A7%E3%81%99%E3%81%8B)
  * [gem化されていないサードパーティ製ライブラリを配置する方法は？](#gem%E5%8C%96%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%81%AA%E3%81%84%E3%82%B5%E3%83%BC%E3%83%89%E3%83%91%E3%83%BC%E3%83%86%E3%82%A3%E8%A3%BD%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E3%82%92%E9%85%8D%E7%BD%AE%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%E3%81%AF)
  * [内製のものもサードパーティ製ライブラリとして置くことはできますか？](#%E5%86%85%E8%A3%BD%E3%81%AE%E3%82%82%E3%81%AE%E3%82%82%E3%82%B5%E3%83%BC%E3%83%89%E3%83%91%E3%83%BC%E3%83%86%E3%82%A3%E8%A3%BD%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E3%81%A8%E3%81%97%E3%81%A6%E7%BD%AE%E3%81%8F%E3%81%93%E3%81%A8%E3%81%AF%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%81%8B)
  * [コントローラをまたぐ共通処理を定義したい](#%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD%E3%83%BC%E3%83%A9%E3%82%92%E3%81%BE%E3%81%9F%E3%81%90%E5%85%B1%E9%80%9A%E5%87%A6%E7%90%86%E3%82%92%E5%AE%9A%E7%BE%A9%E3%81%97%E3%81%9F%E3%81%84)
  * [あるコントローラ内でのみ使う共通処理を定義したい](#%E3%81%82%E3%82%8B%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD%E3%83%BC%E3%83%A9%E5%86%85%E3%81%A7%E3%81%AE%E3%81%BF%E4%BD%BF%E3%81%86%E5%85%B1%E9%80%9A%E5%87%A6%E7%90%86%E3%82%92%E5%AE%9A%E7%BE%A9%E3%81%97%E3%81%9F%E3%81%84)
  * [javascript_include_tag を使ってはいけませんか？](#javascript_include_tag-%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%AF%E3%81%84%E3%81%91%E3%81%BE%E3%81%9B%E3%82%93%E3%81%8B)
  * [javascript_include_tag が非推奨なのは何故ですか？](#javascript_include_tag-%E3%81%8C%E9%9D%9E%E6%8E%A8%E5%A5%A8%E3%81%AA%E3%81%AE%E3%81%AF%E4%BD%95%E6%95%85%E3%81%A7%E3%81%99%E3%81%8B)
  * [あるページでRailsからJSへ値を渡したい](#%E3%81%82%E3%82%8B%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%A7rails%E3%81%8B%E3%82%89js%E3%81%B8%E5%80%A4%E3%82%92%E6%B8%A1%E3%81%97%E3%81%9F%E3%81%84)
  * [既にJavaScriptで書いているコードはCoffeeScriptに直すべきですか？](#%E6%97%A2%E3%81%ABjavascript%E3%81%A7%E6%9B%B8%E3%81%84%E3%81%A6%E3%81%84%E3%82%8B%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AFcoffeescript%E3%81%AB%E7%9B%B4%E3%81%99%E3%81%B9%E3%81%8D%E3%81%A7%E3%81%99%E3%81%8B)
  * [`class @FooClass` と書くコードをよく見るのですが](#class-fooclass-%E3%81%A8%E6%9B%B8%E3%81%8F%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E3%82%88%E3%81%8F%E8%A6%8B%E3%82%8B%E3%81%AE%E3%81%A7%E3%81%99%E3%81%8C)
  * [DOM要素の有無で条件分岐をしないのは何故ですか？](#dom%E8%A6%81%E7%B4%A0%E3%81%AE%E6%9C%89%E7%84%A1%E3%81%A7%E6%9D%A1%E4%BB%B6%E5%88%86%E5%B2%90%E3%82%92%E3%81%97%E3%81%AA%E3%81%84%E3%81%AE%E3%81%AF%E4%BD%95%E6%95%85%E3%81%A7%E3%81%99%E3%81%8B)
  * [何故テンプレートではなくアクションと1対1なのですか？](#%E4%BD%95%E6%95%85%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%A7%E3%81%AF%E3%81%AA%E3%81%8F%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A81%E5%AF%BE1%E3%81%AA%E3%81%AE%E3%81%A7%E3%81%99%E3%81%8B)
* [License](#license)

<!-- /toc -->

## はじめに
当ドキュメントは、主に Rails エンジニアに向けた、Rails 内で JavaScript による機能開発を行うためのガイドラインです。

JavaScript や CoffeeScript コードの差分を含むコミットを行う方は、
先頭セクションの [JavaScript を使った機能の開発手順](#javascript%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F%E6%A9%9F%E8%83%BD%E3%81%AE%E9%96%8B%E7%99%BA%E6%89%8B%E9%A0%86) を読み、
可能な限りその手順を守ってください。  
次セクションの [FAQ](#faq) は、困ったことがあったら参照する程度で構いません。

ここに記載されていない状況に遭遇して困った場合は、気軽に相談して下さい。

--
:memo: 当ドキュメントにおける `"JavaScript"` という単語は、しばしば CoffeeScript をも含めることがあります。


## JavaScriptを使った機能の開発手順
### 0. 前提
以下の方法は、主に新規開発を想定した手順を記しています。

新規ではない場合、既存部分の影響で記載通りに設置できない場合があると思います。  
その場合は、可能な範囲で対応をお願いします。

### 1. エントリポイントの設置
例えば、Rails 側に `UsersController#index` というアクションがあったとします。

そのアクション用に JS のエントリポイントを設置したい場合、
まずは `app/assets/javascripts/users/index.js.coffee` へ以下の様な CoffeeScript ファイルを設置します。

```coffee
# MFApp.controller と MFApp.action に
# Rails 側の controller_path と action_name 情報が格納されているので、
# その値で if 文を書く。
if MFApp.controller == 'users' && MFApp.action == 'index'
  $ ->
    # ビジネスロジックはココから書く
    $('.js-foo').on 'click', ->
```

`turbolinks` を On にしている場合は以下です。

```coffee
$ ->
  if MFApp.controller == 'users' && MFApp.action == 'index'
    # do stuff
```

置きましたか？　お疲れ様です。

さて、ブラウザから動かしてみると、今設置したファイルは実行されているでしょうか？  
もし不明なら、if ブロックの 1 行目やファイル先頭行に `console.log` を書くなどして検証するといいでしょう。

ちゃんと動いているなら、ここで完了です！

もし動いていないなら、そのファイルはまだ読み込まれていないので設定が必要です。  
次の項目を参照願います。

--
- :memo: `MFApp` グローバル変数については、後に FAQ セクションで解説します。現在はその変数が定義されているのを前提として読み進めて下さい。
- :memo: 以降の本ガイドラインの文章では、`turbolinks` は Off である前提で記述します。

### 2. エントリポイントの読み込み
以下の手順で、先程追加したファイルを読み込む設定を行います。

- `app/assets/javascripts/application.js` を開く
- `//= require 'users/index'` の行を追加する
  - 追加する場所は、同じようにエントリポイントのファイル読み込みが並んでいる箇所です
    - 依存関係はディレクトリの構造に従います。同階層間の並び順に決まりはありません
    - `require_self` が使われている場合は、必ずそれよりも下の位置にして下さい

--
また、もし、エントリポイントからモジュールを別ファイルへ分離するという設計を行った場合、ファイル間に依存関係が生じることになります。
その場合は、読み込み順が依存関係を解決するように、直列に配置してください。

```js
//= require 'users/UserModel'
//= require 'users/index'
```

これで読み込みは完了し、「コードを書けば動く」状態になったと思います。  
次は実装内容についての話です。

--
- :exclamation: `require_tree` はファイルの読み込み順が Sprokets の実装依存になるため、ファイル間に依存関係が生じる場合は**使ってはいけません**。
  - 既に使われていた場合は、依存関係が生じた時点で `require` による読み込みへ書き直して下さい。
    - なお、本家の [README](https://github.com/rails/sprockets) でも、同様に `require` で分解する解決法が推奨されています。
- :exclamation: テンプレートに `javascript_include_tag` を書いて直接読み込む手段は、禁止です。

### 3. セキュリティを担保する
実装内容について・・・とは言っても、基本的には自由です。  
ただし、最低限セキュリティに配慮する必要があり、以下の例示した実装に関しては注意願います。

#### $el.html()でXSSを発生させない
例えば、以下のコードは、いわゆる HTML 特殊文字のエスケープを行っていないため、XSS を成立させる危険性があります。
```coffee
foo = 'サーバから取得した文字列'
$el.html("<div>#{ foo }</div>")
```

Rails 側では Slim や Haml などのテンプレーティングエンジンがその問題を暗黙的に処理してくれますが、今のところ JS 側にはそういった共通の仕組みは用意していません。  
そのため、Rails でいうところの `html_escape` のようなアプローチにより、テンプレート変数を個別に解決する必要があります。

```coffee
foo = 'サーバから取得した文字列'
$el.html("<div>#{ エスケープする関数(foo) }</div>")
```

「エスケープする関数」には、大体のプロジェクトには [Lodash](https://lodash.com/) や [Underscore.js](http://underscorejs.org/) がインストールされているので、`_.escape` を使って下さい。

--
なお、Rails 側で生成した安全な HTML 文字列をそのまま使うようなパターンの場合は、エスケープの責務は Rails 側にあるので、JS 側では対応しない方が良いです。

```coffee
htmlString = '<div>例えば HTML を生成する Web-API を叩いた結果など</div>'
$el.html(htmlString)
```

#### テンプレート変数をJS変数として展開する際はto_json.html_safeを使う
まず、良い例です。  
`to_json` で JS ソースコードのサブセットである JSON 形式の文字列に変換し、JS リテラルとして埋め込みます。

```slim
javascript:
  var foo = #{foo.to_json.html_safe};
  var obj = {
    foo: #{foo.to_json.html_safe}
  };
  var ary = [#{foo.to_json.html_safe}];
```

--
悪い例 その1 です。  
`foo` に JS 文字列リテラルの一部であるダブルクォートが含まれていた場合、構文エラーになります。

```slim
javascript:
  var foo = "#{foo.html_safe}";
```

--
悪い例 その2 です。  
ダブルクォートやシングルクォートが含まれていても構文エラーを発生しなくなりましたが、`</script>` という文字列により問題が発生する可能性が残ります。

```slim
javascript:
  var foo = "#{escape_javascript(foo.html_safe)}";
  var foo2 = "#{j(foo.html_safe)}";
```

:memo: `</script>` 問題については、お手数ですが [コチラ](http://qiita.com/kjirou/items/74906b8ad43c72b63a02) の外部資料を確認して下さい。

--
悪い例 その3 です。  
テンプレーティングエンジン側で HTML 特殊文字の変換を行うことで、結果的にクォートの問題や `</script>` 問題は回避されました。

```slim
javascript:
  var foo = "#{foo}";
```

しかし、変換後の値は、おそらくは JS 側が期待した値では無いでしょう。

```slim
javascript:
  var foo = "#{'M&M'}";
  console.log(foo);  // -> "M&amp;M"
```

### 4. 以上
守らねばいけないルールは、以上です。


## FAQ
### MFApp グローバル変数はどのように定義されているのですか？
ビューのレイアウトファイルの先頭などで、以下の JS コードを実行して定義しています。

```slim
javascript:
  window.MFApp = {
    controller: #{params[:controller].to_json.html_safe},
    action: #{params[:action].to_json.html_safe}
  };
```

Rails 側の `ApplicationController` の影響下に存在することを想定しており、
その他の共通でサーバ側から JS 側へ渡したい値を含めることも許容しています。

### 何故エントリポイントにアクション単位の if 文を設定するのですか？
この箇所が何故付与されているかの解説です。

```
if MFApp.controller == 'users' && MFApp.action == 'index'
```

まず Rails は、`require` で読み込んだ全ての JS や Coffee のソースコードを変換しつつ結合し、1 つの `application.js` という巨大な JS ファイルへ加工して、それを全画面で配信します。  
つまり、**どの画面でも全ての JS ソースコードを読み込む**、という仕組みを採用しています。  
そのため、手なりで JS コードを書くと全画面で実行されてしまいます。

それを、Rails 側のエントリポイントである「アクション」という単位で管理できるようにすることで、
一回の URL アクセスによって実行されるコード範囲や副作用の影響を絞ることを目的としています。

--
:memo: 同じ問題について、body タグの class 属性で指定するなどで解決する方法もあります。
その方法で解決しなかったのは以下の点で優れていると判断したからです。

- DOM に依存しないので、ユニットテストを行う際などに、DOM 全体をモックする必要が無くなる
- 定義するタイミングが DOM の展開を待たないため早い。`$()` 実行前に存在できる

### $ -> とは何ですか？
呪文のように記述されている`$ ->` が何かという点の解説です。

```coffee
if MFApp.controller == 'users' && MFApp.action == 'index'
  $ ->
```

まず、`$ ->` を JavaScript に訳すとこうなります。

```js
// コールバック関数を登録している
$(function() {
});
```

この `$(callback)` は jQuery が提供する機能で、「`callback` に登録した処理を、全てのHTMLページがロードされたら実行する」という**予約**をしています。  
つまり、**callbackはこの時は実行されません**。

では、いつ実行されるのかと言うと、あくまでイメージですが HTML 描画後の以下の位置で実行される感じです。

```html
<html>
<body>
  <main>My Awesome Contents!</main>

  <script>
    // HTML 描画後
    callback();
  </script>
</body>
</html>
```

もし、`$ ->` の下に入れずにこう書いてみた場合、

```coffee
if MFApp.controller == 'users' && MFApp.action == 'index'
  # $ -> の下に入れずに直接ロジックを書く
  $('main').on 'click', ->
    # do stuff
```

先程のイメージを引き合いにすると、HTML 描画前に実行されることになり、

```html
<html>
<head>
  <script>
    // HTML 描画前
    callback();
  </script>
</head>
<body>
  <main>My Awesome Contents!</main>
</body>
</html>
```

上記例の場合、ランタイムエラーこそ発生しませんが、`main` へのイベントは付与されません。

### エントリポイントの書式は固定ですか？
いいえ。

上記までの内容を理解しているなら、個人の裁量で自由に定義できます。

例えば、このようなアレンジも可です。

```coffee
onReady = ->
  # do stuff

if MFApp.controller == 'users' && MFApp.action == 'index'
  $(onReady);
```

### gem化されていないサードパーティ製ライブラリを配置する方法は？
プロジェクトによって異なりますが、

- `app/assets/javascripts/libraries`
- `app/assets/javascripts/vendor`

などの所定の位置にそのファイルをコピーし、`application.js` で `require` を定義して下さい。  

CSS や画像がセットの場合は、それぞれ `app/assets/stylesheets` や `app/assets/images` に振り分けて下さい。

### 内製のものもサードパーティ製ライブラリとして置くことはできますか？
内製や自作ライブラリであっても、プロジェクトに依存せずに動作するものならここに置いても良いです。

ただし、「サードパーティ製ライブラリ」というものに対しては、
「特定環境に依存しない仕様を持っている」という以外に「高い品質である」ことを期待するものでもあります。

「高い品質」の正確な定義はありませんが、以下にとりあえずは思いついた範囲で要件を例示列挙しておきます。

- テストを書くなどの、安定性を担保するための何らか施策を別途行っている
- モジュール読み込み時には、可能な限り実行環境に影響を与えない。例えば以下のようなこと、
  - グローバルな名前空間への影響は、1 つの変数を定義するのみに留める
  - `setTimeout` や `setInterval` などのタイマーを実行しない
  - DOM を操作しない
  - 外部通信をしない
  - `alert`, `confirm`, `prompt` を実行しない
- 依存する他ライブラリがある場合は、例外で明示しつつ終了させたり、READMEやコメントなどのドキュメントで明示する

### コントローラをまたぐ共通処理を定義したい
コントローラをまたぐ複数のエントリポイントから使用する共通処理を定義する手順です。

--
例えば、こういうライブラリを作りたかったとします。

```coffee
emojiManager =
  searchEmoji: (id) ->
    # do stuff
```

--
ファイルは `shared` 以下に配置します。  
今回の例だと、`app/assets/javascripts/shared/emoji-manager.js.coffee` になるでしょう。

そして、`{プロジェクト別のグローバル変数}.Shared` 以下に追加するコードを付与します。

```coffee
emojiManager =
  searchEmoji: (id) ->
    # do stuff

# 追加する
MyApp.Shared.emojiManager = emojiManager
```

--
それを各エントリポイント内から呼び出す場合は、この様に記述します。

```coffee
MyApp.Shared.emojiManager.searchEmoji('ok_woman')

# import 風に、ファイル先頭などで別変数に移動しても良いです。
emojiManager = MyApp.Shared.emojiManager
emojiManager.searchEmoji('ok_woman')
```

--
サードパーティ製ライブラリとして配置する場合との違いは、「プロジェクト専用であること」を前提にできることです。  
具体的には、以下のような点が変わります。

- `MFApp` グローバル変数や、上記例だと `MyApp` と書いたプロジェクト用グローバル変数に依存できる
- 特定の URL や、特定の HTML の出力内容に依存できる

--
:memo: プロジェクト別のグローバル変数は、プロジェクト毎に `application.js` の先頭部分で、概ね以下のようなシンプルな定義で生成されています。

```coffee
window.MyApp =
  Controllers: {}
  Shared: {}
```

### あるコントローラ内でのみ使う共通処理を定義したい
`require` を使って個別にファイルを読み込み、コントローラで共通で使うファイルを先に読み込むことで表現することができます。

以下「`users` コントローラ全体で共有する `UserManager` というクラスを定義したい」というストーリーで、例を書きます。

`application.js`:
```js
//= require users/UserManager
//= require users/index
```

`users/UserManager.js.coffee`:
```coffee
class UserManager
  foo: ->
  bar: ->

# CaApp.Controllers.コントローラ名 に名前空間を作る
#
# 独立しているモジュール同士の読み込み順を自由にしたいなら
# ?= 演算子（Rubyの ||= 相当）で代入してもいい
CaApp.Controllers.users = {}
CaApp.Controllers.users.UserManager = UserManager
```

`users/index.js.coffee`:
```coffee
if MFApp.controller == 'users' && MFApp.action == 'index'
  $ ->
    userManager = new CaApp.Controllers.users.UserManager()
    # do stuff
```

### javascript_include_tag を使ってはいけませんか？
限りなく禁止に近い非推奨です。可能な限り使わないで下さい。

### javascript_include_tag が非推奨なのは何故ですか？
一番の理由は、「1 枚のファイルにする以外にモジュール同士の依存関係を管理する方法が無いから」というものです。

例えば、A モジュールに依存する B モジュールを定義したい場合に、ページ別に A が存在したりしなかったりすると困ります。
だから、常に A が存在する前提にしてしまおうという意図です。

Ruby は勿論、その他の多くのプログラミング言語（Node.js 含む）では、モジュール管理機構を内包しています。  
しかし、ブラウザ内の JavaScript 実行環境には現状は存在しないため、このような歪な対応が必要になってしまいます。

ちなみに、`browserify` や `webpack` などのいわゆる「モダン」な JS 開発手法を介す場合でも、最終的に「全ページで 1 枚」という結論はよく見ます。
（いや、そこに至るまでは段違いなのですが）

ただし、圧縮しても数MBあるようなライブラリや、読み込み時点で副作用があるサードパーティ製ライブラリを使う場合など、別段の事情がある場合はこの限りではありません。

### あるページでRailsからJSへ値を渡したい
描画内で、このように 1 つのグローバル変数へ格納して渡したり、

```slim
javascript:
  window.templareVars = {
    foopoints: #{@foo.points.to_json.html_safe},
    bars: #{@bars.to_json.html_safe}
  };
```

または、Rails の `hidden_field_tag` などを使い DOM を介して渡して下さい。

### 既にJavaScriptで書いているコードはCoffeeScriptに直すべきですか？
いいえ、わざわざそのためだけに書き直す必要はありません。

ただし、`.js` だと以下のようにグローバルに変数を撒き散らしてしまいがちな点は認識しておいて下さい。

```coffee
# この foo のスコープは、このファイル内のみ
foo = 1
```

```js
// この foo のスコープはグローバル変数
var foo = 1;
// こう書いているのと同等
window.foo = 1;
```

なお、もし後者を簡単に解決したい場合は、以下のように書きます。

```js
(function() {
  var foo = 1;
})();
```

### `class @FooClass` と書くコードをよく見るのですが
このコードは、

```coffee
class @FooClass
```

概ねこのような JS を書いているのと同じです。

```js
// class が function になっている点は、主題から外れるのでスルーして下さい
window.FooClass = function() {};
```

Rails デフォルト設定の CoffeeScript は、ファイル単位で閉じた変数スコープを生成します。  
その状況下でファイル間連携をするために、グローバルスコープにベタ置きしているだけです。

つまり、保守性に難があるコードで、本ガイドライン下では禁止にあたる書き方です。  
おそらくは、**見た目だけがスマート**という理由で広まったのでは無いでしょうか？（要出典）

### DOM要素の有無で条件分岐をしないのは何故ですか？
後で書く

### 何故テンプレートではなくアクションと1対1なのですか？
後で書く


## License
[CC-BY 3.0](https://creativecommons.org/licenses/by/3.0/)
