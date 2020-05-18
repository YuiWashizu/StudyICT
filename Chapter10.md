# インターネット時代のプログラミング
Webシステムの基本原理と、よく使用されるクライアント側およびサーバ側言語について

## インターネット時代のプログラム開発と実行
- **WWWシステム(Webシステム)**：
  - CGIプログラム

これらは企業内の業務系でも使用されるようになっていくが、その場合はイントラネットシステムという名前で広まっていく

### イントラネットシステム
WebブラウザをクライアントとするC/Sシステムとして考えることができる
- 従来のC/Sシステム
  - アプリケーション機能が追加されるたびにユーザPCにインストールされているクライアントソフトウェアをバージョンアップしなければならない
- ブラウザクライアントを使用したイントラネットシステム
  - クライアントのバージョンバップは不必要で、システム管理・運用の面では負担の少ないシステムにすることができる

#### CGI処理用として使用されていたプログラム言語(C言語やPearl)の使いにくいポイント
- CGIプログラムでは、Webクライアントから送られてくるクエリパラメータを分解し、項目ごとの項目名とそのデータに分解して変数にセットする必要があるが、Webシステム以前の言語では、その手順を自分で作るか、または、ライブラリを探してきて、自部が作成するCGIプログラムに組み込む必要があった
- CGIからWebクライアントにレスポンスを返す場合の不自由さ。CGIプログラムでブラウザ表示用のタグを生成し、それをWebクライアントに送信・表示するようになっていたが、クライアントへの送信は標準出力への出力になるため、C言語では、`printf`文などの中にHTMLのタグ記述を組み込んで出力する必要がある

```c
#include <stdio.h>
main () {
     printf("Content-type: text/html\n\n"); //HTML の出力を宣言
     
     printf("<HTML>");
     printf("<HEAD>挨拶</HEAD>");
     printf("<BODY>こんにちは</BODY>>");
     printf("</HTML>");
     printf("\n");
}
```

このような方法だと、HTMLの表示内容を事前に確認することができず、また、都度`printf`内に記述するのが煩雑で、開発生産性が低下する

CGIを使用したWebシステムの利用が広まるにつれて、言語設計に上記の問題点に対する対応が組み込まれた、新しい言語(PHP, Ruby, Pythonなど)が登場するようになる。これらのWebシステムでの利用を想定した言語はほとんどがPerlと同様にインタープリタ言語になっている。そのため、実行速度が劣る。また、これらのインタープリタ言語をCGIとして使用する場合、Apache(Webサーバ)のモジュールとして実行することによって、Webサーバとは独立したCGIプログラムとして実行するよりもずっと実行パフォーマンスを向上することができる。

## インターネットの構成とプログラム言語
### JavaScriptとは
- **JavaScript**：
  - ECMAScript：JavaScriptの標準規格
  - DOM：HTMLやXML文章を取り扱うためのAPI。ノードの階層構造を識別して、JavaScriptなどのプログラミング言語から、扱いたいノードを特定し操作できるようにする仕組みを提供。
    - ウェブページを表す HTML のように文書の構造をメモリ内に表現することで、ウェブページとスクリプトやプログラミング言語を接続するもの(https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model)

### 初期のJavaScript
初期のJavaScriptはWebデザインでの補助的な機能として、ブラウザ画面に変化や動きのある効果を付け加える目的で使用されることが多かった。これは、Webシステムの仕組みにも起因している

#### Ajax以前のWebシステム構成
- Web画面：HTML
- FORM：POST
- Web利用

#### Ajaxモデル
多くのJavaScriptの使用事例では、その言語機能の

## Webクライアントの作成
WebクライアントではHTML、CSSそれにJavaScriptが主に使用される

### HTML5の基本的な書き方

```html
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>タイトル</title>
<link rel="stylesheet" type="text/css" href="css/html5reset.css" /> </head>
<body>
<!̶ここにWeb画面に表示するタグを記述 -->
</body> 11 </html>     
```

#### DOCTYPE宣言
`DOCTYPE`宣言とは、HTML文書がどのバージョンを利用し、どのDTD(文書型定義)に従って記述されているかを文頭に宣言すること

#### 文字コードと文字のエンコーディング指定
```xhtml
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
```

```html
<html lang="ja">
<head>
<meta charset="UTF-8" />
```

### HTML5の特徴
1. 構造化タグの追加
   - `<header>`、`<footer>`、`<article>`などの構造化タグが追加され、ヘッダ、フッタ、記事本体がどこかを区別できるようになっている。
1. 画像や動画/音声用のタグが追加された
   - `<audio>`、`<video>`、`<canvas>`タグにより、画像や動画/音声の編集が可能になっている。

### CSSの基本的な書き方
- **CSS(Cascading Style Sheets)：Webページのスタイルを指定するための言語で、HTMLで作成されるWebページにスタイルを適用する場合に使用される
  - HTMLがWebページ内の各要素の意味や情報構造を定義するのに大して、CSSはそれらをどのように装飾するかを指定する(色・サイズ・レイアウトの表示スタイルや、プリンタなどの機器で印刷・出力される時の出力スタイルなど)
  - 現在では、HTML5でタグを記述し、CSS3でスタイルを指定するのが標準の組み合わせ

### JacaScriptの基本的な書き方
####  JacaScriptプログラム
JavaScriptはHTMPのHEADタグ内に記述することが多いが、BODYタグうちに記述することも可能
```
<!DOCTYPE html>
<html lang="ja"> <head>
<meta charset="UTF-8"> </head>
<body>
<script type="text/javascript">
	document.write("今年は東京も寒いですね。") 
</script>
<p>長期予報では暖冬でしたけどね。</p> 
</body>
</html>
```

クライアント・サーバ型の典型的な処理パターンでは、画面からデータを入力した後、マウスクリックなどによって、入力されたデータをサーバに送信し、それによるサーバ側での処理結果を受け取るような流れになっている

## Ajaxでサーバと非同期通信
### Ajax非同期通信サンプル
従業員番号から従業員氏名を表示するシステム
1. イベントリスなからXML HttpRequestへ通信
1. XML HttpRequestがPHPへリクエストを送受信する
1. XML HttpRequestがDOM APIへ通信

1. `window.addEventListener("load", initOnLoad, false);`
   - `addEventLintener`はDOMモデルでイベントを検知するイベントリスナを設定する
1. 呼び出される`initOnLoad`関数では`document.getElementById`で、引数の指定したID値の要素、つまり従業員番号フィ＝るどの入力値を返し、そのフィールドで`blur`イベントが検知された時に`shrFunc関数を呼び出す
1. `xhrFunc`関数での処理


Ajaxの処理内容をシンプルに記述するために、Ajaxに対して300種類以上のライブラリがリリースされている。

### AjaxライブラリのiQeryで非同期通信
先ほどのサンプルと同じ内容の処理をjQueryで書き換える

### サーバスクリプト
非同期通信でWebクライアントから送られたデータを処理するサーバプログラムの処理を確認する


## 