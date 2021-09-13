---
keywords: Experience Platform；ホーム；人気のあるトピック；ストリーミング接続；ストリーミング接続の作成；apiガイド；チュートリアル；ストリーミング接続の作成；ストリーミング取得；取得；
solution: Experience Platform
title: APIを使用したストリーミング接続の作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルは、Adobe Experience Platform データ取得サービス API の一部であるストリーミング取得 API の使用を開始する際に役に立ちます。
exl-id: 9f7fbda9-4cd3-4db5-92ff-6598702adc34
source-git-commit: 4ceeac5a7ae4a57787b37f6758851c49e02aa909
workflow-type: tm+mt
source-wordcount: '1268'
ht-degree: 37%

---


# API を使用したストリーミング接続の作成

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して、フローサービスAPIを使用したストリーミング接続の作成手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Experience Data Model (XDM)]](../../../../../xdm/home.md):エクスペリエンスデータを整理する際に使用する標 [!DNL Platform] 準化されたフレームワーク。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合された消費者プロファイルをリアルタイムで提供します。

さらに、ストリーミング接続を作成するには、ターゲットXDMスキーマとデータセットが必要です。 これらのデータの作成方法については、[レコードデータのストリーミング](../../../../../ingestion/tutorials/streaming-record-data.md)に関するチュートリアル、または[時系列データのストリーミング](../../../../../ingestion/tutorials/streaming-time-series-data.md)に関するチュートリアルを参照してください。

以下の節では、ストリーミング取得 API の呼び出しを正常におこなうために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../../../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type： application/json

## ベース接続を作成する

ベース接続ではソースを指定し、フローをストリーミング取得APIと互換性を持たせるために必要な情報を含みます。 ベース接続を作成する場合、非認証接続と認証済み接続を作成するオプションがあります。

### 非認証接続

非認証接続は、データをPlatformにストリーミングする場合に作成できる標準のストリーミング接続です。

**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

ストリーミング接続を作成するには、POSTリクエストの一部としてプロバイダーIDと接続仕様IDを指定する必要があります。 プロバイダーIDは`521eee4d-8cbe-4906-bb48-fb6bd4450033`で、接続仕様IDは`bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb`です。

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample streaming connection",
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
| `auth.params.dataType` | ストリーミング接続のデータタイプ。 この値は`xdm`にする必要があります。 |
| `auth.params.name` | 作成するストリーミング接続の名前。 |
| `connectionSpec.id` | ストリーミング接続の接続仕様`id`。 |

**応答**

正常な応答は、HTTPステータス201と、新しく作成された接続の詳細(一意の識別子(`id`)を返します。

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

認証済みの接続は、信頼できるソースと信頼できないソースからのレコードを区別する必要がある場合に使用します。 個人情報(PII)を含む情報を送信するユーザーは、情報をPlatformにストリーミングする際に、認証済みの接続を作成する必要があります。

**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

ストリーミング接続を作成するには、POSTリクエストの一部としてプロバイダーIDと接続仕様IDを指定する必要があります。 プロバイダーIDは`521eee4d-8cbe-4906-bb48-fb6bd4450033`で、接続仕様IDは`bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb`です。

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample streaming connection",
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
| `auth.params.dataType` | ストリーミング接続のデータタイプ。 この値は`xdm`にする必要があります。 |
| `auth.params.name` | 作成するストリーミング接続の名前。 |
| `auth.params.authenticationRequired` | 作成したストリーミング接続を指定するパラメーター |
| `connectionSpec.id` | ストリーミング接続の接続仕様`id`。 |

**応答**

正常な応答は、HTTPステータス201と、新しく作成された接続の詳細(一意の識別子(`id`)を返します。

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

ベース接続が作成されたら、ストリーミングエンドポイントURLを取得できます。

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

リクエストが成功した場合は、リクエストした接続についての詳細情報と HTTP ステータス 200 が返されます。ストリーミングエンドポイントURLは、接続を使用して自動的に作成され、`inletUrl`値を使用して取得できます。

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

ベース接続を作成した後、ソース接続を作成する必要があります。 ソース接続を作成する場合、作成したベース接続の`id`値が必要です。

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

正常な応答は、HTTPステータス201と、新しく作成されたソース接続の詳細(一意の識別子(`id`)を返します。

```json
{
    "id": "63070871-ec3f-4cb5-af47-cf7abb25e8bb",
    "etag": "\"28000b90-0000-0200-0000-6091b0150000\""
}
```

## ターゲット接続の作成

ターゲット接続は、取り込んだデータの宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様IDを指定する必要があります。 この接続仕様IDは次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、ターゲットスキーマとターゲットデータセット、およびデータレイクへの接続仕様IDの一意の識別子が得られます。 これらの識別子を使用して、[!DNL Flow Service] APIを使用してターゲット接続を作成し、受信ソースデータを格納するデータセットを指定できます。

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

正常な応答は、HTTPステータス201と、新しく作成されたターゲット接続の詳細(一意の識別子(`id`)を返します。

```json
{
    "id": "98a2a72e-a80f-49ae-aaa3-4783cc9404c2",
    "etag": "\"0500b73f-0000-0200-0000-6091b0b90000\""
}
```

## データフローの作成

ソース接続とターゲット接続を作成したら、データフローを作成できます。 データフローは、ソースからデータをスケジュールおよび収集する役割を担います。 `/flows`エンドポイントに対してPOSTリクエストを実行すると、データフローを作成できます。

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

正常な応答は、HTTPステータス201と、新しく作成したデータフローの詳細(一意の識別子(`id`)を返します。

```json
{
    "id": "ab03bde0-86f2-45c7-b6a5-ad8374f7db1f",
    "etag": "\"1200c123-0000-0200-0000-6091b1730000\""
}
```

## 次の手順

このチュートリアルに従って、ストリーミングHTTP接続を作成し、ストリーミングエンドポイントを使用してデータをPlatformに取り込むことができます。 UIでストリーミング接続を作成する手順については、[ストリーミング接続の作成に関するチュートリアル](../../../ui/create/streaming/http.md)を参照してください。

データをPlatformにストリーミングする方法については、[時系列データのストリーミング](../../../../../ingestion/tutorials/streaming-time-series-data.md)に関するチュートリアル、または[レコードデータのストリーミング](../../../../../ingestion/tutorials/streaming-record-data.md)に関するチュートリアルを参照してください。

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

### Platformに取り込む生データの後処理 {#ingest-data}

フローを作成したら、以前に作成したストリーミングエンドポイントにJSONメッセージを送信できます。

**API 形式**

```http
POST /collection/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 新しく作成されたストリーミング接続の `id` 値。 |

**リクエスト**

この例のリクエストは、以前に作成されたストリーミングエンドポイントに生データを取り込みます。

```shell
curl -X POST https://dcs.adobedc.net/collection/2301a1f761f6d7bf62c5312c535e1076bbc7f14d728e63cdfd37ecbb4344425b \
  -H 'Content-Type: application/json' \
  -H 'x-adobe-flow-id: 1f086c23-2ea8-4d06-886c-232ea8bd061d' \
  -d '{
      "name": "Johnson Smith",
      "location": {
          "city": "Seattle",
          "country": "United State of America",
          "address": "3692 Main Street"
      },
      "gender": "Male",
      "birthday": {
          "year": 1984,
          "month": 6,
          "day": 9
      }
  }'
```

**応答**

正常な応答は、HTTPステータス200と、新しく取り込んだ情報の詳細を返します。

```json
{
    "inletId": "{CONNECTION_ID}",
    "xactionId": "1584479347507:2153:240",
    "receivedTimeMs": 1584479347507
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続の ID。 |
| `xactionId` | 送信したレコードに対してサーバー側で生成された一意の ID です。この IDは、様々なシステムやデバッグを通じて、アドビがこのレコードのライフサイクルを追跡するのに役立ちます。 |
| `receivedTimeMs` | リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポックタイム）。 |
