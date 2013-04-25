# Markdown to Inao-Format

[![Build Status](https://travis-ci.org/naoya/md2inao.pl.png?branch=master)](https://travis-ci.org/naoya/md2inao.pl) [![Coverage Status](https://coveralls.io/repos/naoya/md2inao.pl/badge.png?branch=master)](https://coveralls.io/r/naoya/md2inao.pl)

Description
----------

Markdown で書かれたテキストを「inaoフォーマット」に変換します。

* bin/md2inao.pl : CUIコマンド版
* http://md2inao.bloghackers.net/ : Web版

markdown2inao.pl 改め md2inao.pl のこれまでについては https://gist.github.com/inao/baea09bc6fc53551886b を見て下さい。

How to use
----------

    % cpanm Carton
    % carton
    % carton exec perl bin/md2inao.pl your_markdown.md > path/to/output.txt
    
出力見本
----------

PDFにすると、以下のような仕上がりになります。

### 書籍版

https://docs.google.com/open?id=0BzbGMS73rIkDUXpyUVlrSUxURXlmMXhQRV9Ua2JCUQ

### WEB+DB PRESS版

https://docs.google.com/open?id=0BzbGMS73rIkDZjdCTnBkMDFUaGF2UDJIdTNfaVJUUQ

How to test
----------

    % carton exec -Ilib -- prove  
    
自由置換の書き方
----------

```
{
    "before_filter": {
        "<kbd>F10</kbd>" : "<cFont:Key Snd Mother>*<cFont:>",
        "<kbd>F11</kbd>" : "<cFont:Key Snd Mother>+<cFont:>",
        "<kbd>F12</kbd>" : "<cFont:Key Snd Mother>,<cFont:>",
        "<kbd>F1</kbd>" : "<cFont:Key Snd Mother>!<cFont:>",
        "<kbd>F2</kbd>" : "<cFont:Key Snd Mother>\"<cFont:>",
        "<kbd>End</kbd>" : "<cFont:Key Snd Mother>n<cFont:>"
    },
    "after_filter": {
        "★" : "<CharStyle:赤字>★<CharStyle:>",
                
        "◆→◆" : "<cTypeface:R-KL><cFont:A-OTF リュウミン Pr5><27A1><cTypeface:><cFont:>",
        "◆←◆" : "<cTypeface:R-KL><cFont:A-OTF リュウミン Pr5><2B05><cTypeface:><cFont:>",
        "◆↑◆" : "<cTypeface:R-KL><cFont:A-OTF リュウミン Pr5><2B06><cTypeface:><cFont:>",
        "◆↓◆" : "<cTypeface:R-KL><cFont:A-OTF リュウミン Pr5><2B07><cTypeface:><cFont:>",
        
        "◆←→◆" : "<21D4>",
        "◆＞＝◆" : "<2267>",
        "◆＝＞◆" : "<2266>",
        
        "◆WDB◆" : "<cstyle:ストッパ>#<cstyle:>"
    }
}
```

- InDesign 出力時は config/id_filter.json に書いた設定通りに出力を置換できます
- キーには正規表現が使えます
- JSON の文法に注意 (末尾のカンマ、" のエスケープなど)

### before_filter

- Markdown parse の前に置換
- Markdown のテキストを置換したい時は こちら
- HTML を置換したい時もこちら
- 値の &lt;, &gt; はエスケープされてから Markdown parser に渡されます。その後 after_filter で復元されます。(要するに書いたとおりに出力される。HTMLとして処理されることは期待できない、ということ)
        
### after_filter

- InDesign への変換が終わった後に置換
- InDesign になったテキストを置換したい時はこちら
- md 中の `<span class="symbol">…</span>` は after_filter 前に` ◆…◆` になります

Authors
----------

* @typester : Original version: https://gist.github.com/typester/380428
* @inao : Current product owner & maintainer
* @naoya : Refactoring, Add some tests, Web version
* @hsbt
* @hokaccha
* @suzuki

捕捉
----------

* gist から普通のレポジトリにする方法わかんなくて新規に切っちゃいました。すみません (@naoya)

LICENSE
----------

* Same as Perl

Contributing
----------

1. Fork it
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create new Pull Request

IRC
----------

- #md2inao@freenode

取り扱い説明書
----------

### メタデータ（タイトル、著者名など）

テキストファイルの冒頭に、以下のメタデータを書けます。

```
Chapter: 3（章番号）
Serial: 5（連載回数）
Title: Markdown to Inao（タイトル）
Subtitle: Convert markdown text to Inao format（キャッチコピー）
Author: 伊藤 直也（著者名）
Author(romaji): ITO Naoya（著者名のローマ字表記）
Supervisor: 稲尾 尚徳（監修者名）
Supervisor(romaji): INAO Naonori（監修者名のローマ字表記）
Affiliation: 技術評論社（所属）
URL: http://naoya.github.com/
mail: i.naoya@gmail.com
Github: naoya
Twitter: @naoya_ito

Hello, World（本文）
```

#### 注意事項

* Title、Subtitle、Author、Author(romaji)は必須です
* テキストファイルの冒頭に書く必要があります
* メタデータと本文の間に空行が必要です
* 任意のメタデータを追加可能ですが、キーにマルチバイト文字は使えません

### 見出し

各章の配下、各記事（連載、一般記事）の配下には、見出しが3階層まで使えます。

```
# 大見出し（節）
## 中見出し（項）
### 小見出し（目）
```

#### 非推奨

md2inao的には以下の記法にも対応していますが、現状は非推奨です（アウトラインの作成がちょっとめんどうになるので）。

```
大見出し（節）
===============

中見出し（項）
---------------
```
