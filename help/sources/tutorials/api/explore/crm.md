---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用したCRMシステムの調査
topic: overview
translation-type: tm+mt
source-git-commit: 3d8682eb1a33b7678ed814e5d6d2cb54d233c03e
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 2%

---


# Flow Service APIを使用したCRMシステムの調査

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用してCRMシステムを調べます。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してCRMシステムに正常に接続するために知っておく必要がある追加情報について説明します。

### ベース接続の取得

プラットフォームAPIを使用してCRMシステムを調査するには、有効なベース接続IDが必要です。 操作するCRMシステムの基本接続がまだない場合は、次のチュートリアルを使用して作成できます。

* [Microsoft Dynamics](../create/crm/ms-dynamics.md)
* [Salesforce](../create/crm/salesforce.md)

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platformのすべてのリソース（フローサービスに属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## データテーブルの調査

CRMシステムの基本接続を使用して、GET要求を実行することで、データテーブルを調べることができます。 次の呼び出しを使用して、プラットフォームに対して検査または取り込むテーブルのパスを探します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | CRMシステムのベース接続のID。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{BASE_CONNECTION_ID}/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答とは、CRMシステムに対する一連のテーブルです。 プラットフォームに取り込む表を探し、次の手順でプラットフォームの構造を調べる必要があるので、その `path` プロパティをメモしておきます。

```json
[
    {
        "type": "table",
        "name": "Solution Component Summary",
        "path": "msdyn_solutioncomponentsummary",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Quote Invoicing Product",
        "path": "msdyn_quoteinvoicingproduct",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Opportunity Relationship",
        "path": "customeropportunityrole",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## テーブルの構造を検査する

CRMシステムからテーブルの構造を検査するには、テーブルのパスをクエリパラメーターとして指定しながらGETリクエストを実行します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | CRMシステムのベース接続のID。 |
| `{TABLE_PATH}` | テーブルのパス。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、テーブルの構造を返します。 各テーブルの列に関する詳細は、 `columns` 配列の要素内にあります。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "first_name",
                "type": "string",
                "meta": {
                    "originalType": "String"
                }
            },
            {
                "name": "last_name",
                "type": "string",
                "meta": {
                    "originalType": "String"
                }
            },
            {
                "name": "email",
                "type": "string",
                "meta": {
                    "originalType": "String"
                }
            }
        ]
    }
}
```

## 次の手順

このチュートリアルに従って、CRMシステムを調べ、プラットフォームに取り込む表のパスを見つけ、構造に関する情報を取得しました。 次のチュートリアルでこの情報を使用して、CRMシステムからデータを [収集し、プラットフォームに取り込むことができます](../collect/crm.md)。