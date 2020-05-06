---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 認証済みストリーミング接続の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: d9ce9506e43c4deed01f18e5913fda5a5c3cee84
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 2%

---


# 認証済みストリーミング接続の作成

認証済みデータ収集機能を使用すると、リアルタイム顧客プロファイルやIDなどのAdobe Experience Platformサービスで、信頼できるソースからのレコードと信頼できないソースからのレコードを区別できます。 個人識別情報(PII)を送信したい顧客は、POSTリクエストの一環としてアクセストークンを送信することで、送信できます。

## はじめに

Adobe Experience Platformに開始ストリーミングデータを接続するには、ストリーミング接続の登録が必要です。 ストリーミング接続を登録する場合は、ストリーミングデータのソースなど、主な詳細情報を入力する必要があります。

ストリーミング接続を登録すると、データプロデューサーとして、ユーザーは一意のURLを持ち、このURLを使用してプラットフォームにデータをストリーミングできます。

このチュートリアルでは、様々なAdobe Experience Platformサービスの実用的な知識も必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームがエクスペリエンスデータを編成する際に使用する、標準化されたフレームワークです。
- [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。

以下の節では、ストリーミング取り込みAPIの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## 接続の作成

接続は、ソースを指定し、フローをストリーミング取り込みAPIと互換性を持たせるために必要な情報を含みます。

**API形式**

```http
POST /flowservice/connections
```

**リクエスト**

>[!NOTE] 例に示すように、リスト `providerId` ととの値を使用する `connectionSpec` 必要があります **** 。これは、ストリーミング取り込み用にストリーミング接続を作成するAPIに対して何が指定されているかを示す値です。

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

正常に応答すると、新たに作成された接続の詳細と共にHTTPステータス201が返されます。

```json
{
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成 `id` した接続の。 これは、を参照してください `{CONNECTION_ID}`。 |
| `etag` | 接続に割り当てられる識別子。接続のリビジョンを指定します。 |

## データ収集URLの取得

作成した接続で、データ収集URLを取得できるようになります。

**API形式**

```http
GET /flowservice/connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 前に作成した接続の `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/flowservice/connections/{CONNECTION_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTPステータス200が返され、要求された接続に関する詳細情報が返されます。 データ収集URLは接続時に自動的に作成され、 `inletUrl` 値を使用して取得できます。

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

認証済みのストリーミング接続を作成したら、時系列またはデータを記録して、プラットフォーム内のデータを取り込むことができます。 時系列データをプラットフォームにストリーミングする方法については、「 [ストリーミング時系列データのチュートリアル](./streaming-time-series-data.md)」を参照してください。 レコードデータをプラットフォームにストリーミング再生する方法については、「 [ストリーミングレコードデータのチュートリアル](./streaming-record-data.md)」を参照してください。

## 付録

ここでは、認証済みストリーミング接続についての補足情報を説明します。

### 認証済みストリーミング接続へのメッセージの送信

ストリーミング接続で認証が有効になっている場合、クライアントは要求に `Authorization` ヘッダーを追加する必要があります。

ヘッダーが存在しない場合、 `Authorization` または無効な/期限切れのアクセストークンが送信された場合、HTTP 401 Unauthorized responseが返され、次のような応答が返されます。

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
