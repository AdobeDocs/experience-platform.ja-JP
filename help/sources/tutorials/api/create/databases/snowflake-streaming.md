---
title: Snowflake ストリーミングアカウントのAdobe Experience Platformへの接続
description: Flow Service API を使用してAdobe Experience PlatformをSnowflake Streaming に接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 3fc225a4-746c-4a91-aa77-bbeb091ec364
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 21%

---

# [!DNL Flow Service] API[!DNL Snowflake] 使用したExperience Platformへのデータのストリーミング

>[!IMPORTANT]
>
>
> Real-Time Customer Data Platform Ultimateを購入したユーザーは、API で [!DNL Snowflake] ストリーミングソースを利用できます。

このチュートリアルでは、[[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>) を使用して、[!DNL Snowflake] アカウントからAdobe Experience Platformにデータを接続してストリーミングする手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

前提条件として、[!DNL Snowflake] ストリーミングソースに関する設定と情報を行います。 [[!DNL Snowflake]  ストリーミングソースの概要 ](../../../../connectors/databases/snowflake-streaming.md) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成 {#create-a-base-connection}

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Snowflake] 認証資格情報をリクエスト本文の一部として指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Snowflake] のベース接続を作成します。

>[!TIP]
>
>`auth.specName` の値は、空白を含め、以下の例のように正確に入力する必要があります。

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
| `auth.params.account` | [!DNL Snowflake] ストリーミングアカウントの名前。 |
| `auth.params.database` | データの取得元となる [!DNL Snowflake] データベースの名前。 |
| `auth.params.warehouse` | [!DNL Snowflake] ウェアハウスの名前。 [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |
| `auth.params.username` | [!DNL Snowflake] ストリーミングアカウントのユーザー名。 |
| `auth.params.schema` | （オプション） [!DNL Snowflake] ストリーミングアカウントに関連付けられたデータベーススキーマ。 |
| `auth.params.password` | [!DNL Snowflake] ストリーミングアカウントのパスワード。 |
| `auth.params.role` | （任意）この [!DNL Snowflake] 接続のユーザーの役割。 指定しない場合、この値はデフォルトで `public` になります。 |
| `connectionSpec.id` | [!DNL Snowflake] 接続仕様 ID: `51ae16c2-bdad-42fd-9fce-8d5dfddaf140`。 |

**応答**

リクエストが成功した場合は、新しく作成したベース接続と、対応する etag が返されます。

```json
{
    "id": "1b614dc0-b76e-41e1-b25f-09f4a9d3f111",
    "etag": "\"d300cf4e-0000-0200-0000-6447a7750000\""
}
```

## データテーブルの探索 {#explore-your-data-tables}

次に、ベース接続 ID をパラメーターとして指定しながら、`/connections/{BASE_CONNECTION_ID}/explore?objectType=root` エンドポイントに対してGET リクエストを実行することで、ソースのデータテーブルを調べ、移動できます。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | [!DNL Snowflake] ストリーミングソースのベース接続 ID。 |


**リクエスト**

次のリクエストでは、[!DNL Snowflake] ストリーミングアカウントの構造と内容を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/1b614dc0-b76e-41e1-b25f-09f4a9d3f111/explore?objectType=root' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、ルートレベルでソースのデータの構造と内容が返されます。

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
| `items.names` | テーブルの名前。 |

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
| `baseConnectionId` | [!DNL Snowflake] ストリーミングソースの認証済みベース接続 ID。 この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | [!DNL Snowflake] ストリーミングソースの接続仕様 ID。 |
| `params.tableName` | Experience Platformに取り込む、[!DNL Snowflake] データベース内のテーブルの名前。 |
| `params.timestampColumn` | 増分値を取得するために使用されるタイムスタンプ列の名前。 |
| `params.backfill` | データが最初（0 エポック時間）から取得されるか、ソースが開始された時間から取得されるかを決定するブール値フラグ。 この値について詳しくは、[[!DNL Snowflake]  ストリーミングソースの概要 ](../../../../connectors/databases/snowflake-streaming.md) を参照してください。 |
| `params.timezoneValue` | タイムゾーンの値は、[!DNL Snowflake] データベースに対するクエリ時に、どのタイムゾーンの現在の時刻を取得するかを示します。 設定のタイムスタンプ列が `TIMESTAMP_NTZ` に設定されている場合は、このパラメーターを指定する必要があります。 指定しない場合、デフォルト `timezoneValue`UTC になります。 |

**応答**

正常な応答は、ソース接続 ID と対応する etag を返します。 このソース接続 ID は、後の手順でデータフローを作成する際に使用します。

```json
{
    "id": "61c0c5f1-bfe5-40f7-8f8c-a4dc175ddac6",
    "etag": "\"d300cf4e-0000-0200-0000-6447a7750000\""
}
```

## データフローの作成

ツアー [!DNL Snowflake] ーザーアカウントからExperience Platformにデータをストリーミングするデータフローを作成するには、次の値を指定したうえで、`/flows` エンドポイントに対して POST リクエストを行う必要があります。

>[!TIP]
>
>以下のリンクに従って、次の ID を取得する方法に関するステップバイステップガイドを確認してください。

* [ソース接続 ID](#create-a-source-connection)
* [ターゲット接続 ID](../../collect/database-nosql.md#create-a-target-connection)
* [フロー仕様 ID](../../collect/database-nosql.md#retrieve-dataflow-specifications)
* [マッピング ID](../../collect/database-nosql.md#create-a-mapping)

**API 形式**

```http
POST /flows
```

**リクエスト**

次のリクエストは、[!DNL Snowflake] アカウントのストリーミングデータフローを作成します。

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
| `sourceConnectionIds` | [!DNL Snowflake] ストリーミングソースのソース接続 ID。 |
| `targetConnectionIds` | [!DNL Snowflake] ストリーミングソースのターゲット接続 ID。 |
| `flowSpec.id` | [!DNL Snowflake] ストリーミングソースのデータフローを作成するためのフロー仕様 ID。 このフロー仕様 ID を使用すると、マッピング変換を使用したストリーミングデータフローを作成できます。 この ID は固定で、`c1a19761-d2c7-4702-b9fa-fe91f0613e81` です。 |
| `transformations.params.mappingId` | データフローのマッピング ID。 |

**応答**

正常な応答は、フロー ID と対応する etag を返します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"770029f8-0000-0200-0000-6019e7d40000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Snowflake] データのストリーミングデータフローを作成しました。 Adobe Experience Platform ソースについて詳しくは、次のドキュメントを参照してください。

* [ソースの概要](../../../../home.md)
* [API を使用したデータフローの監視](../../monitor.md)
