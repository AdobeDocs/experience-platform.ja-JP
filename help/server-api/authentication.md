---
title: 認証
description: Adobe Experience Platform Edge Network Server API の認証を設定する方法について説明します。
exl-id: 73c7a186-9b85-43fe-a586-4c6260b6fa8c
source-git-commit: 3d0f2823dcf63f25c3136230af453118c83cdc7e
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 19%

---

# 認証 {#authentication}

## 概要

この [!DNL Edge Network Server API] は、イベントのソースと API 収集ドメインに応じて、認証済みと未認証の両方のデータ収集を処理します。

リクエストごとに、 [!DNL Server API] データストリームを検証します。 [!DNL access type] 設定。 この設定を使用すると、お客様は認証済みデータを受け入れるか、認証済みデータと未認証データの両方を受け入れるようにデータストリームを設定できます。 デフォルトでは、両方のタイプのデータが受け入れられます。

データストリームアクセスタイプの設定について詳しくは、 [データストリームの作成と設定](../datastreams/overview.md#create).

以下に、データストリームに基づく動作の概要を示します [!DNL Access Type] 設定およびリクエストを受信したエンドポイント。

| [!DNL Access Type] | edge.adobedc.net | server.adobedc.net |
|-----------------|-------------------------------|-----------------------|
| mixed （デフォルト） | リクエストを認証しません | 要求を認証します |
| 認証済み | 要求を認証します | 要求を認証します |

のプライベートサーバーからの API 呼び出し `server.adobedc.net` は常に認証される必要があります。

## 前提条件 {#prerequisites}

を呼び出す前に、 [!DNL Server API]を使用する場合は、次の前提条件を満たしていることを確認します。

* Adobe Experience Platformへのアクセス権を持つ組織アカウントがある。
* Experience Platformアカウントに `developer` および `user` 役割がAdobe Experience Platform API 製品プロファイルで有効になっていること お問い合わせ [Admin Console](../access-control/home.md) 管理者：アカウントに対してこれらのロールを有効にします。
* あなたはAdobe IDを持っている。 Adobe IDがない場合は、 [Adobe Developer Console](https://developer.adobe.com/console) 新しいアカウントを作成します。

##  資格情報の収集 {#credentials}

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

データセットの書き込み権限を設定するには、 [Admin Console](https://adminconsole.adobe.com)で、API キーに関連付けられている製品プロファイルを探し、次の権限を設定します。

* 内 [!UICONTROL サンドボックス] 「 」セクションで、データストリームサンドボックスを選択します。
* 内 [!UICONTROL データ管理] セクションで、 **[!UICONTROL データセットの管理]** 権限。

## 認証エラーのトラブルシューティング {#troubleshooting-authorization}

| エラーコード | エラーメッセージ | 説明 |
| --- | --- | --- |
| `EXEG-0500-401` | 無効な認証トークン | このエラーメッセージは、次のいずれかの状況で表示されます。  <ul><li>この `authorization` ヘッダー値がありません。</li><li>この `authorization` ヘッダーの値に必要な値が含まれていません `Bearer` トークン。</li><li>指定された認証トークンの形式が無効です。</li><li>データストリームには認証が必要ですが、リクエストには必須ヘッダーがありません。</li></ul> |
| `EXEG-0501-401` | 無効なユーザー認証トークン | このエラーメッセージは、次のいずれかの状況で表示されます。 <ul><li>API 呼び出しに必要ながありません `x-user-token` ヘッダー。</li><li>指定されたユーザートークンの形式が無効です。</li></ul> |
| `EXEG-0502-401` | 無効な認証トークン | このエラーメッセージは、指定された認証トークンの形式 (JWT) が有効で、署名が無効な場合に表示されます。 次を確認します。 [認証チュートリアル](../landing/api-authentication.md) 有効な JWT トークンの取得方法を参照してください。 |
| `EXEG-0503-401` | 無効な認証トークン | このエラーメッセージは、指定された認証トークンの有効期限が切れると表示されます。 詳しくは、 [認証チュートリアル](../landing/api-authentication.md) をクリックして新しいトークンを生成します。 |
| `EXEG-0504-401` | 必要な製品コンテキストがありません | このエラーメッセージは、次のいずれかの状況で表示されます。  <ul><li>開発者アカウントはAdobe Experience Platform製品コンテキストにアクセスできません。</li><li>この会社アカウントには、まだ Experience Platform をAdobeする資格がありません。</li></ul> |
| `EXEG-0505-401` | 必要な認証トークンスコープがありません | このエラーは、サービスアカウント認証にのみ適用されます。 このエラーメッセージは、呼び出しに含まれるサービス認証トークンが、 `acp.foundation` IMS スコープ。 |
| `EXEG-0506-401` | 書き込み用のサンドボックスにアクセスできません | このエラーメッセージは、開発者アカウントに `WRITE` datastream が定義されているExperience Platformサンドボックスへのアクセス |
