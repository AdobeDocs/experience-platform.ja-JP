---
keywords: Experience Platform;home;popular topics;authenticated streaming connection;streaming connection;create streaming connection;create authenticated streaming connection;streaming ingestion;ingestion;
solution: Experience Platform
title: 認証済みストリーミング接続の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: c04fb056d4564e53f192e0734a700a13820f5ba7
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 67%

---


# 認証済みストリーミング接続の作成

Authenticated Data Collection allows Adobe Experience Platform services, such as [!DNL Real-time Customer Profile] and [!DNL Identity], to differentiate between records coming from trusted sources and un-trusted sources. 個人を特定できる情報（PII）を送信するクライアントがレコードを区別するには、POST リクエストの一部としてアクセストークンを送信します。

## はじめに

Adobe Experience Platform へのデータストリーミングを開始するには、ストリーミング接続を登録する必要があります。ストリーミング接続を登録する場合、ストリーミングデータのソースなど、重要な情報をいくつか指定する必要があります。

After registering a streaming connection, you, as the data producer, will have a unique URL which can be used to stream data to [!DNL Platform].

このチュートリアルでは、様々な Adobe Experience Platform サービスに関する実用的な知識も必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):エクスペリエンスデータを [!DNL Platform] 編成するための標準化されたフレームワーク。
- [[!DNLリアルタイム顧客プロファイル]](../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。

以下の節では、ストリーミング取得 API の呼び出しを正常におこなうために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## 接続の作成

接続ではソースを指定します。また、接続には、Streaming Ingestion API と互換性があるフローを作成するために必要な情報を含めます。

**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

>[!NOTE]
>
>次の例に示すように、`providerId` と `connectionSpec` の値を使用する&#x200B;**必要があります**。これらの値により、ストリーミングインジェスト用のストリーミング接続を作成していることが API に示されます。

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample name",
     "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
     "description": "Sample description",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Sample connection",
             "dataType": "xdm",
             "name": "Sample connection",
             "authenticationRequired": true
         }
     }
 }
```

**応答**

リクエストが成功した場合は、新しく作成した接続の詳細と HTTP ステータス 201 が返されます。

```json
{
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成した接続の `id`。ここでは、`{CONNECTION_ID}` を指します。 |
| `etag` | 接続に割り当てられ、接続のリビジョンを指定する識別子。 |

## データ収集 URL の取得

接続が作成されたら、データ収集 URL を取得することができます。

**API 形式**

```http
GET /flowservice/connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に作成した接続の `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/flowservice/connections/{CONNECTION_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、リクエストした接続についての詳細情報と HTTP ステータス 200 が返されます。データ収集 URL は、接続を使用して自動的に作成されます。これは、`inletUrl` 値を使って取得できます。

```json
{
    "items": [
        {
            "createdAt": 1583971856947,
            "updatedAt": 1583971856947,
            "createdBy": "{API_KEY}",
            "updatedBy": "{API_KEY}",
            "createdClient": "{USER_ID}",
            "updatedClient": "{USER_ID}",
            "id": "77a05521-91d6-451c-a055-2191d6851c34",
            "name": "Another new sample connection (Experience Event)",
            "description": "Sample description",
            "connectionSpec": {
                "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
                "version": "1.0"
            },
            "state": "enabled",
            "auth": {
                "specName": "Streaming Connection",
                "params": {
                    "sourceId": "Sample connection (ExperienceEvent)",
                    "inletUrl": "https://dcs.adobedc.net/collection/a868e1ce678a911ef1482b083329af3cafa4bafdc781285f25911eaae9e00eb2",
                    "inletId": "a868e1ce678a911ef1482b083329af3cafa4bafdc781285f25911eaae9e00eb2",
                    "dataType": "xdm",
                    "name": "Sample connection (ExperienceEvent)"
                }
            },
            "version": "\"56008aee-0000-0200-0000-5e697e150000\"",
            "etag": "\"56008aee-0000-0200-0000-5e697e150000\""
        }
    ]
}
```

## 次の手順

Now that you have created an authenticated streaming connection, you can stream either time series or record data, allowing you to ingest data within [!DNL Platform]. To learn how to stream time series data to [!DNL Platform], go to the [streaming time series data tutorial](./streaming-time-series-data.md). To learn how to stream record data to [!DNL Platform], go to the [streaming record data tutorial](./streaming-record-data.md).

## 付録

この節では、認証済みのストリーミング接続に関する補足情報を示します。

### 認証済みストリーミング接続にメッセージを送信する

ストリーミング接続で認証が有効になっている場合、クライアントはリクエストに `Authorization` ヘッダーを追加する必要があります。

`Authorization` ヘッダーがない場合、または無効な／期限切れのアクセストークンが送信された場合は、HTTP 401 Unauthorized と以下のようなレスポンスが返されます。

**応答**

```json
{
    "type": "https://ns.adobe.com/adobecloud/problem/data-collection-service-authorization",
    "status": "401",
    "title": "Authorization",
    "report": {
        "message": "[id] Ims service token is empty"
    }
}
```
