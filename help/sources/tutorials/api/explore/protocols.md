---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用したプロトコルシステムの調査
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 1%

---


# APIを使用したプロトコル・システムの調査 [!DNL Flow Service]

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用してプロトコルアプリケーションを調べます。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

次の節では、 [!DNL Flow Service] APIを使用してプロトコルアプリケーションに正常に接続するために知っておく必要がある追加情報について説明します。

### ベース接続の取得

APIを使用してプロトコルシステムを調査するには、有効なベース接続IDが必要 [!DNL Platform] です。 使用するプロトコルシステムの基本接続がまだない場合は、次のチュートリアルを通じて接続を作成できます。

* [汎用OData](../create/protocols/odata.md)

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Flow Service]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## データテーブルの調査

プロトコルアプリケーションの接続IDを使用して、GET要求を実行することで、データテーブルを調べることができます。 次の呼び出しを使用して、検査または取り込むテーブルのパスを探し [!DNL Platform]ます。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | プロトコルベース接続のID。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/a5c6b647-e784-4b58-86b6-47e784ab580b/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答を返すと、プロトコルアプリケーションからテーブルの配列が返されます。 次の手順で、に取り込む表を探し [!DNL Platform] 、 `path` プロパティをメモしておきます。このプロパティは、次の手順で指定して構造を検査する必要があります。

```json
[
    {
        "type": "table",
        "name": "Categories",
        "path": "Categories",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "CustomerDemographics",
        "path": "CustomerDemographics",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Customers",
        "path": "Customers",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Orders",
        "path": "Orders",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## テーブルの構造を検査する

プロトコルアプリケーションからテーブルの構造を検査するには、テーブルのパスをクエリパラメーターとして指定しながらGETリクエストを実行します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | プロトコルアプリケーションの接続ID。 |
| `{TABLE_PATH}` | プロトコルアプリケーション内のテーブルのパスです。 |

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/a5c6b647-e784-4b58-86b6-47e784ab580b/explore?objectType=table&object=Orders' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたテーブルの構造を返します。 各テーブルの列に関する詳細は、 `columns` 配列の要素内にあります。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "OrderID",
                "type": "integer",
                "xdm": {
                    "type": "integer",
                    "minimum": -2147483648,
                    "maximum": 2147483647
                }
            },
            {
                "name": "CustomerID",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "EmployeeID",
                "type": "integer",
                "xdm": {
                    "type": "integer",
                    "minimum": -2147483648,
                    "maximum": 2147483647
                }
            },
            {
                "name": "OrderDate",
                "type": "string",
                "meta:xdmType": "date-time",
                "xdm": {
                    "type": "string",
                    "format": "date-time"
                }
            },
        ]
    }
}
```

## 次の手順

このチュートリアルに従って、プロトコルのアプリケーションを調べ、取り込むテーブルのパスを見つけ、その構造に関する情報を得 [!DNL Platform]ました。 次のチュートリアルでこの情報を使用して、プロトコルアプリケーションからデータを [収集し、Platformに取り込むことができます](../collect/protocols.md)。
