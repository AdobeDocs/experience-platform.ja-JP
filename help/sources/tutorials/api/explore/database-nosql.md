---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用して、データベースまたはNoSQLシステムを調査します。
topic: overview
translation-type: tm+mt
source-git-commit: 4c34ecdaeb4a0df1faf2dd54e8a264b9126f20b4

---


# Flow Service APIを使用して、データベースまたはNoSQLシステムを調査します。

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、Flow Service APIを使用して、データベースまたはNoSQLシステムを調査します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してデータベースまたはNoSQLシステムに正常に接続するために知っておく必要のある追加情報を示します。

### ベース接続の取得

プラットフォームAPIを使用してデータベースまたはNoSQLシステムを調べるには、有効なベース接続IDが必要です。 操作するデータベースまたはNoSQLシステムのベース接続がまだない場合は、次のチュートリアルを使用して接続を作成できます。

* [Amazon Redshift](../create/databases/redshift.md)
* [Azure HDInsightsのApache Spark ](../create/databases/spark.md)
* [Azure Synapse Analytics](../create/databases/synapse-analytics.md)
* [Azureテーブルストレージ](../create/databases/ats.md)
* [Google BigQuery](../create/databases/bigquery.md)
* [Hive](../create/databases/hive.md)
* [MariaDB](../create/databases/mariadb.md)
* [MySQL](../create/databases/mysql.md)
* [フェニックス](../create/databases/phoenix.md)
* [PostgreSQL](../create/databases/postgres.md)
* [SQL Server](../create/databases/sql-server.md)

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## データテーブルの調査

データベースまたはNoSQLシステムのベース接続を使用して、GET要求を実行することで、データテーブルを調べることができます。 次の呼び出しを使用して、プラットフォームに対して検査または取り込むテーブルのパスを探します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | データベースまたはNoSQLベース接続のID。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/54c22133-3a01-4d3b-8221-333a01bd3b03/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、データベースまたはNoSQLシステムからテーブルの配列を返します。 Platformに取り込む表を探し、そのプロパティをメモしておきます。これは、次の手順で `path` その構造を調査するために指定する必要があるためです。

```json
[
    {
        "type": "table",
        "name": "test1.Mytable",
        "path": "test1.Mytable",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "test1.austin_demo",
        "path": "test1.austin_demo",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## テーブルの構造を検査する

データベースまたはNoSQLシステムからテーブルの構造を検査するには、テーブルのパスをクエリパラメータとして指定しながらGETリクエストを実行します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | データベースまたはNoSQLベース接続のID。 |
| `{TABLE_PATH}` | テーブルのパス。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/54c22133-3a01-4d3b-8221-333a01bd3b03/explore?objectType=table&object=test1.Mytable' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したテーブルの構造を返します。 各テーブルの列に関する詳細は、配列の要素内に配置され `columns` ます。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "TestID",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Name",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            }
        ]
    }
}
```

## 次の手順

このチュートリアルに従って、データベースまたはNoSQLシステムを調べ、プラットフォームに取り込むテーブルのパスを見つけ、その構造に関する情報を得ました。 次のチュートリアルでこの情報を使用して、データベ [ースまたはNoSQLシステムからデータを収集し、Platformに取り込むことができます](../collect/database-nosql.md)。