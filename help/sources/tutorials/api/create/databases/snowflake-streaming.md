---
title: SnowflakeストリーミングアカウントをAdobe Experience Platformに接続
description: フローサービス API を使用してAdobe Experience PlatformをSnowflakeストリーミングに接続する方法について説明します。
badgeBeta: label="ベータ版" type="Informative"
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 3fc225a4-746c-4a91-aa77-bbeb091ec364
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 29%

---

# ストリーム [!DNL Snowflake] Experience Platformに [!DNL Flow Service] API

>[!IMPORTANT]
>
>* The [!DNL Snowflake] ストリーミングソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細
>* The [!DNL Snowflake] ストリーミングソースは、Real-time Customer Data Platform Ultimate を購入したユーザーが API で使用できます。

このチュートリアルでは、 [!DNL Snowflake] を使用してAdobe Experience Platformにアカウント [[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

前提条件の設定および [!DNL Snowflake] ストリーミングソース。 詳しくは、 [[!DNL Snowflake] ストリーミングソースの概要](../../../../connectors/databases/snowflake-streaming.md).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成 {#create-a-base-connection}

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Snowflake] 認証資格情報をリクエスト本文の一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Snowflake] のベース接続を作成します。

>[!TIP]
>
>The `auth.specName` の値は、空白を含め、以下の例のとおりに入力する必要があります。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Snowflake base connection",
      "description": "Snowflake base connection",
      "auth": {
          "specName": "Basic Authentication for Snowflake",
          "params": {
              "account": "wixnnnd-ui60793.snowflakecomputing.com",
              "database": "ACME_DB",
              "warehouse": "ACME_WH",
              "username": "nikola15",
              "schema": "PUBLIC",
              "password": "xxxx",
              "role": "ACCOUNTADMIN"
          }
      },
      "connectionSpec": {
          "id": "51ae16c2-bdad-42fd-9fce-8d5dfddaf140",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.account` | お客様の [!DNL Snowflake] ストリーミングアカウント。 |
| `auth.params.database` | お客様の [!DNL Snowflake] データの取り込み元のデータベース。 |
| `auth.params.warehouse` | お客様の [!DNL Snowflake] 倉庫。 The [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各ウェアハウスは互いに独立しており、データを Platform に引き渡す際には、個別にアクセスする必要があります。 |
| `auth.params.username` | ユーザー名 [!DNL Snowflake] ストリーミングアカウント。 |
| `auth.params.schema` | （オプション） [!DNL Snowflake] ストリーミングアカウント。 |
| `auth.params.password` | ユーザーのパスワード [!DNL Snowflake] ストリーミングアカウント。 |
| `auth.params.role` | （オプション）このユーザーの役割 [!DNL Snowflake] 接続。 指定しない場合、この値はデフォルトでになります。 `public`. |
| `connectionSpec.id` | The [!DNL Snowflake] 接続仕様 ID: `51ae16c2-bdad-42fd-9fce-8d5dfddaf140`. |

**応答**

正常な応答は、新しく作成されたベース接続と、それに対応する etag を返します。

```json
{
    "id": "1b614dc0-b76e-41e1-b25f-09f4a9d3f111",
    "etag": "\"d300cf4e-0000-0200-0000-6447a7750000\""
}
```

## データテーブルの調査 {#explore-your-data-tables}

次に、ベース接続 ID を使用して、に対してGETリクエストを実行し、ソースのデータテーブルを調べて移動します。 `/connections/{BASE_CONNECTION_ID}/explore?objectType=root` エンドポイントを使用して、ベース接続 ID をパラメーターとして指定する必要があります。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | のベース接続 ID [!DNL Snowflake] ストリーミングソース。 |


**リクエスト**

次のリクエストでは、 [!DNL Snowflake] ストリーミングアカウント。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/1b614dc0-b76e-41e1-b25f-09f4a9d3f111/explore?objectType=root' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、ソースのデータの構造と内容をルートレベルで返します。

```json
{
    "items": [
        {
            "type": "table",
            "name": "ACME"
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `items.type` | テーブルのタイプ。 |
| `items.names` | テーブルの名前. |

## ソース接続の作成 {#create-a-source-connection}

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントに POST リクエストを実行します。

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
      "name": "Snowflake Streaming Source Connection",
      "description": "A source connection for Snowflake Streaming data",
      "baseConnectionId": "1b614dc0-b76e-41e1-b25f-09f4a9d3f111",
      "connectionSpec": {
          "id": "51ae16c2-bdad-42fd-9fce-8d5dfddaf140",
          "version": "1.0"
      },
      "params": {
          "tableName": "ACME",
          "timestampColumn": "dOb",
          "backfill": "true",
          "timezoneValue": "PST"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `baseConnectionId` | の認証済みベース接続 ID。 [!DNL Snowflake] ストリーミングソース。 この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | の接続仕様 ID [!DNL Snowflake] ストリーミングソース。 |
| `params.tableName` | テーブルの名前 [!DNL Snowflake] Platform に取り込むデータベースです。 |
| `params.timestampColumn` | 増分値の取得に使用される timestamp 列の名前です。 |
| `params.backfill` | データを最初（0 エポック時間）から取得するか、ソースが開始された時刻から取得するかを決定するブール型フラグ。 この値の詳細については、 [[!DNL Snowflake] ストリーミングソースの概要](../../../../connectors/databases/snowflake-streaming.md). |
| `params.timezoneValue` | timezone 値は、 [!DNL Snowflake] データベース。 設定のタイムスタンプ列が `TIMESTAMP_NTZ`. 指定されていない場合、 `timezoneValue` デフォルトは UTC です。 |

**応答**

正常な応答は、ソース接続 ID と、対応する ETag を返します。 ソース接続 ID は、後の手順でデータフローを作成する際に使用します。

```json
{
    "id": "61c0c5f1-bfe5-40f7-8f8c-a4dc175ddac6",
    "etag": "\"d300cf4e-0000-0200-0000-6447a7750000\""
}
```

## データフローの作成

ツアーのデータをストリーミングするデータフローを作成するには [!DNL Snowflake] アカウントを Platform に送信する場合は、 `/flows` エンドポイントで次の値を指定します。

>[!TIP]
>
>次の ID の取得方法に関する詳しい手順ガイドについては、以下のリンクを参照してください。

* [ソース接続 ID](#create-a-source-connection)
* [ターゲット接続 ID](../../collect/database-nosql.md#create-a-target-connection)
* [フロー仕様 ID](../../collect/database-nosql.md#retrieve-dataflow-specifications)
* [マッピング ID](../../collect/database-nosql.md#create-a-mapping)

**API 形式**

```http
POST /flows
```

**リクエスト**

次のリクエストでは、 [!DNL Snowflake] アカウント。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Snowflake Streaming Dataflow",
      "description": "A dataflow for Snowflake streaming data",
      "sourceConnectionIds": [
        "61c0c5f1-bfe5-40f7-8f8c-a4dc175ddac6"
      ],
      "targetConnectionIds": [
        "78f41c31-3652-4a5e-b264-74331226dcf3"
      ],
      "flowSpec": {
        "id": "c1a19761-d2c7-4702-b9fa-fe91f0613e81",
        "version": "1.0"
      },
      "transformations": [
        {
          "name": "Mapping",
          "params": {
            "mappingId": "44d42ed27c46499a80eb0c0705c38cbd",
            "mappingVersion": 0
          }
        }
      ]
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `sourceConnectionIds` | のソース接続 ID [!DNL Snowflake] ストリーミングソース。 |
| `targetConnectionIds` | のターゲット接続 ID [!DNL Snowflake] ストリーミングソース。 |
| `flowSpec.id` | のデータフローを作成するフロー仕様 ID。 [!DNL Snowflake] ストリーミングソース。 このフロー仕様 ID を使用すると、マッピング変換を使用してストリーミングデータフローを作成できます。 この ID は修正され、次の値になります。 `c1a19761-d2c7-4702-b9fa-fe91f0613e81`. |
| `transformations.params.mappingId` | データフローのマッピング ID。 |

**応答**

リクエストが成功した場合は、フロー ID とそれに対応する e タグが返されます。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"770029f8-0000-0200-0000-6019e7d40000\""
}
```

## 次の手順

このチュートリアルでは、 [!DNL Snowflake] データを [!DNL Flow Service] API. Adobe Experience Platform Sources について詳しくは、次のドキュメントを参照してください。

* [ソースの概要](../../../../home.md)
* [API を使用したデータフローの監視](../../monitor.md)
