---
title: Flow Service API を使用したGoogle BigQuery のExperience Platformへの接続
description: Flow Service API を使用してAdobe Experience PlatformをGoogle BigQuery に接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 51f90366-7a0e-49f1-bd57-b540fa1d15af
source-git-commit: 1900a8c6a3f3119c8b9049b12f5660cc9fd181a2
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 39%

---

# [!DNL Flow Service] API を使用した [!DNL Google BigQuery] のExperience Platformへの接続

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Google BigQuery] ソースを利用できます。

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL Google BigQuery] データベースをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

### 必要な資格情報の収集

[!DNL Google BigQuery] 資格情報の取得手順について詳しくは、[[!DNL Google BigQuery]  認証ガイド ](../../../../connectors/databases/bigquery.md#prerequisites) を参照してください。

## [!DNL Google BigQuery] を Azure 上のExperience Platformに接続 {#azure}

[!DNL Google BigQuery] ソースを Azure 上のExperience Platformに接続する方法については、以下の手順を参照してください。

### Azure 上のExperience Platformに [!DNL Google BigQuery] のベース接続を作成する {#azure-base}

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Google BigQuery] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

>[!BEGINTABS]

>[!TAB  基本認証の使用 ]

+++リクエスト

次のリクエストは、基本認証を使用して [!DNL Google BigQuery] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google BigQuery connection with basic authentication",
        "description": "Google BigQuery connection with basic authentication",
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
| `auth.params.project` | 照会する既定の [!DNL Google BigQuery] プロジェクトのプロジェクト ID。 に対して。 |
| `auth.params.clientId` | 更新トークンの生成に使用される ID 値。 |
| `auth.params.clientSecret` | 更新トークンの生成に使用されるクライアント値。 |
| `auth.params.refreshToken` | [!DNL Google] から取得された更新トークンは、[!DNL Google BigQuery] へのアクセスを許可するために使用されます。 |
| `connectionSpec.id` | [!DNL Google BigQuery] 接続仕様 ID: `3c9b37f8-13a6-43d8-bad3-b863b941fedd`。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6990abad-977d-41b9-a85d-17ea8cf1c0e4",
    "etag": "\"ca00acbf-0000-0200-0000-60149e1e0000\""
}
```

+++

>[!TAB  サービス認証の使用 ]


+++リクエスト

次のリクエストは、サービス認証を使用して [!DNL Google BigQuery] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Google BigQuery base connection with service account",
      "description": "Google BigQuery connection with service account",
      "auth": {
          "specName": "Service Authentication",
          "params": {
                  "projectId": "{PROJECT_ID}",
                  "keyFileContent": "{KEY_FILE_CONTENT},
                  "largeResultsDataSetId": "{LARGE_RESULTS_DATASET_ID}"
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
| `auth.params.projectId` | 照会する既定の [!DNL Google BigQuery] プロジェクトのプロジェクト ID。 に対して。 |
| `auth.params.keyFileContent` | サービスアカウントの認証に使用されるキーファイル。 キーファイルのコンテンツは [!DNL Base64] でエンコードする必要があります。 |
| `auth.params.largeResultsDataSetId` | （オプション）大きな結果セットのサポートを有効にするために必要な、事前に作成された [!DNL Google BigQuery] データセット ID。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6990abad-977d-41b9-a85d-17ea8cf1c0e4",
    "etag": "\"ca00acbf-0000-0200-0000-60149e1e0000\""
}
```

+++

>[!ENDTABS]

## Experience Platform on Amazon Web Services（AWS）への [!DNL Google BigQuery] の接続 {#aws}

[!DNL Google BigQuery] データベースをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### AWS上のExperience Platformに [!DNL Google BigQuery] のベース接続を作成する

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、AWS上のExperience Platformに接続するためのベース接続 [!DNL Google BigQuery] 作成します。

+++選択すると例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Google BigQuery base connection on AWS",
      "description": "Google BigQuery base connection on AWS",
      "auth": {
          "specName": "Service Authentication",
          "params": {
                  "projectId": "{PROJECT_ID}",
                  "keyFileContent": "{KEY_FILE_CONTENT},
                  "datasetId": "{DATASET_ID}"
      },
      "connectionSpec": {
          "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.projectId` | 照会する既定の [!DNL Google BigQuery] プロジェクトのプロジェクト ID。 に対して。 |
| `auth.params.keyFileContent` | サービスアカウントの認証に使用されるキーファイル。 キーファイルのコンテンツは [!DNL Base64] でエンコードする必要があります。 |
| `auth.params.datasetId` | [!DNL Google BigQuery] ソースに対応するデータセット ID。 この ID は、データテーブルの場所を表します。 |

+++

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでストレージを調査するために必要になります。

+++選択すると例が表示されます

```json
{
    "id": "6990abad-977d-41b9-a85d-17ea8cf1c0e4",
    "etag": "\"ca00acbf-0000-0200-0000-60149e1e0000\""
}
```

+++

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Google BigQuery] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
