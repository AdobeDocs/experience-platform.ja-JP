---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用して顧客の成功システムを調査する
topic: overview
translation-type: tm+mt
source-git-commit: ea2e67ae30bae1f2c0a76f0e5aa97aaf378bbdec

---


# Flow Service APIを使用して顧客の成功システムを調査する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、Flow Service APIを使用して、顧客の成功(CS)システムを調べます。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してCSシステムに正常に接続するために知っておく必要のある追加情報を示します。

### ベース接続の取得

プラットフォームAPIを使用してCSシステムを調べるには、有効なベース接続IDが必要です。 作業対象のCSシステムのベース接続がまだない場合は、次のチュートリアルで接続を作成できます。

* [Salesforceサービスクラウド](../create/customer-success/salesforce-service-cloud.md)
* [ServiceNow](../create/customer-success/servicenow.md)

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

CSシステムのベース接続を使用して、GETリクエストを実行することで、データテーブルを調べることができます。 次の呼び出しを使用して、プラットフォームに対して検査または取り込むテーブルのパスを探します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | CSベース接続のID。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/60a5c8b9-3c30-43ba-a5c8-b93c3093ba66/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した場合は、CSシステムからテーブルの配列が返されます。 Platformに取り込む表を探し、そのプロパティをメモしておきます。これは、次の手順で `path` その構造を調査するために指定する必要があるためです。

```json
[
    {
        "type": "table",
        "name": "Accepted Event Relation",
        "path": "AcceptedEventRelation",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Account",
        "path": "Account",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Account Change Event",
        "path": "AccountChangeEvent",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Account Clean Info",
        "path": "AccountCleanInfo",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## テーブルの構造を検査する

CSシステムからテーブルの構造を調べるには、テーブルのパスをパラメータとして指定しながらGETリクエストをクエリします。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | CSベース接続のID。 |
| `{TABLE_PATH}` | テーブルのパス。 |

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/60a5c8b9-3c30-43ba-a5c8-b93c3093ba66/explore?objectType=table&object=test1.Mytable' \
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
                "name": "Id",
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
            },
            {
                "name": "Phone",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
        ]
    }
}
```

## 次の手順

このチュートリアルに従って、CSシステムを調べ、プラットフォームに取り込むテーブルのパスを見つけ、その構造に関する情報を取得しました。 次のチュートリアルでこの情報を使用して、CSシステム [からデータを収集し、Platformに取り込むことができます](../collect/customer-success.md)。