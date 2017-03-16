# CoffeeScript Coding Style Guide

- [はじめに](#はじめに)
- [ファイル名 / ファイルパス](#ファイル名--ファイルパス)
- [インデント / 空白 / セミコロン](#インデント--空白--セミコロン)
- [文字数](#文字数--コメント)
- [文字列 / 配列 / ハッシュ](#文字列--配列--ハッシュ)
- [演算子](#演算子)
- [変数名](#変数名)
- [関数](#関数)
- [その他推奨・非推奨事項](#その他推奨非推奨事項)
- [実装に当たって](#実装に当たって)
- [関連資料](#関連資料)

## はじめに

  - 細かいスタイルについてはCoffeeLintの設定ファイル（coffeelint.json）に従う
  - 実装上不都合が出てきた場合、適宜判断し改修を行う（issueをお願いします）
  - これらのガイドを参考にしています
    - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)([日本語](http://cou929.nu/data/google_javascript_style_guide/))
    - [Cookpad | Cookpad CoffeeScript Coding Style Guide](https://github.com/cookpad/styleguide/blob/master/coffeescript.ja.md)
    - [GitHub | Coding Style](https://github.com/styleguide/javascript)

## ファイル名 / ファイルパス

  - 拡張子は `.js.coffee`

## インデント / 空白 / セミコロン

  - 2スペースとする
  - 行末の空行は禁止
  - メソッド間には一行空ける
  - 演算子の前後には空白を1つずつ空ける
  - ,の後には1つ空白を空ける
  - セミコロンは必然性がない限り省略する

## 文字数 / コメント

  - 可能な限り120文字以内とする。エラーメッセージ文言などはその限りではない
  - 複数行のコメントには```###```を使用する

## 文字列 / 配列 / ハッシュ

  - 基本的にシングルクォートを使用する
  - 式展開を行う場合、```"#{foo}"```を使用すること
  - +演算子を用いた式展開は使用しない
  - 複数行の文字列には```"""..."""```を使用できるがHTMLは可能なかがりHTMLとして記述すること。レスポンスが複雑かつ更新を伴うものの場合この限りではない
  - 複数行の配列、ハッシュを記述する場合には,を省略する
  - ハッシュを一行で記述する場合、{}の前後に1つ空白を空ける ```{ foo: bar }```  
  ※ ハッシュだけであることに注意すること。ハッシュ以外の場合、空白は入れない```"Hello World, ${name}!"```

## 演算子

  - 以下の演算子を使用を推奨する  
  ```&&, ||, ==, !=```
  - コードの統一のため以下のエイリアスは非推奨とする  
  ```and, or, is, isnt```

## 変数名

  - 変数名はcamelCaseを用いる
  - クラス名はPascalCaseを用いる
  - 定数は全て大文字でSNAKE_CASEを用いる
  - 一般的でない特定しにくい省略は避けること
  - 文言等の文章は可能な限り一箇所にまとめ、定数として定義すること  

  ```coffee
  text =  
    SUCCESS_AJAX: '通信に成功しました'  
    ERROR: 'エラーが発生しました'
  ```

## 関数

  - ```->```の前後にはスペースを空けること
  - 引数がない場合、括弧は省略すること
  - メソッド呼び出しの括弧は複数行の場合、省略する。一行の場合、省略しないこと
  - メソッドとメソッドの間には改行をいれること
  - thisの保持には```=>```を使用すること
  - 必要性のないfat allow```=>```は使用しないこと
  - 理由がない限りthisは@を使用すること。thisを使用する場合、コメントを残すこと

## クラス

  - privateのプロパティは `_` プレフィックスを付与すること


## その他推奨・非推奨事項

- JavaScript埋め込みは使用しないこと
- JavaScript / CoffeeScriptからDOMにアクセスする場合、そのDOMのクラスにjs-プレフィックスを付与すること
- JavaScript / CoffeeScriptからスタイルの更新を行う場合、スタイルのプロパティを変更するのではなくクラスの付け替えで行うこと。ドラッグ、タッチはこの限りではない
- JavaScript / CoffeeScriptからスタイルの更新を行う場合のクラス名はis-プレフィックスを使用すること

## 実装に当たって

- クラスファイルはPascalCase、他はcamelCaseとする
- ディレクトリはpascalCaseで設置とする
- Viewクラスファイルなどはファイル名の最後にViewとつけ、SomeView.js.coffeeとする
- Rails環境でのディレクトリ構成はroutesに従い、その配下は以下(sample)のようにすること
- クラスファイルが単一のroutesをまたぐ場合、javascript/classesに移すこと
- Viewクラスのイベントを結びつけるメソッドはsetEventListenersで統一すること

```
# sample
/some_action/index.js.coffee
/some_action/show.js.coffee
/some_action/views/SomeBtnView.js.coffee
/some_action/views/SomeFormView.js.coffee
/some_action/views/SomeModalView.js.coffee
/some_action/utils/SomeUtils.js.coffee
```

## 関連資料
- [Hands on Front-end :-) with CoffeeScript](https://github.com/khirayama/handson-front-end)
