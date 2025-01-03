Japanese Braille Table
======================

日本語点字のための静的な変換表です。著作権等は主張しません。

これはフリーソフトウェア・オープンソースソフト等で広く用いられるのを意図して
作られた、静的なテキストの対応表です。すべてのファイルはUTF-8でエンコード
されています。

単純な一覧表のため、著作権は発生しない可能性も大きいと考えています。しかし
それが発生した場合、確かに無効とするために[CC0ライセン
ス](https://creativecommons.org/public-domain/cc0/)を適用します。
同梱のファイル[LICENSE.md](./LICENSE.md)をご覧ください。

## 現状の制限

現在、日本語かな一文字にしか対応していません。これは単に、作業途中のためです。

## ファイルの構成

以下の4つのファイルから構成されています。全ては実質的に同じ物で、いづれも
一つのファイル[`source.tsv`](./source.tsv)から生成されています。詳しくは
[Rakefile](./Rakefile)をご覧ください。

* [braille-ja-table-**raw.tsv**](./braille-ja-table-raw.tsv)
    * TSVファイルです。
    * 生のUTF-8で表された「日本語ひらがな」と「点字」が一行に入っています。
* [braille-ja-table-**escaped.tsv**](./braille-ja-table-escaped.tsv)
    * TSVファイルです。
    * ASCIIによるUnicodeエスケープで表された「日本語ひらがな」と「点字」が一行に入ってい
      ます。
* [braille-ja-table-**raw.json**](./braille-ja-table-raw.json)
    * JSONファイルです。
    * 生のUTF-8で表され、「日本語ひらがな」がキーに、「点字」がその値になっています。
* [braille-ja-table-**escaped.json**](./braille-ja-table-escaped.json)
    * JSONファイルです。
    * ASCIIによるUnicodeエスケープで表され、「日本語ひらがな」がキーに、「点字」がその値に
      なっています。

## ファイルの生成法

`source.tsv`を編集した後、単純に`rake`を実行します。これで4つのファイルが自動的に生成され
ます。成果物を一旦削除したい場合は`rake clobber`を実行してください。

なお、標準ライブラリ以外に依存する外部gemはありません。そのため`bundle`も不要です。
