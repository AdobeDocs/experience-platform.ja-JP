---
title: 認証
description: Adobe Experience Platform Edge Networkサーバー API の認証を設定する方法について説明します。
exl-id: 73c7a186-9b85-43fe-a586-4c6260b6fa8c
source-git-commit: 3bf13c3f5ac0506ac88effc56ff68758deb5f566
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 24%

---

# 認証 {#authentication}

## 概要

[!DNL Edge Network Server API] は、イベントのソースと API 収集ドメインに応じて、認証済みデータ収集と未認証データ収集の両方を処理します。

リクエストごとに、[!DNL Server API] はデータストリームの [!DNL access type] 設定を検証します。 この設定を使用すると、認証済みデータ、または認証済みデータと未認証データの両方を受け入れるようにデータストリームを設定できます。 デフォルトでは、どちらのタイプのデータも受け入れられます。

データストリームのアクセスタイプの設定について詳しくは、[ データストリームの作成と設定 ](../datastreams/overview.md#create) 方法に関するドキュメントを参照してください。

データストリーム [!DNL Access Type] 設定とリクエストを受信したエンドポイントに基づいた、動作の概要を以下に示します。

| [!DNL Access Type] | edge.adobedc.net | server.adobedc.net |
|-----------------|-------------------------------|-----------------------|
| 混在（デフォルト） | リクエストを認証しない | リクエストを認証 |
| 認証済み | リクエストを認証 | リクエストを認証 |

`server.adobedc.net` のプライベートサーバーからの API 呼び出しは、常に認証される必要があります。

## 前提条件 {#prerequisites}

[!DNL Server API] を呼び出す前に、次の前提条件を満たしていることを確認してください。

* Adobe Experience Platformにアクセスできる組織アカウントがある。
* Experience Platformアカウントでは、`developer` ロールと `user` ロールがAdobe Experience Platform API 製品プロファイルに対して有効になっています。 アカウントでこれらのロールを有効にするには、[Admin Console](../access-control/home.md) 管理者にお問い合わせください。
* Adobe IDがある。 Adobe IDがない場合は、[Adobe Developer Consoleに移動して ](https://developer.adobe.com/console) 新しいアカウントを作成します。

## 資格情報の収集 {#credentials}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](../landing/api-authentication.md)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## データセット書き込み権限の設定 {#dataset-write-permissions}

データセットの書き込み権限を設定するには、[Admin Console](https://adminconsole.adobe.com) に移動し、API キーに添付されている製品プロファイルを見つけて、次の権限を設定します。

* 「[!UICONTROL  サンドボックス ]」セクションで、データストリームサンドボックスを選択します。
* 「[!UICONTROL  データ管理 ]」セクションで、「**[!UICONTROL データセットを管理]** 権限を選択します。

## 認証エラーのトラブルシューティング {#troubleshooting-authorization}

| エラーコード | エラーメッセージ | 説明 |
| --- | --- | --- |
| `EXEG-0500-401` | 認証トークンが無効です | このエラーメッセージは、次のいずれかの状況で表示されます。  <ul><li>`authorization` ヘッダー値がありません。</li><li>`authorization` ヘッダー値には、必須 `Bearer` トークンが含まれていません。</li><li>指定された認証トークンの形式が無効です。</li><li>データストリームには認証が必要ですが、リクエストに必要なヘッダーがありません。</li></ul> |
| `EXEG-0501-401` | 無効なユーザー認証トークン | このエラーメッセージは、次のいずれかの状況で表示されます。 <ul><li>API 呼び出しに必要な `x-user-token` ヘッダーがありません。</li><li>指定したユーザートークンの形式が無効です。</li></ul> |
| `EXEG-0502-401` | 認証トークンが無効です | このエラーメッセージが表示されるのは、指定された認証トークンの形式（JWT）が有効であるが、その署名が無効な場合です。 有効な JWT トークンの取得方法については、[ 認証チュートリアル ](../landing/api-authentication.md) を参照してください。 |
| `EXEG-0503-401` | 認証トークンが無効です | このエラーメッセージは、指定された認証トークンが期限切れの場合に表示されます。 [ 認証チュートリアル ](../landing/api-authentication.md) を実行して、新しいトークンを生成します。 |
| `EXEG-0504-401` | 必須製品コンテキストがありません | このエラーメッセージは、次のいずれかの状況で表示されます。  <ul><li>開発者アカウントには、Adobe Experience Platform製品コンテキストへのアクセス権がありません。</li><li>会社アカウントには、まだAdobe Experience Platform を使用する権限がありません。</li></ul> |
| `EXEG-0505-401` | 必須認証トークンスコープがありません | このエラーは、サービス アカウント認証にのみ適用されます。 このエラーメッセージが表示されるのは、呼び出しに含まれているサービス認証トークンが、`acp.foundation` IMS スコープへのアクセス権を持たないサービスアカウントに属している場合です。 |
| `EXEG-0506-401` | 書き込みのためにサンドボックスにアクセスできません | このエラーメッセージが表示されるのは、開発者アカウントが、データストリームが定義されたExperience Platformサンドボックス `WRITE` のアクセス権を持っていない場合です。 |
