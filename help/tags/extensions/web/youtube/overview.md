---
title: YouTube ビデオトラッキング拡張機能の概要
description: Adobe Experience PlatformのYouTube Video Trackingタグ拡張について説明します。
source-git-commit: 5da1fd18e0032c5e3d6695639f98a7ee683819f1
workflow-type: tm+mt
source-wordcount: '890'
ht-degree: 51%

---

# YouTube ビデオトラッキング拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

**前提条件**

Adobe Experience Platformの各タグプロパティでは、次の拡張機能がインストールされ、拡張機能画面から設定されている必要があります。

* Adobe Analytics
* Experience Cloud 訪問者 ID サービス
* Core 拡張機能

[「Embed a player using an \&lt;iframe\> tag」](https://developers.google.com/youtube/player_parameters#Manual_IFrame_Embeds)コードスニペットをGoogle開発者ドキュメントのビデオプレーヤーがレンダリングされる各WebページのHTMLに使用します。

この拡張機能のバージョン2.0.1では、iframeスクリプトタグに一意の値を持つ`id`属性を挿入し、`enablejsapi=1`と`rel=0`を`src`属性値の末尾に追加する（まだ含まれていない場合）ことで、1つのWebページに1つ以上のYouTubeビデオを埋め込むことができます。 次に例を示します。

`<iframe id="player1" width="560" height="315" src="https://www.youtube.com/embed/xpatB77BzYE?enablejsapi=1" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>`

また、この拡張機能は、`enablejsapi`と`rel`のクエリー文字列パラメーターが存在するかどうか、および期待される値が正しいかどうかに関係なく、`player1`などの一意のID属性値を動的に確認するように設計されています。 その結果、YouTube スクリプトタグは、`id` 属性の有無および `enablejsapi` と `rel` のクエリ文字列パラメーターが含まれているかどうかに関わらず、web ページに追加できます。

>[!NOTE]
>
>ページに複数のビデオが含まれる場合、各ビデオで、そのページで実行されるタグルールに設定された同じ設定が使用されます。 例えば、ビデオが 50％ 完了したときにイベントをトリガーするルールを作成した場合、ページ上の各ビデオで、50％ のキューポイントに到達するとルールがトリガーされます。

この拡張機能は、iFrames を書き換えるために次のロジックに依存します。

```javascript
document.onreadystatechange = function () {
 if (document.readyState === 'complete') {
```

したがって、ページが読み込まれると、わずかにちらつきが発生します。 この動作は期待されたとおりです。

## データ要素

拡張機能内には 6 つのデータ要素がありますが、いずれも設定は不要です。

* **再生ヘッドの位置：** タグルール内で呼び出されたときの、ビデオタイムライン上の再生ヘッドの位置を秒単位で記録します。
* **ビデオ ID：** ビデオに関連付けられている YouTube ID を指定します。
* **ビデオ名：** ビデオのわかりやすい名前を指定します。
* **ビデオの URL：** 現在読み込まれている／再生中のビデオの YouTube.com URL を返します。
* **ビデオの長さ：** ビデオコンテンツの合計時間（秒）を記録します。
* **拡張機能バージョン：** このデータ要素は、例えば「Video Tracking_YouTube_2.0.0」などの YouTube トラッキング拡張機能バージョンを記録します。

## イベント

拡張機能内では 8 つのイベントを使用できます。カスタムキューポイントトラッキングのみが設定を必要とします。

* **ビデオ準備完了：** ビデオがキューに入り、再生の準備ができたときにトリガーされます。
* **ビデオ開始：** ビデオが最初に開始されたときと `player.getCurrentTime() === 0` ときにトリガーされます
* **ビデオ再生：** ビデオがキューに入ったときにトリガーされ、最初の開始の後に再生されます。このトリガーは、すべてのリプレイで実行されます。
* **ビデオ一時停止：** ビデオが一時停止されたときにトリガーされます。
* **ビデオ再開：** ビデオが再開されたとき、および `player.getCurrentTime() !== 0` ときにトリガーされます 
* **カスタムキュートラッキング：** ビデオが指定されたビデオしきい値の割合に達した場合にトリガーされます。例えば、ビデオが60秒で、指定したキューポイントが50%の場合、再生ヘッドの位置が30秒に等しいと、イベントはトリガーします。 キューポイントトラッキングは、初期再生とリプレイの両方に適用されます。ユーザーがキューポイントをシークした場合、イベントは発生しません。 キューポイントイベントは、再生ヘッドがタイムライン上の計算されたキューポイントの場所を越え、ビデオプレーヤーが再生中の場合にのみ実行されます。
* **ビデオバッファ：** プレーヤーが一定量のデータをダウンロードしてからビデオの再生を開始した場合にトリガーされます。
* **ビデオ終了：** ビデオが完全に完了した場合にトリガーされます。

## 用途

各ビデオイベント（上記の7つのイベント）に対して1つのタグルールを設定できます。 追跡する各イベントに対して、特定のタグルールを作成します。 イベントを追跡しない場合は、を省略してイベントのルールを作成します。

ルールには次の 3 つのアクションがあります。

* **変数を設定：** Adobe Analytics 変数を設定します（含まれるすべてのまたは一部のデータ要素にマップ）。
* **ビーコンを送信：** Adobe Analytics ビーコンをカスタムリンクトラッキングコールとして送信し、「リンク名」の値を指定します。
* **変数をクリア：** Adobe Analytics 変数をクリアします。

## 「ビデオ開始」のタグルールの例

次のビデオ拡張オブジェクトが含まれます。

* **イベント**:「Video Start」(このイベントは、訪問者がYouTubeビデオの再生を開始したときにルールを起動します)。

* **条件**：なし

* **アクション**： **Analytics拡張機能**&#x200B;を使用して「変数を設定」アクションを使用し、次のようにマッピングします。

   * ビデオ開始のイベント、
   * ビデオデュレーションデータ要素の prop／eVar
   * ビデオ ID データ要素の prop／eVar
   * ビデオ名データ要素の prop／eVar
   * ビデオ URL データ要素の prop／eVar

   次に、「ビーコンを送信」アクション(`s.tl`)をリンク名「ビデオ開始」と共に追加し、「変数をクリア」アクションを実行します。

>[!TIP]
> 
>各ビデオ要素に対して複数のeVarやpropを使用できない実装では、Platform内でデータ要素の値を連結し、[https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-rulebuilder/classification-rule-builder.html](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-rulebuilder/classification-rule-builder.html?lang=ja)で説明されているように分類ルールビルダーを使用して分類レポートに解析し、Analysis Workspaceでセグメントとして適用できます。

ビデオ情報の値を連結するには、「Video Meta Data」という新しいデータ要素を作成し、プログラムしてすべてのビデオデータ要素（上記を参照）を取り込み、それらをまとめます。 次に例を示します。

```javascript
var r = ””;

r.push('YouTube'); //Player Name
r.push(_satellite.getVar('Video ID'));
r.push(_satellite.getVar('Video Name'));
r.push(_satellite.getVar('Video Duration'));
r.push(_satellite.getVar('Extension Version'));

return r.join('|');
```
