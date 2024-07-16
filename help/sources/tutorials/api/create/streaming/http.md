---
keywords: Experience Platform；ホーム；人気のトピック；ストリーミング接続；ストリーミング接続の作成；api ガイド；チュートリアル；ストリーミング接続の作成；ストリーミング取得；取り込み；
title: Flow Service API を使用した HTTP API ストリーミング接続の作成
description: このチュートリアルでは、Flow Service API を使用して、生データと XDM データの両方に HTTP API ソースを使用してストリーミング接続を作成する手順を説明します
exl-id: 9f7fbda9-4cd3-4db5-92ff-6598702adc34
source-git-commit: 84ffbb86e8973c2795d19122d3866e980949759d
workflow-type: tm+mt
source-wordcount: '1658'
ht-degree: 34%

---


# [!DNL Flow Service] API を使用した HTTP API ストリーミング接続の作成

フローサービスは、Adobe Experience Platform内の様々なソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされているすべてのソースを接続できます。

このチュートリアルでは [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Flow Service] API でストリーミング接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)]](../../../../../xdm/home.md):[!DNL Platform] がエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

さらに、ストリーミング接続を作成するには、ターゲット XDM スキーマとデータセットが必要です。 これらを作成する方法については、[ レコードデータのストリーミング ](../../../../../ingestion/tutorials/streaming-record-data.md) または [ 時系列データのストリーミング ](../../../../../ingestion/tutorials/streaming-time-series-data.md) に関するチュートリアルを参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続はソースを指定し、フローをストリーミング取得 API と互換性を持たせるために必要な情報が含まれています。 ベース接続を作成する際には、非認証接続と認証接続を作成するオプションがあります。

### 認証されていない接続

非認証接続は、データを Platform にストリーミングする際に作成できる標準のストリーミング接続です。

認証されていないベースPOSTを作成するには、`/connections` エンドポイントに接続リクエストを行い、接続名、データタイプ、HTTP API 接続仕様 ID を指定します。 この ID は `bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb` です。

**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

次のリクエストは、HTTP API のベース接続を作成します。

>[!BEGINTABS]

>[!TAB XDM]

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
    "name": "ACME Streaming Connection XDM Data",
    "description": "ACME streaming connection for customer data",
    "connectionSpec": {
        "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
        "version": "1.0"
    },
    "auth": {
      "specName": "Streaming Connection",
      "params": {
        "dataType": "xdm"
      }
    }
  }'
```

>[!TAB  生データ ]

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
    "name": "ACME Streaming Connection Raw Data",
    "description": "ACME streaming connection for customer data",
    "connectionSpec": {
        "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
        "version": "1.0"
    },
    "auth": {
      "specName": "Streaming Connection",
      "params": {
        "dataType": "raw"
      }
    }
  }'
```

>[!ENDTABS]

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。ベース接続の情報を検索する際に使用できるので、わかりやすい名前にしてください。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | HTTP API に対応する接続仕様 ID。 この ID は `bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb` です。 |
| `auth.params.dataType` | ストリーミング接続のデータタイプ。 サポートされる値：`xdm` および `raw`。 |
| `auth.params.name` | 作成するストリーミング接続の名前。 |

**応答**

リクエストが成功した場合は、HTTP ステータス 201 が、一意の ID （`id`）など、新しく作成された接続の詳細と共に返されます。

```json
{
  "id": "a59d368a-1152-4673-a46e-bd52e8cdb9a9",
  "etag": "\"f50185ed-0000-0200-0000-637e8fad0000\""
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成したベース接続の `id`。 |
| `etag` | 接続に割り当てられる識別子。ベース接続のバージョンを指定します。 |

### 認証済みの接続

信頼できるソースと信頼できないソースのレコードを区別する必要がある場合は、認証済み接続を使用する必要があります。 個人情報（PII）を使用して情報を送信するユーザーは、情報を Platform にストリーミングする際に、認証済みの接続を作成する必要があります。

認証済みベース接続を作成するには、リクエストに `authenticationRequired` パラメーターを含め、その値を `true` として指定する必要があります。 この手順では、認証済みベース接続用のソース ID を指定することもできます。 このパラメーターはオプションで、指定されていない場合、`name` 属性と同じ値を使用します。


**API 形式**

```http
POST /flowservice/connections
```

**リクエスト**

次のリクエストは、HTTP API 用の認証済みベース接続を作成します。

>[!BEGINTABS]

>[!TAB XDM]

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "ACME Streaming Connection XDM Data Authenticated",
     "description": "ACME streaming connection for customer data",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Authenticated XDM streaming connection",
             "dataType": "xdm",
             "name": "Authenticated XDM streaming connection",
             "authenticationRequired": true
         }
     }
 }
```

>[!TAB  生データ ]

```shell
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "ACME Streaming Connection Raw Data Authenticated",
     "description": "ACME streaming connection for customer data",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Authenticated raw streaming connection",
             "dataType": "raw",
             "name": "Authenticated raw streaming connection",
             "authenticationRequired": true
         }
     }
 }
```

>[!ENDTABS]

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.sourceId` | 認証済みのベース接続を作成する際に使用できる追加の識別子。 このパラメーターはオプションで、指定されていない場合、`name` 属性と同じ値を使用します。 |
| `auth.params.authenticationRequired` | このパラメーターは、ストリーミング接続で認証が必要かどうかを指定します。 `authenticationRequired` を `true` に設定した場合、ストリーミング接続用に認証を指定する必要があります。 `authenticationRequired` が `false` に設定されている場合、認証は必要ありません。 |

**応答**

リクエストが成功した場合は、HTTP ステータス 201 が、一意の ID （`id`）など、新しく作成された接続の詳細と共に返されます。

```json
{
  "id": "a59d368a-1152-4673-a46e-bd52e8cdb9a9",
  "etag": "\"f50185ed-0000-0200-0000-637e8fad0000\""
}
```

## ストリーミングエンドポイント URL を取得

ベース接続を作成した状態で、ストリーミングエンドポイント URL を取得できるようになりました。

**API 形式**

```http
GET /flowservice/connections/{BASE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 以前に作成した接続の `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/flowservice/connections/{BASE_CONNECTION_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、リクエストした接続についての詳細情報と HTTP ステータス 200 が返されます。ストリーミングエンドポイント URL は、接続で自動的に作成され、`inletUrl` の値を使用して取得できます。

```json
{
  "items": [
    {
      "id": "a59d368a-1152-4673-a46e-bd52e8cdb9a9",
      "createdAt": 1669238699119,
      "updatedAt": 1669238699119,
      "createdBy": "acme@AdobeID",
      "updatedBy": "acme@AdobeID",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{UPDATED_CLIENT}",
      "sandboxId": "{SANDBOX_ID}",
      "sandboxName": "{SANDBOX_NAME}",
      "imsOrgId": "{ORG_ID}",
      "name": "ACME Streaming Connection XDM Data",
      "description": "ACME streaming connection for customer data",
      "connectionSpec": {
        "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
        "version": "1.0"
      },
      "state": "enabled",
      "auth": {
        "specName": "Streaming Connection",
        "params": {
          "sourceId": "ACME Streaming Connection XDM Data",
          "inletUrl": "https://dcs.adobedc.net/collection/667b41cf2dbf3509927da1ebf7e93c20afa727cc8d8373e51da18b62e1b985ec",
          "authenticationRequired": false,
          "inletId": "667b41cf2dbf3509927da1ebf7e93c20afa727cc8d8373e51da18b62e1b985ec",
          "dataType": "xdm",
          "name": "ACME Streaming Connection XDM Data"
        }
      },
      "version": "\"f50185ed-0000-0200-0000-637e8fad0000\"",
      "etag": "\"f50185ed-0000-0200-0000-637e8fad0000\""
    }
  ]
}
```

## ソース接続の作成 {#source}

ソース接続を作成するには、ベースPOSTID を指定したうえで、`/sourceConnections` エンドポイントに接続リクエストを行います。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
      "name": "ACME Streaming Source Connection for Customer Data",
      "description": "A streaming source connection for ACME XDM Customer Data",
      "baseConnectionId": "a59d368a-1152-4673-a46e-bd52e8cdb9a9",
      "connectionSpec": {
          "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
          "version": "1.0"
      }
    }'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 201 と、一意の ID （`id`）を含む新しく作成されたソース接続の詳細が返されます。

```json
{
  "id": "34ece231-294d-416c-ad2a-5a5dfb2bc69f",
  "etag": "\"d505125b-0000-0200-0000-637eb7790000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[Schema Registry API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) に POST リクエストを実行することで、ターゲット XDM スキーマを作成できます。

ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../../../xdm/api/schemas.md)に関するチュートリアルを参照してください。

### ターゲットデータセットの作成 {#target-dataset}

[Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) に POST リクエストを実行し、その際にペイロード内でターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。

## ターゲット接続の作成 {#target}

ターゲット接続は、取り込まれたデータが取り込まれる宛先への接続を表します。 ターゲット接続を作成するには、`/targetConnections` に対してPOSTリクエストを行い、ターゲットデータセットおよびターゲット XDM スキーマの ID を指定します。 この手順では、データレイク接続仕様 ID も指定する必要があります。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
      "name": "ACME Streaming Target Connection",
      "description": "ACME Streaming Target Connection",
      "data": {
          "schema": {
              "id": "https://ns.adobe.com/{TENANT}/schemas/7f682c29f887512a897791e7161b90a1ae7ed3dd07a177b1",
              "version": "application/vnd.adobe.xed-full+json;version=1.0"
          }
      },
      "params": {
          "dataSetId": "637eb7fadc8a211b6312b65b"
      },
          "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      }
  }'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 201 が、一意の ID （`id`）など、新しく作成されたターゲット接続の詳細と共に返されます。

```json
{
  "id": "07f2f6ff-1da5-4704-916a-c615b873cba9",
  "etag": "\"340680f7-0000-0200-0000-637eb8730000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。

マッピングセットを作成するには、[[!DNL Data Prep]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-prep.yaml) の `mappingSets` エンドポイントに POST リクエストを実行し、その際にターゲット XDM スキーマ `$id` および作成するマッピングセットの詳細を指定します。

**API 形式**

```http
POST /mappingSets
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "version": 0,
      "xdmSchema": "https://ns.adobe.com/{TENANT}/schemas/7f682c29f887512a897791e7161b90a1ae7ed3dd07a177b1",
      "xdmVersion": "1.0",
      "mappings": [
          {
              "destinationXdmPath": "person.name.firstName",
              "sourceAttribute": "firstName",
              "identity": false,
              "version": 0
          },
          {
              "destinationXdmPath": "person.name.lastName",
              "sourceAttribute": "lastName",
              "identity": false,
              "version": 0
          }
      ]
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `xdmSchema` | ターゲット XDM スキーマの `$id`。 |

**応答**

応答が成功すると、一意の ID（`id`）など、新しく作成されたマッピングの詳細が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
  "id": "79a623960d3f4969835c9e00dc90c8df",
  "version": 0,
  "createdDate": 1669249214031,
  "modifiedDate": 1669249214031,
  "createdBy": "acme@AdobeID",
  "modifiedBy": "acme@AdobeID"
}
```

## データフローの作成

ソース接続とターゲット接続を作成したら、データフローを作成できます。 データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。 `/flows` エンドポイントにデータフロー要求を実行することで、POSTを作成できます。

**API 形式**

```http
POST /flows
```

**リクエスト**

>[!BEGINTABS]

>[!TAB  変換なし ]

次のリクエストでは、データ変換を行わずに、HTTP API のストリーミングデータフローを作成しています。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
      "name": "ACME Streaming Dataflow",
      "description": "ACME streaming dataflow for customer data",
      "flowSpec": {
        "id": "d8a6f005-7eaf-4153-983e-e8574508b877",
        "version": "1.0"
      },
      "sourceConnectionIds": [
        "34ece231-294d-416c-ad2a-5a5dfb2bc69f"
      ],
      "targetConnectionIds": [
        "07f2f6ff-1da5-4704-916a-c615b873cba9"
      ]
    }'
```

>[!TAB  変換を使用 ]

次のリクエストは、データに適用されたマッピング変換を使用して、HTTP API 用のストリーミングデータフローを作成します。

変換を使用してデータフローを作成する場合、`name` パラメーターは変更できません。 この値は、常に `Mapping` に設定する必要があります。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
      "name": "<name>",
      "description": "<description>",
      "flowSpec": {
        "id": "c1a19761-d2c7-4702-b9fa-fe91f0613e81",
        "version": "1.0"
      },
      "sourceConnectionIds": [
        "34ece231-294d-416c-ad2a-5a5dfb2bc69f"
      ],
      "targetConnectionIds": [
        "07f2f6ff-1da5-4704-916a-c615b873cba9"
      ],
      "transformations": [
        {
          "name": "Mapping",
          "params": {
            "mappingId": "79a623960d3f4969835c9e00dc90c8df",
            "mappingVersion": 0
          }
        }
      ]
    }'
```

>[!ENDTABS]

| プロパティ | 説明 |
| --- | --- |
| `name` | データフローの名前。データフローの情報を検索する際に使用できるので、データフローはわかりやすい名前にしてください。 |
| `description` | （オプション）データフローの詳細を提供するために含めることができるプロパティ。 |
| `flowSpec.id` | [!DNL HTTP API] のフロー仕様 ID。 変換を含むデータフローを作成するには、`c1a19761-d2c7-4702-b9fa-fe91f0613e81` を使用する必要があります。 変換を行わずにデータフローを作成するには、`d8a6f005-7eaf-4153-983e-e8574508b877` を使用します。 |
| `sourceConnectionIds` | 前の手順で取得した[ソース接続 ID](#source)。 |
| `targetConnectionIds` | 前の手順で取得した[ターゲット接続 ID](#target)。 |
| `transformations.params.mappingId` | 前の手順で取得した[マッピング ID](#mapping)。 |

**応答**

リクエストが成功した場合は、HTTP ステータス 201 が、一意の ID （`id`）を含む、新しく作成されたデータフローの詳細と共に返されます。

```json
{
  "id": "f2ae0194-8bd8-4a40-a4d9-f07bdc3e6ce2",
  "etag": "\"dc0459ae-0000-0200-0000-637ebaec0000\""
}
```

## Platform に取り込まれるPost データ {#ingest-data}

>[!NOTE]
>
>データフローの作成からストリーミングデータの取り込みまでの間には、少なくとも 5 分の遅延を追加する必要があります。 これにより、データを取り込む前に、データフローを完全に有効にできます。

フローを作成したので、以前に作成したストリーミングエンドポイントに JSON メッセージを送信できます。

**API 形式**

```http
POST /collection/{INLET_URL}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INLET_URL}` | ストリーミングエンドポイント URL。 この URL を取得するには、ベース接続 ID を指定する際に、`/connections` エンドポイントに対してGETリクエストを行います。 |
| `{FLOW_ID}` | HTTP API ストリーミングデータフローの ID。 この ID は、XDM と RAW の両方のデータに必要です。 |

**リクエスト**

>[!BEGINTABS]

>[!TAB XDM データの送信 ]

```shell
curl -X POST https://dcs.adobedc.net/collection/667b41cf2dbf3509927da1ebf7e93c20afa727cc8d8373e51da18b62e1b985ec \
  -H 'Content-Type: application/json' \
  -d '{
        "header": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT}/schemas/7f682c29f887512a897791e7161b90a1ae7ed3dd07a177b1",
            "contentType": "application/vnd.adobe.xed-full-notext+json; version=1.0"
          },
          "flowId": "f2ae0194-8bd8-4a40-a4d9-f07bdc3e6ce2",
          "datasetId": "604a18a3bae67d18db6d258c"
        },
        "body": {
          "xdmMeta": {
            "schemaRef": {
              "id": "https://ns.adobe.com/{TENANT}/schemas/7f682c29f887512a897791e7161b90a1ae7ed3dd07a177b1",
              "contentType": "application/vnd.adobe.xed-full-notext+json; version=1.0"
            }
          },
          "xdmEntity": {
            "_id": "http-source-connector-acme-01",
            "person": {
              "name": {
                "firstName": "suman",
                "lastName": "nolan"
              }
            },
            "workEmail": {
              "primary": true,
              "address": "suman@acme.com",
              "type": "work",
              "status": "active"
            }
          }
        }
      }'
```

>[!TAB  フロー ID を持つ生データを HTTP ヘッダーとして送信 ]

生データを送信する際に、フロー ID をクエリパラメーターまたは HTTP ヘッダーの一部として指定できます。 次の例では、フロー ID を HTTP ヘッダーとして指定しています。

```shell
curl -X POST https://dcs.adobedc.net/collection/667b41cf2dbf3509927da1ebf7e93c20afa727cc8d8373e51da18b62e1b985ec \
  -H 'Content-Type: application/json' 
  -H 'x-adobe-flow-id=f2ae0194-8bd8-4a40-a4d9-f07bdc3e6ce2' \
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

>[!TAB  フロー ID を持つ生データをクエリパラメーターとして送信 ]

生データを送信する場合、フロー ID をクエリパラメーターまたは HTTP ヘッダーとして指定できます。 次の例では、フロー ID をクエリパラメーターとして指定しています。

```shell
curl -X POST https://dcs.adobedc.net/collection/667b41cf2dbf3509927da1ebf7e93c20afa727cc8d8373e51da18b62e1b985ec?x-adobe-flow-id=f2ae0194-8bd8-4a40-a4d9-f07bdc3e6ce2 \
  -H 'Content-Type: application/json' \
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

>[!ENDTABS]

**応答**

応答が成功すると、HTTP ステータス 200 が、新しく取り込んだ情報の詳細と共に返されます。

```json
{
    "inletId": "{BASE_CONNECTION_ID}",
    "xactionId": "1584479347507:2153:240",
    "receivedTimeMs": 1584479347507
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BASE_CONNECTION_ID}` | 以前に作成したストリーミング接続の ID。 |
| `xactionId` | 送信したレコードに対してサーバー側で生成された一意の ID です。この IDは、様々なシステムやデバッグを通じて、アドビがこのレコードのライフサイクルを追跡するのに役立ちます。 |
| `receivedTimeMs` | リクエストが受信された時刻を示すタイムスタンプ（ミリ秒単位のエポックタイム）。 |


## 次の手順

このチュートリアルでは、ストリーミング HTTP 接続を作成し、ストリーミングエンドポイントを使用してデータを Platform に取り込めるようにしました。 UI でストリーミング接続を作成する手順については、[ ストリーミング接続の作成チュートリアル ](../../../ui/create/streaming/http.md) を参照してください。

データを Platform にストリーミングする方法については、[ 時系列データのストリーミング ](../../../../../ingestion/tutorials/streaming-time-series-data.md) または [ レコードデータのストリーミング ](../../../../../ingestion/tutorials/streaming-record-data.md) に関するチュートリアルを参照してください。

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

