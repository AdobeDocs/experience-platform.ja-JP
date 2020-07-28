---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーイベントへの購読
topic: privacy events
translation-type: tm+mt
source-git-commit: 5b32c1955fac4f137ba44e8189376c81cdbbfc40
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 31%

---


# 購読する [!DNL Privacy Events]

[!DNL Privacy Events] は、Adobe Experience Platformが提供するメッセージ [!DNL Privacy Service]です。これは、設定されたwebフックに送信されるAdobeI/Oイベントを利用して、効率的なジョブリクエストの自動化を促進します。 They reduce or eliminate the need to poll the [!DNL Privacy Service] API in order to check if a job is complete or if a certain milestone within a workflow has been reached.

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の 4 種類があります。

| タイプ | 説明 |
--- | ---
| ジョブ完了 | All [!DNL Experience Cloud] solutions have reported back and the overall or global status of the job has been marked as complete. |
| ジョブエラー | リクエストの処理中に 1 つ以上のソリューションがエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているソリューションの 1 つが、その作業を完了しました。 |
| 製品エラー | リクエストの処理中に 1 つ以上のソリューションがエラーを報告しました。 |

This document provides steps for setting up an integration for [!DNL Privacy Service] notifications within Adobe I/O. For a high-level overview of [!DNL Privacy Service] and its features, see the [Privacy Service overview](home.md).

## はじめに

このチュートリアルでは、セキュアなトンネルを通じてローカルサーバーをパブリックインターネットに公開するソフトウェア製品、**ngrok** を使用します。このチュートリアルを始めてローカルマシンに Webhook を作成する前に、[ngrok をインストール](https://ngrok.com/download)してください。This guide also requires you to have a GIT repository downloaded that contains a simple [Node.js](https://nodejs.org/) server.

## ローカルサーバーの作成

Node.js サーバーは、リクエストによってルート（`/`）エンドポイントに送信された `challenge` パラメーターを返す必要があります。これをおこなうには、次の JavaScript を使用して `index.js` ファイルを設定します。

```js
var express = require('express')
var app = express()

app.set('port', (process.env.PORT || 3000))
app.use(express.static(__dirname + '/public'))

app.get('/', function(request, response) {
  response.send(request.originalUrl.split('?challenge=')[1]);
})

app.listen(app.get('port'), function() {
  console.log("Node app is running at localhost:" + app.get('port'))
})
```

コマンドラインを使用して、Node.js サーバーのルートディレクトリに移動します。次に、以下のコマンドを入力します。

1. `npm install`
1. `npm start`

これらのコマンドは、すべての依存関係をインストールし、サーバーを初期化します。成功した場合、サーバーが http://localhost:3000/ で実行されていることを確認できます。

## Ngrok を使用した Webhook の作成

新しいコマンドラインウィンドウを開き、以前にngrookをインストールしたディレクトリに移動します。 ここから、次のコマンドを入力します。

```shell
./ngrok http -bind-tls=true 3000
```

出力が成功すると、次のようになります。

![Ngrok 出力](images/privacy-events/ngrok-output.png)

次の手順で Webhook を識別するために使用される `Forwarding` URL（`https://212d6cd2.ngrok.io`）を控えておきます。

## Adobeデベロッパーコンソールでの新しいプロジェクトの作成

Go to [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) and sign in with your Adobe ID. 次に、Adobeデベロッパーコンソールのドキュメントで、空のプロジェクトの [作成に関するチュートリアルに説明されている手順に従います](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) 。

## プロジェクトの追加プライバシーイベント

コンソールで新しいプロジェクトの作成が完了したら、「プ **[!UICONTROL ロジェクト概要]** 」画面の「 _イベント_ 」をクリックします。

![](./images/privacy-events/add-event-button.png)

The _Add events_ dialog appears. 「 **[!UICONTROL Experience Cloud]** 」を選択して使用可能なイベントタイプのリストを絞り込み、「 **[!UICONTROL Privacy Serviceイベント]** 」を選択してから「 **[!UICONTROL 次へ]**」をクリックします。

![](./images/privacy-events/add-privacy-events.png)

[ _イベント登録の_ 設定]ダイアログが表示されます。 対応するチェックボックスをオンにして、受け取るイベントを選択します。 選択したイベントは、左の列の「 _[!UICONTROL 購読済みイベント]_」の下に表示されます。 終了したら、「**[!UICONTROL &#x200B;次へ&#x200B;]**」をクリックします。

![](./images/privacy-events/choose-subscriptions.png)

次の画面では、イベント登録用の公開鍵を入力するように求められます。 自動的にキーペアを生成するか、端末で生成した独自の公開鍵をアップロードするかを選択できます。

このチュートリアルでは、最初のオプションに従います。 「キーペアを **[!UICONTROL 生成」のオプションボックスをクリックし]**、右下隅の「キーペアを **[!UICONTROL 生成]** 」ボタンをクリックします。

![](./images/privacy-events/generate-key-value.png)

キーペアが生成されると、ブラウザーによって自動的にダウンロードされます。 このファイルは開発者コンソールで保持されないので、自分で保存する必要があります。

次の画面では、新しく生成されたキーペアの詳細を確認できます。 「**[!UICONTROL 次へ]**」をクリックして次に進みます。

![](./images/privacy-events/keypair-generated.png)

次の画面で、イベント登録の名前と説明を入力します。 ベストプラクティスは、同じプロジェクト内の他のユーザーとこのイベントの登録を区別できるように、一意で、簡単に識別できる名前を作成することです。

![](./images/privacy-events/event-details.png)

同じ画面の下に、イベントの受信方法を設定する2つのオプションが表示されます。 「 **[!UICONTROL Webhook]** 」を選択し、「 `Forwarding` Webhook URL _[!UICONTROL 」で前に作成したNgrook Webフックの]_URLを指定します。 次に、「設定済みのイベントを**[!UICONTROL &#x200B;保存」をクリックする前に、希望の配信スタイル（単一またはバッチ）を選択し、イベントの登録を完了します&#x200B;]**。

![](./images/privacy-events/webhook-details.png)

プロジェクトの詳細ページが再表示され、左のナビゲーションの [!DNL Privacy Events] イベント __の下に表示されます。

## イベントデータの表示

プロジェクトに登録し、プライバシージョブ [!DNL Privacy Events] が処理されると、その登録に関して受信した通知を表示できます。 デベロッパーコンソールの「 **[!UICONTROL プロジェクト]** 」タブで、リストからプロジェクトを選択し、 _製品概要_ ページを開きます。 画面左側のナビゲーションから「 **[!UICONTROL プライバシーイベント]** 」を選択します。

![](./images/privacy-events/events-left-nav.png)

The _Registration Details_ tab appears, allowing you to view more information about the registration, edit its configuration, or view the actual events that were received since activating your webhook.

![](./images/privacy-events/registration-details.png)

[ **[!UICONTROL デバッグトレース]** ]タブをクリックして、受信したイベントのリストを表示します。 表示されたイベントをクリックして詳細を表示します。

![](images/privacy-events/debug-tracing.png)

「_[!UICONTROL ペイロード]_」セクションには、上の例で強調表示されているイベントタイプ（`com.adobe.platform.gdpr.productcomplete`）を含む、選択したイベントの詳細が表示されます。

## 次の手順

必要に応じて、異なる Webhook アドレスに対して新しい統合を追加する場合に、上記の手順を繰り返すことができます。