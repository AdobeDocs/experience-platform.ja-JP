---
keywords: Experience Platform；ホーム；人気のトピック；protocol
solution: Experience Platform
title: Flow Service API を使用したプロトコルシステムの探索
description: このチュートリアルでは、Flow Service API を使用して、プロトコルアプリケーションを調べます。
exl-id: e4b24312-543e-4014-aa53-e8ca9c620950
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 39%

---

# [!DNL Flow Service] API を使用したプロトコルシステムの調査

[!DNL Flow Service] を使用すると、様々な異なるソースから顧客データを収集し、Adobe Experience Platformで一元化できます。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされているすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] API を使用してプロトコルアプリケーションを調べます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してプロトコルアプリケーションに正常に接続するために必要な追加情報を示しています。

### ベース接続の取得

[!DNL Experience Platform] API を使用してプロトコルシステムを探索するには、有効なベース接続 ID を持っている必要があります。 使用するプロトコルシステムのベース接続がまだない場合は、次のチュートリアルを通じて作成できます。

* [汎用 OData](../create/protocols/odata.md)

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name：`{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## データテーブルの探索

プロトコルアプリケーションの接続 ID を使用すると、GET リクエストを実行してデータテーブルを調べることができます。 次の呼び出しを使用して、検査または [!DNL Experience Platform] に取り込むテーブルのパスを検索します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | プロトコルベース接続の ID。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/a5c6b647-e784-4b58-86b6-47e784ab580b/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、プロトコルアプリケーションからテーブルの配列が返されます。 [!DNL Experience Platform] に取り込むテーブルを見つけ、その `path` プロパティをメモします。これは、次の手順でその構造を検査するためにテーブルを指定する必要があるからです。

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

プロトコルアプリケーションからテーブルの構造を調べるには、テーブルのパスをクエリパラメーターとして指定してGET リクエストを実行します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | プロトコルアプリケーションの接続 ID。 |
| `{TABLE_PATH}` | プロトコルアプリケーション内のテーブルのパス。 |

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/a5c6b647-e784-4b58-86b6-47e784ab580b/explore?objectType=table&object=Orders' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、指定されたテーブルの構造が返されます。 テーブルの各列に関する詳細は、`columns` 配列の要素内にあります。

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

このチュートリアルに従って、プロトコルアプリケーションを調べ、[!DNL Experience Platform] に取り込むテーブルのパスを見つけ、その構造に関する情報を取得しました。 次のチュートリアルでこの情報を使用して [&#x200B; プロトコルアプリケーションからデータを収集し、Experience Platformに取り込む &#x200B;](../collect/protocols.md) ことができます。
