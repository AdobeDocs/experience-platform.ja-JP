---
title: BrightCove ビデオトラッキング拡張機能の概要
description: Adobe Experience PlatformのBrightCove Video Trackingタグ拡張機能について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 38%

---

# BrightCove ビデオトラッキング拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

## 前提条件

Adobe Experience Platformの各タグプロパティには、拡張機能画面でインストールおよび設定された次の拡張機能が必要です。

* Adobe Analytics
* Experience Cloud 訪問者 ID サービス
* コア拡張機能のインストール

ビデオプレーヤーがレンダリングされる各WebページのHTMLで、「ページ内埋め込みコード（詳細）」コードスニペットを使用します。 「ページ内埋め込みコード（詳細）」HTMLスニペットは、[Brightcoveのドキュメント](https://studio.support.brightcove.com/publish/choosing-correct-embed-code.html#inpage)にあります。 次のリンクでは、プレビューと公開済みビデオプレーヤーの両方で埋め込みコードを生成する方法[に関する詳細情報を提供します。](https://ja.studio.support.brightcove.com/players/generating-player-embed-code.html)

この拡張機能バージョン1.1.0では、1つのWebページへの複数のBrightCoveビデオの埋め込みがサポートされています。 高度な埋め込みタグ内に複数の`id`プロパティがある場合は、それぞれに一意の値が設定されていることを確認します。 例えば、`player1`、`player2`などです。

>[!NOTE]
>
>ページに複数のビデオが含まれる場合、各ビデオで、そのページで実行されるタグルールに設定された同じ設定が使用されます。 例えば、ビデオが 50％完了したときにイベントをトリガーするルールを作成した場合、ページ上の各ビデオで、50％のキューポイントに到達するとルールがトリガーされます。

この拡張機能を使用するWebページが、関連するスクリプトが完全に読み込まれる前にビデオを操作している場合、問題を解決するために実行できるアクションは2つあります。 まず、タグライブラリを同期的に読み込み、次に、ページ上でビデオ埋め込みの前に`<script type="text/javascript">\_satellite.pageBottom();\</script\>`要素を配置します。

この拡張機能で使用されるコンポーネントメソッドとイベントについて詳しくは、[BrightCove APIのドキュメント](https://docs.brightcove.com/brightcove-player/1.x/Player.html#vjsplayer)を参照してください。

## データ要素

拡張機能内には 7 つのデータ要素がありますが、いずれも設定は不要です。

* **Playhead Position:** このデータ要素がタグルール内で呼び出されると、ビデオタイムライン上の再生ヘッドの位置を秒単位で記録します。
* **Video Account ID：**&#x200B;このデータ要素は、ビデオを公開した Brightcove アカウントの ID を記録します。
* **Video Duration：**&#x200B;このデータ要素は、ビデオコンテンツの合計時間（秒）を記録します。また、Analytics 内で計算指標を作成して、秒単位、分単位、時間単位で数値を変換できます。
* **Video Ad Support:** このデータ要素は、広告がビデオ内でサポートされるかどうかを指定します。
* **Video ID：**&#x200B;このデータ要素は、ビデオに関連付けられている BrightCove ID を指定します。
* **Video Name：**&#x200B;このデータ要素は、ビデオのわかりやすい名前を指定します。
* **Video Tags:** このデータ要素は、ビデオに関連付けられている特定のスクリプトを指定します。

## イベント

拡張機能内では 7 つのイベントを使用できます。カスタムキューポイントトラッキングのみが設定を必要とします。

* **Custom Cue Point Tracking：**&#x200B;このイベントは、ビデオが指定されたビデオしきい値の割合に達した場合にトリガーされます。例えば、ビデオの長さが60秒で、指定したキューポイントが50%の場合、イベントは30秒のマークでトリガーされます。

>[!NOTE]
>
>このイベントは、このキューポイントに到達するたびにトリガーされることにご注意ください。例えば、ユーザーが 50％マークに到達し、50％マークより前のビデオを探してから、再び 50％マークに到達すると、もう一度トリガーされます。

* **Video Completed:** このイベントは、ビデオが完全に完了したときにトリガーします。
* **Video Loaded Metadata:** このイベントは、プレーヤーが初期時間とディメンション情報を受け取ったときに発生します。
* **Video Pause：**&#x200B;このイベントは、ビデオが一時停止されたときにトリガーされます。
* **Video Resume：**&#x200B;このイベントは、一時停止イベントの後にビデオコンテンツが再開された場合にトリガーされます。
* **ビデオ画面の変更：** ビデオがフルスクリーンモードに切り替わったり、フルスクリーンモードから切り替わったりしたときのイベントトリガー。
* **Video Start：**&#x200B;このイベントは、ビデオコンテンツの開始を初めて発生したときにトリガーされます。

## 用途

各ビデオイベント（上記の7つのイベント）に対して1つのタグルールを設定できます。 追跡する各イベントに対して、特定のタグルールを作成します。 イベントを追跡しない場合は、を省略してイベントのルールを作成します。

ルールには次の 3 つのアクションがあります。

1. Adobe Analytics 変数を設定する（上記のデータ要素のすべてまたは一部に対してデータ要素を作成します）。
1. Adobe Analytics ビーコンを送信する
1. Adobe Analytics 変数をクリアする

## 「ビデオ開始」のタグルールの例

次のビデオ拡張オブジェクトが含まれます。

* **イベント**

   1. 「Video Start」（このイベントは、訪問者が BrightCove ビデオの再生を開始したときにルールを起動します）。

* **条件**

   1. なし

* **アクション**

   1. Analytics の「Set Variables」アクションで、次のように設定します。

      * **Video Start** のイベント（例：イベント 17）
      * **ビデオ名**&#x200B;データ要素のprop/eVar(例：eVar10)
      * **ビデオデュレーション**&#x200B;データ要素のprop/eVar(例：eVar11)
      * **現在のビデオ配置**&#x200B;データ要素のprop/eVar(例：eVar12)
   1. Analytics の「Send Beacon」アクション（`s.tl`）
   1. Analytics の「Clear Variables」アクション


>[!TIP]
>
>各ビデオ要素に対して複数のeVarやpropをプロビジョニングしたくない場合は、別の方法があります。 データ要素の値は、データ収集UI内で連結できます。 次に、分類ルールビルダーツールを使用して、分類レポートに解析されます。 詳しくは、 [分類ルールビルダーツール](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-rulebuilder/classification-rule-builder.html?lang=ja)のドキュメントを参照してください。 最後に、これらはAnalysis Workspaceでセグメントとして適用されます。
>
>これをおこなうには、「Video MetaData」などの新しいデータ要素を作成し、プログラムしてすべてのビデオデータ要素（上記）を取り込み、連結します。

```javascript
var r = [];

r.push( \_satellite.getVar( &#39;Video ID&#39; ) );

r.push( \_satellite.getVar( &#39;Video Name&#39; ) );

r.push( \_satellite.getVar( &#39;Video Duraction&#39; ) );


return r.join(&#39;|&#39;);
```
