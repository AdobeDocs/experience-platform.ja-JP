---
keywords: Experience Platform；ホーム；人気のあるトピック；ストリーミング接続；ストリーミング接続の作成；apiガイド；チュートリアル；ストリーミング接続の作成；ストリーミング取り込み；取り込み；
solution: Experience Platform
title: APIを使用したストリーミング接続の作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルは、Adobe Experience Platform データ取得サービス API の一部であるストリーミング取得 API の使用を開始する際に役に立ちます。
exl-id: 9f7fbda9-4cd3-4db5-92ff-6598702adc34
translation-type: tm+mt
source-git-commit: 96f400466366d8a79babc194bc2ba8bf19ede6bb
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 33%

---


# API を使用したストリーミング接続の作成

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元管理するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、Flow Service APIを使用したストリーミング接続の作成手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Experience Data Model (XDM)]](../../../../../xdm/home.md):エクスペリエンスデータを [!DNL Platform] 編成する際に使用される標準化されたフレームワーク。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された消費者プロファイルを提供します。

また、ストリーミング接続を作成するには、ターゲットXDMスキーマとデータセットが必要です。 これらの作成方法を学ぶには、[ストリーミングレコードデータ](../../../../../ingestion/tutorials/streaming-record-data.md)のチュートリアル、または[ストリーミング時系列データ](../../../../../ingestion/tutorials/streaming-time-series-data.md)のチュートリアルをお読みください。

以下の節では、ストリーミング取得 API の呼び出しを正常におこなうために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../../../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## ベース接続を作成する

ベース接続は、ソースを指定し、フローをストリーミング取り込みAPIと互換性を持たせるために必要な情報を含みます。 ベース接続を作成する場合、非認証接続と認証済み接続を作成できます。

### 非認証接続

非認証接続は、データをプラットフォームにストリーミングする場合に作成できる標準のストリーミング接続です。

**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

ストリーミング接続を作成するには、POST要求の一部としてプロバイダIDと接続指定IDを指定する必要があります。 プロバイダーIDは`521eee4d-8cbe-4906-bb48-fb6bd4450033`で、接続指定IDは`bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb`です。

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample streaming connection",
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
             "name": "Sample connection"
         }
     }
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.sourceId` | 作成するストリーミング接続のID。 |
| `auth.params.dataType` | ストリーミング接続のデータタイプです。 この値は`xdm`にする必要があります。 |
| `auth.params.name` | 作成するストリーミング接続の名前。 |
| `connectionSpec.id` | ストリーミング接続の接続仕様`id`。 |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)を含む)と共に、HTTPステータス201が返されます。

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

### 認証済み接続

認証済みの接続は、信頼できるソースと信頼できないソースからのレコードを区別する必要がある場合に使用します。 個人情報(PII)と共に情報を送信したいユーザーは、情報をプラットフォームにストリーミングする際に、認証済みの接続を作成する必要があります。

**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

ストリーミング接続を作成するには、POST要求の一部としてプロバイダIDと接続指定IDを指定する必要があります。 プロバイダーIDは`521eee4d-8cbe-4906-bb48-fb6bd4450033`で、接続指定IDは`bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb`です。

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample streaming connection",
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


| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.sourceId` | 作成するストリーミング接続のID。 |
| `auth.params.dataType` | ストリーミング接続のデータタイプです。 この値は`xdm`にする必要があります。 |
| `auth.params.name` | 作成するストリーミング接続の名前。 |
| `auth.params.authenticationRequired` | 作成したストリーミング接続を指定するパラメーター |
| `connectionSpec.id` | ストリーミング接続の接続仕様`id`。 |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)を含む)と共に、HTTPステータス201が返されます。

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

## ストリーミングエンドポイントURLの取得

ベース接続を作成すると、ストリーミングエンドポイントのURLを取得できるようになります。

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

リクエストが成功した場合は、リクエストした接続についての詳細情報と HTTP ステータス 200 が返されます。ストリーミングエンドポイントURLは、接続時に自動的に作成され、`inletUrl`値を使用して取得できます。

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

## ソース接続の作成

ベース接続を作成した後、ソース接続を作成する必要があります。 ソース接続を作成する場合は、作成したベース接続の`id`値が必要です。

**API 形式**

```http
POST /flowservice/sourceConnections
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name": "Sample source connection",
    "description": "Sample source connection description",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
        "version": "1.0"
    }
}'
```

**応答** 

正常に応答すると、HTTPステータス201が返され、一意の識別子(`id`)を含む、新たに作成されたソース接続の詳細が返されます。

```json
{
    "id": "63070871-ec3f-4cb5-af47-cf7abb25e8bb",
    "etag": "\"28000b90-0000-0200-0000-6091b0150000\""
}
```

## ターゲット接続の作成

ソース接続を作成したら、ターゲット接続を作成できます。 ターゲット接続を作成する場合は、以前に作成したデータセットの`id`値が必要です。

**API 形式**

```http
POST /flowservice/targetConnections
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name": "Sample target connection",
    "description": "Sample target connection description",
    "connectionSpec": {
        "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
        "version": "1.0"
    },
    "data": {
        "format": "parquet_xdm"
    },
    "params": {
        "dataSetId": "{DATASET_ID}"
    }
}'
```

**応答** 

正常に応答すると、新たに作成されたターゲット接続の詳細(一意の識別子(`id`)を含む)と共に、HTTPステータス201が返されます。

```json
{
    "id": "98a2a72e-a80f-49ae-aaa3-4783cc9404c2",
    "etag": "\"0500b73f-0000-0200-0000-6091b0b90000\""
}
```

## データフローの作成

ソース接続とターゲット接続を作成した後、データフローを作成できます。 データフローは、ソースからデータをスケジューリングおよび収集する役割を持ちます。 `/flows`エンドポイントに対してPOST要求を実行すると、データフローを作成できます。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name": "Sample flow",
    "description": "Sample flow description",
    "flowSpec": {
        "id": "d8a6f005-7eaf-4153-983e-e8574508b877",
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "{SOURCE_CONNECTION_ID}"
    ],
    "targetConnectionIds": [
        "{TARGET_CONNECTION_ID}"
    ]
}'
```

**応答** 

正常に応答すると、HTTPステータス201が返され、一意の識別子(`id`)を含む新しく作成されたデータフローの詳細が返されます。

```json
{
    "id": "ab03bde0-86f2-45c7-b6a5-ad8374f7db1f",
    "etag": "\"1200c123-0000-0200-0000-6091b1730000\""
}
```

## 次の手順

このチュートリアルに従って、ストリーミングHTTP接続を作成し、ストリーミングエンドポイントを使用してデータをプラットフォームに取り込むことができます。 UIでのストリーミング接続の作成手順については、[ストリーミング接続の作成のチュートリアル](../../../ui/create/streaming/http.md)を参照してください。

プラットフォームにデータをストリーミングする方法を学ぶには、[ストリーミング時系列データ](../../../../../ingestion/tutorials/streaming-time-series-data.md)のチュートリアルか、[ストリーミングレコードデータ](../../../../../ingestion/tutorials/streaming-record-data.md)のチュートリアルをお読みください。

## 付録

この節では、API を使用したストリーミング接続の作成に関する補足情報を提供します。

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
