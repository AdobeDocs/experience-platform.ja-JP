---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーイベントの購読
topic: privacy events
translation-type: tm+mt
source-git-commit: e4cd042722e13dafc32b059d75fca2dab828df60
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---


# プライバシーイベントの購読

プライバシーイベントは、Adobe Experience Platform Privacy Serviceが提供するメッセージです。Adobe Experience Platform Privacy Serviceは、設定されたWebフックに送信されるAdobe I/Oイベントを利用して、効率的なジョブリクエストの自動化を促進します。 ジョブが完了したか、ワークフロー内の特定のマイルストーンに達したかを確認するために、プライバシーサービスAPIをポーリングする必要が少なくなるか、不要になります。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の4種類があります。

| タイプ | 説明 |
--- | ---
| ジョブ完了 | すべてのExperience Cloudソリューションが報告を取り戻し、ジョブの全体的なステータスまたはグローバルステータスが完了とマークされました。 |
| ジョブエラー | 1つ以上のソリューションが、要求の処理中にエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられている解決策の1つが、作業を完了しました。 |
| 製品エラー | いずれかのソリューションで、要求の処理中にエラーが報告されました。 |

このドキュメントでは、Adobe I/O内でプライバシーサービス通知の統合を設定する手順を説明します。 プライバシーサービスとその機能の概要については、「 [プライバシーサービスの概要](home.md)」を参照してください。

## はじめに

このチュートリアルでは、 **ngrok**、セキュリティで保護されたトンネルを通してローカルサーバをパブリックインターネットに公開するソフトウェア製品を使用します。 このチュートリアルを開始する前に [、ngrokを](https://ngrok.com/download) インストールして、ローカルマシンにWebフックを作成してください。 また、このガイドでは、 [Node.jsで記述されたシンプルなサーバーを含むGITリポジトリをダウンロードする必要があります](https://nodejs.org/)。

## ローカルサーバーの作成

Node.jsサーバーは、リクエストによってルート( `challenge``/`)エンドポイントに送信されたパラメーターを返す必要があります。 これを行うには、次のJavaScriptを使用して `index.js` ファイルを設定します。

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

コマンドラインを使用して、Node.jsサーバーのルートディレクトリに移動します。 次に、次のコマンドを入力します。

1. `npm install`
1. `npm start`

これらのコマンドは、すべての依存関係をインストールし、サーバーを初期化します。 正常に終了した場合は、サーバーがhttp://localhost:3000/で実行されていることを確認できます。

## ngrookを使用してWebフックを作成する

同じディレクトリ内および新しいコマンドラインウィンドウで、次のコマンドを入力します。

```shell
ngrok http -bind-tls=true 3000
```

成功した場合の出力結果は次のようになります。

![ngrook出力](images/privacy-events/ngrok-output.png)

次の手順でWebフックを識別するために使用される `Forwarding` URL(`https://e142b577.ngrok.io`)を控えておいてください。

## Adobe I/Oコンソールを使用した新しい統合の作成

[Adobe I/Oコンソールにサインインし](https://console.adobe.io) 、「 **統合** 」タブをクリックします。 「 _統合_ 」ウィンドウが表示されます。 ここから、「 **新しい統合**」をクリックします。

![Adobe I/Oコンソールでの表示統合](images/privacy-events/integrations.png)

[新しい統合の *作成* ]ウィンドウが表示されます。 「近いリアルタイムイベントを **受信する**」を選択し、「 **続行**」をクリックします。

![新しい統合の作成](images/privacy-events/new-integration.png)

次の画面には、購読、権限および権限に基づいて、組織が使用できる様々なイベント、製品およびサービスとの統合を作成するオプションが表示されます。 この統合の場合は、「 **プライバシーサービスのイベント**」を選択し、「 **続行**」をクリックします。

![プライバシーイベントの選択](images/privacy-events/privacy-events.png)

「 *統合の詳細* 」フォームが表示され、統合の名前と説明、および公開鍵証明書を入力する必要があります。

![統合の詳細](images/privacy-events/integration-details.png)

公開証明書がない場合は、次のterminalコマンドを使用して証明書を生成できます。

```shell
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub
```

証明書を生成したら、ファイルを「 **公開鍵証明書** 」ボックスにドラッグ&amp;ドロップするか、「ファイルの **選択** 」をクリックしてファイルディレクトリを参照し、証明書を直接選択します。

証明書を追加すると、「 *イベント登録* 」オプションが表示されます。 「 **イベント登録**」をクリックします。

![追加イベント登録](images/privacy-events/add-event-registration.png)

ダイアログが展開され、追加のコントロールが表示されます。 ここでは、目的のイベントタイプを選択して、Webフックを登録できます。 イベント登録の名前、WebフックURL(Webフックを最初に `Forwarding` 作成したときに返される [アドレス](#create-a-webhook-using-ngrok))、簡単な説明を入力します。 最後に、購読するイベントタイプを選択し、「 **保存**」をクリックします。

![イベント登録フォーム](images/privacy-events/event-registration-form.png)

イベント登録フォームが完了したら、[統合の **作成** ]をクリックします。I/O統合が完了します。

![統合の作成](images/privacy-events/create-integration.png)

## 表示イベントデータ

I/O統合とプライバシージョブの作成が完了すると、その統合に関して受信した通知を表示できます。 I/Oコンソールの **統合** (Integrations)タブで、統合に移動し、「 **表示**」をクリックします。

![表示統合](images/privacy-events/view-integration.png)

統合の詳細ページが表示されます。 「 **イベント** 」をクリックして、統合のイベント登録を表示します。 プライバシーイベント登録を見つけ、「 **表示**」をクリックします。

![表示イベント登録](images/privacy-events/view-registration.png)

[ *イベントの詳細* ]ウィンドウが表示され、登録の詳細を表示したり、設定を編集したり、Webフックのアクティブ化後に受け取った実際のイベントを表示したりできます。 表示イベントの詳細を表示したり、[ **デバッグトレース** ]オプションに移動したりできます。

![トレースのデバッグ](images/privacy-events/debug-tracing.png)

「 **Payload** 」セクションには、上の例で強調表示されているイベントタイプ(`"com.adobe.platform.gdpr.productcomplete"`)を含む、選択したイベントに関する詳細が表示されます。

## 次の手順

必要に応じて、異なるWebフックアドレスに対して新しい統合を追加する場合、上記の手順を繰り返すことができます。