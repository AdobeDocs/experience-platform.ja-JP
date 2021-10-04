---
keywords: Experience Platform；ホーム；人気のあるトピック；bigquery;Google;google;Google BigQuery
solution: Experience Platform
title: フローサービス API を使用した Google BigQuery ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Google BigQuery に接続する方法を説明します。
exl-id: 51f90366-7a0e-49f1-bd57-b540fa1d15af
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 10%

---

# [!DNL Flow Service] API を使用して [!DNL Google BigQuery] ベース接続を作成する

>[!NOTE]
>
>[!DNL Google BigQuery] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Google BigQuery]（以下「[!DNL BigQuery]」と呼ばれます）の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL BigQuery] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL BigQuery] を Platform に接続するには、次の OAuth 2.0 認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | クエリを実行するデフォルトの [!DNL BigQuery] プロジェクトのプロジェクト ID。 |
| `clientID` | 更新トークンの生成に使用する ID 値。 |
| `clientSecret` | 更新トークンの生成に使用するシークレット値。 |
| `refreshToken` | [!DNL Google] から取得した更新トークン。[!DNL BigQuery] へのアクセスを承認するために使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL BigQuery] の接続仕様 ID は次のとおりです。`3c9b37f8-13a6-43d8-bad3-b863b941fedd`. |

これらの値の詳細については、[[!DNL BigQuery]  ドキュメント ](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL BigQuery] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL BigQuery] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google BigQuery connection",
        "description": "Google BigQuery connection",
        "auth": {
            "specName": "Basic Authentication",
            "type": "OAuth2.0",
            "params": {
                    "project": "{PROJECT}",
                    "clientId": "{CLIENT_ID},
                    "clientSecret": "{CLIENT_SECRET}",
                    "refreshToken": "{REFRESH_TOKEN}"
                }
        },
        "connectionSpec": {
            "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.project` | クエリを実行するデフォルトの [!DNL BigQuery] プロジェクトのプロジェクト ID。 対して |
| `auth.params.clientId` | 更新トークンの生成に使用する ID 値。 |
| `auth.params.clientSecret` | 更新トークンの生成に使用するクライアント値。 |
| `auth.params.refreshToken` | [!DNL Google] から取得した更新トークン。[!DNL BigQuery] へのアクセスを承認するために使用されます。 |
| `connectionSpec.id` | [!DNL Google BigQuery] 接続仕様 ID:`3c9b37f8-13a6-43d8-bad3-b863b941fedd`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6990abad-977d-41b9-a85d-17ea8cf1c0e4",
    "etag": "\"ca00acbf-0000-0200-0000-60149e1e0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL BigQuery] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID は、次のチュートリアルで [ フローサービス API](../../explore/database-nosql.md) を使用してデータベースや NoSQL システムを調べる方法を学ぶ際に使用できます。
