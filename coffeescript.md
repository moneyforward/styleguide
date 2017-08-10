# CoffeeScript Coding Style Guide

## はじめに

- 細かいスタイルについてはCoffeeLintの設定ファイル（coffeelint.json）に従う
- 実装上不都合が出てきた場合、適宜判断し改修を行う（issueをお願いします）
- これらのガイドを参考にしています
  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)([日本語](http://cou929.nu/data/google_javascript_style_guide/))
  - [Cookpad | Cookpad CoffeeScript Coding Style Guide](https://github.com/cookpad/styleguide/blob/master/coffeescript.ja.md)
  - [GitHub | Coding Style](https://github.com/styleguide/javascript)

## ルール
### ファイル名 / ファイルパス

- 拡張子は `.js.coffee`

### インデント / 空白 / セミコロン

- 2 スペースとする
- 演算子の前後には空白を1つずつ空ける
- `,` の後には 1 つ空白を空ける
- セミコロンは必然性がない限り省略する

### 文字数 / コメント

- ソースコードの 1 行は半角 120 文字以内とする。エラーメッセージ文言などはその限りではない。

### 文字列 / 配列 / ハッシュ

- 文字列リテラルの宣言には、基本的にシングルクォートを使用する。変数を埋め込む場合はダブルクォートを使う。
- ハッシュを一行で記述する場合、`{}` の前後に1つ空白を空ける。例： ```{ foo: bar }```

### 演算子

- 以下の演算子を使用を推奨する
  - `&&, ||, ==, !=`
- 以下のエイリアスは非推奨とする
  - `and, or, is, isnt`

### 変数名

- 変数名は camelCase を用いる
- クラス名は PascalCase を用いる
- 定数は全て大文字で SNAKE_CASE を用いる
- 命名に困った場合は、無理に省略した名前にするよりは、冗長で説明的でも良いので正確さを重視した命名を行う

### 関数

- `->` の前後にはスペースを空けること
- 引数がない場合、括弧は省略すること
- メソッド呼び出しの括弧は複数行の場合、省略する。一行の場合、省略しないこと
- `this` の保持には `=>` を使用すること
- 必要性のない `=>` (Fat Arrow) は使用しないこと
- 理由がない限り `this` は `@` を使用すること

### クラス

- private のプロパティは `_` プレフィックスを付与して表現すること

### その他

- JavaScript埋め込みは使用しないこと
- JavaScript / CoffeeScriptからDOMにアクセスする場合、そのDOMのクラスに `js-` プレフィックスを付与すること
  - 例外的に `is-` プレフィックスは許容されている
- JavaScript / CoffeeScriptからスタイルの更新を行う場合、スタイルのプロパティを変更するのではなくクラスの付け替えで行うこと
