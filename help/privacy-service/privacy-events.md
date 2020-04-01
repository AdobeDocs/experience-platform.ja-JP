---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーイベント
topic: privacy events
translation-type: tm+mt
source-git-commit: e4cd042722e13dafc32b059d75fca2dab828df60

---


# プライバシーイベント

プライバシーイベントとは、Adobe Experience Platform Privacy Serviceが提供するメッセージで、設定済みのWebフックに送信されるAdobe I/Oイベントを利用して、効率的なジョブリクエストの自動化を促進します。 ジョブが完了したか、またはワークフロー内の特定のマイルストーンに達したかを確認するために、プライバシーサービスAPIをポーリングする必要性を減らすか、なくします。

現在、プライバシージョブリクエストのライフサイクルに関連する通知には、次の4種類があります。

| タイプ | 説明 |
--- | ---
| ジョブ完了 | すべてのExperience Cloudソリューションからレポートが返され、ジョブの全体的なステータスまたはグローバルステータスが完了とマークされました。 |
| ジョブエラー | 1つ以上のソリューションが、要求の処理中にエラーを報告しました。 |
| 製品完了 | このジョブに関連付けられているソリューションの1つが、その作業を完了しました。 |
| 製品エラー | いずれかの解決策で、要求の処理中にエラーが報告されました。 |

このドキュメントでは、Adobe I/O内でプライバシーサービス通知の統合を設定する手順を説明します。プライバシーサービスとその機能の概要については、プライバシーサービスの概要を参 [照してください](home.md)。

## はじめに

このチュートリアルで **は**、ローカルサーバを安全なトンネルを通じてパブリックインターネットに公開するソフトウェア製品、ngrokを使用します。 このチュ [ートリアルを開始する前に](https://ngrok.com/download) 、ngrokをインストールして、ローカルマシンにWebフックを作成してください。 また、このガイドでは、 [Node.jsで書き込まれたシンプルなサーバーを含むGITリポジトリをダウンロードする必要があります](https://nodejs.org/)。

## ローカルサーバーの作成

Node.jsサーバーは、要求によってルート( `challenge` )エンドポイントに送信されたパラメーターを返す必要`/`があります。 これを行うには、 `index.js` 次のJavaScriptを使用してファイルを設定します。

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

これらのコマンドは、すべての依存関係をインストールし、サーバーを初期化します。 成功した場合は、サーバーがhttp://localhost:3000/で実行されていることを確認できます。

## ngrokを使用してWebフックを作成する

同じディレクトリ内および新しいコマンドラインウィンドウで、次のコマンドを入力します。

```shell
ngrok http -bind-tls=true 3000
```

出力が成功すると、次のようになります。

![ngrok出力](images/privacy-events/ngrok-output.png)

次の手順でWebフックを `Forwarding` 識別す`https://e142b577.ngrok.io`るために使用されるURL()を控えておきます。

## Adobe I/Oコンソールを使用した新しい統合の作成

Adobe I/O Consoleにサ [インインし、](https://console.adobe.io) 「統合」タブをクリ **ックします** 。 「統合」ウ _ィンドウ_ が表示されます。 ここから、[新しい統合]を **クリックしま**&#x200B;す。

![Adobe I/Oコンソールでの表示統合](images/privacy-events/integrations.png)

[新しい *統合の作成* ]ウィンドウが表示されます。 「ニア **リアルタイムイベントを受信**」を選択し、「続行」をクリ **ックします**。

![新しい統合の作成](images/privacy-events/new-integration.png)

次の画面には、購読、権限、権限に基づいて、組織が使用できる様々なイベント、製品、サービスを使用した統合を作成するオプションが表示されます。 この統合の場合は、「プライバシーサ **ービスのイベント**」を選択し、「続行」をクリ **ックしま**&#x200B;す。

![プライバシーイベント](images/privacy-events/privacy-events.png)

統合 *の詳細* ・フォームが表示され、統合の名前と説明、および公開鍵証明書を入力する必要があります。

![統合の詳細](images/privacy-events/integration-details.png)

公開証明書がない場合は、次のターミナルコマンドを使用して証明書を生成できます。

```shell
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub
```

証明書を生成したら、「 **Public keys certificates** 」ボックスにファイルをドラッグ&amp;ドロップするか、「 **Select a File** 」をクリックしてファイルディレクトリを参照し、証明書を直接選択します。

証明書を追加すると、「登録」オプ *ションが* イベントされます。 「 **追加イベント登録」をクリックします**。

![追加イベント登録](images/privacy-events/add-event-registration.png)

ダイアログが展開され、追加のコントロールが表示されます。 ここでは、目的のオプションを選択し、Webイベントタイプを登録できます。 イベント登録の名前、WebフックのURL(Webフックを最初に作成し `Forwarding` たときに返され [るアドレス](#create-a-webhook-using-ngrok))、簡単な説明を入力します。 最後に、購読するイベントタイプを選択し、「保存」をクリック **します**。

![イベント登録フォーム](images/privacy-events/event-registration-form.png)

イベント登録フォームが完了したら、[統合の作成 **** ]をクリックします。I/O統合が完了します。

![統合の作成](images/privacy-events/create-integration.png)

## 表示イベントデータ

I/O統合とプライバシージョブを作成したら、その統合に関する受信通知を表示できます。 I/Oコンソ **ールの** 「統合」タブから統合に移動し、「統合」をクリック **表示**。

![表示統合](images/privacy-events/view-integration.png)

統合の詳細ページが表示されます。 「 **イベント** 」をクリックして、統合のイベント登録を登録します。 プライバシーイベントの登録を探し、「 **表示」をクリックしま**&#x200B;す。

![表示イベント登録](images/privacy-events/view-registration.png)

[ *イベントの詳細* ]ウィンドウが表示され、登録の詳細を表示したり、設定を編集したり、Webフックのアクティブ化後に受け取った実際のイベントを表示したりできます。 表示イベントの詳細を表示し、「トレースのデバッグ」オプシ **ョンに移動できま** す。

![トレースのデバッグ](images/privacy-events/debug-tracing.png)

「ペイ **ロード** 」セクションには、上の例で強調表示されているイベントタイプ(`"com.adobe.platform.gdpr.productcomplete"`)を含む、選択したイベントの詳細が表示されます。

## 次の手順

必要に応じて、異なるWebフックアドレスに対して新しい統合を追加する場合に、上記の手順を繰り返すことができます。