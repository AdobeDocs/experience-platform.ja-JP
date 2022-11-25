---
title: BrightCove ビデオトラッキング拡張機能の概要
description: Adobe Experience Platform の BrightCove ビデオトラッキングタグ拡張機能について説明します。
exl-id: d27eff21-2abf-4495-8382-08cab32742e0
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 97%

---

# BrightCove ビデオトラッキング拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 前提条件

Adobe Experience Platform の各タグプロパティには、拡張機能画面でインストールおよび設定された次の拡張機能が必要です。

* Adobe Analytics
* Experience Cloud 訪問者 ID サービス
* コア拡張機能のインストール

ビデオプレーヤーがレンダリングされる各 Web ページの HTML で、「ページ内埋め込みコード（詳細）」コードスニペットを使用します。「ページ内埋め込みコード（詳細）」HTML スニペットは、[Brightcove のドキュメント](https://studio.support.brightcove.com/publish/choosing-correct-embed-code.html#inpage)に記載されています。 次のリンクでは、[プレビューと公開済みビデオプレーヤーの両方で埋め込みコードを生成する方法](https://ja.studio.support.brightcove.com/players/generating-player-embed-code.html)に関する詳細情報を提供します。

この拡張機能バージョン 1.1.0 では、単一 Web ページへの複数の BrightCove ビデオの埋め込みがサポートされています。高度な埋め込みタグ内に複数の `id` プロパティがある場合は、それぞれに一意の値があることを確認します。例えば、`player1`、`player2` など。

>[!NOTE]
>
>複数のビデオがあるページでは、各ビデオはそのページで実行されるタグルールで同じ設定を使用します。例えば、ビデオが 50％完了したときにイベントをトリガーするルールを作成した場合、ページ上の各ビデオで、50％のキューポイントに到達するとルールがトリガーされます。

この拡張機能を使用しているWeb ページで、関連するスクリプトが完全に読み込まれる前にビデオとやり取りする場合、問題を解決するために実行できるアクションが 2 つあります。まず、タグライブラリを同期的に読み込むことができます。次に、ページに埋め込まれるビデオの前に `<script type="text/javascript">\_satellite.pageBottom();\</script\>` 要素を配置します。

この拡張機能で使用されるコンポーネントメソッドとイベントについて詳しくは、 [BrightCove API のドキュメント](https://docs.brightcove.com/brightcove-player/1.x/Player.html#vjsplayer) を参照してください。

## データ要素

拡張機能内には 7 つのデータ要素がありますが、いずれも設定は不要です。

* **Playhead Position：** このデータ要素がタグルール内で呼び出されると、ビデオタイムライン上の再生ヘッドの位置を秒単位で記録します。
* **Video Account ID：**&#x200B;このデータ要素は、ビデオを公開した Brightcove アカウントの ID を記録します。
* **Video Duration：**&#x200B;このデータ要素は、ビデオコンテンツの合計時間（秒）を記録します。また、Analytics 内で計算指標を作成して、秒単位、分単位、時間単位で数値を変換できます。
* **Video Ad Support：**&#x200B;このデータ要素は、ビデオ内で広告がサポートされるかどうかを指定します。
* **Video ID：**&#x200B;このデータ要素は、ビデオに関連付けられている BrightCove ID を指定します。
* **Video Name：**&#x200B;このデータ要素は、ビデオのわかりやすい名前を指定します。
* **Video Tags：**&#x200B;このデータ要素は、ビデオに関連付けられている特定のタグを指定します。

## イベント

拡張機能内では 7 つのイベントを使用できます。カスタムキューポイントトラッキングのみが設定を必要とします。

* **Custom Cue Point Tracking：**&#x200B;このイベントは、ビデオが指定されたビデオしきい値の割合に達した場合にトリガーされます。例えば、ビデオの長さが 60 秒で、指定されたキューポイントが 50%の場合、イベントは 30 秒のマークでトリガーされます。

>[!NOTE]
>
>このイベントは、このキューポイントに到達するたびにトリガーされることにご注意ください。例えば、ユーザーが 50％マークに到達し、50％マークより前のビデオを探してから、再び 50％マークに到達すると、もう一度トリガーされます。

* **Video Completed：**&#x200B;このイベントは、ビデオが完全に完了したときにトリガーされます。
* **Video Loaded Meta Data：**&#x200B;このイベントは、プレーヤーが初期時間とディメンション情報を受け取ったときに発生します。
* **Video Pause：**&#x200B;このイベントは、ビデオが一時停止されたときにトリガーされます。
* **Video Resume：**&#x200B;このイベントは、一時停止イベントの後にビデオコンテンツが再開された場合にトリガーされます。
* **Video Screen Change：**&#x200B;このイベントは、ビデオがフルスクリーンモードに切り替わったり、フルスクリーンモードから切り替えられたりしたときにトリガーされます。
* **Video Start：**&#x200B;このイベントは、ビデオコンテンツの開始を初めて発生したときにトリガーされます。

## 用途

各ビデオイベント（上記の 7 つのイベント）に対して 1 つのタグルールを設定できます。 トラックする各イベントに対して、特定のタグルールを作成します。イベントをトラッキングしない場合は、イベントのルールの作成を省略します。

ルールには次の 3 つのアクションがあります。

1. Adobe Analytics 変数を設定する（上記のデータ要素のすべてまたは一部のデータ要素を作成します。）
1. Adobe Analytics ビーコンを送信する
1. Adobe Analytics 変数をクリアする

## 「Video Start」のタグルールの例

次のビデオ拡張機能オブジェクトが含まれます。

* **イベント**

   1. 「Video Start」（このイベントは、訪問者が BrightCove ビデオの再生を開始したときにルールを起動します）。

* **条件**

   1. なし

* **アクション**

   1. Analytics の「Set Variables」アクションで、次のように設定します。

      * **Video Start** のイベント（例：イベント 17）
      * **ビデオ名**&#x200B;データ要素の prop／eVar（例：eVar10）
      * **ビデオ期間**&#x200B;データ要素の prop／eVar（例：eVar11）
      * **現在のビデオ配置**&#x200B;データ要素の prop／eVar（例：eVar12）
   1. Analytics の「Send Beacon」アクション（`s.tl`）
   1. Analytics の「Clear Variables」アクション


>[!TIP]
>
>各ビデオ要素に対して複数の eVar や prop をプロビジョニングしたくない場合は、代替方法としてデータ要素の値が連結されます。 次に、分類ルールビルダーツールを使用して、分類レポートに解析されます。詳しくは、 [分類ルールビルダーツール](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-rulebuilder/classification-rule-builder.html?lang=ja) のドキュメントを参照してください。最終的には、Analysis Workspace のセグメントとして適用されます。
>
>これをおこなうには、「ビデオメタデータ」のような新しいデータ要素を作成し、（上記の）すべてのビデオデータ要素をプルし、連結するようにプログラミングします。

```javascript
var r = [];

r.push( \_satellite.getVar( &#39;Video ID&#39; ) );

r.push( \_satellite.getVar( &#39;Video Name&#39; ) );

r.push( \_satellite.getVar( &#39;Video Duraction&#39; ) );


return r.join(&#39;|&#39;);
```
