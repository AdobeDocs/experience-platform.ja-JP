---
title: Flow Service API を使用したデータベースの参照
description: このチュートリアルでは、Flow Service API を使用して、サードパーティのデータベースの内容とファイル構造を調べます。
exl-id: 94935492-a7be-48dc-8089-18476590bf98
source-git-commit: 46e1a62e558a209ffed4a693cfd71ad5e76d7d98
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 9%

---

# [!DNL Flow Service] API を使用したデータベースの参照

このチュートリアルでは、[!DNL Flow Service] API を使用して、サードパーティのデータベースの内容とファイル構造を調べます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [&#x200B; ソース &#x200B;](../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してサードパーティデータベースに正常に接続するために必要な追加情報を示しています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../landing/api-guide.md) を参照してください。

## データテーブルの探索

データベースの接続 ID を使用すると、GET リクエストを実行してデータテーブルを調べることができます。 次の呼び出しを使用して、検査するテーブルまたはExperience Platformに取り込むテーブルのパスを検索します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | データベースソースの接続 ID。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/6990abad-977d-41b9-a85d-17ea8cf1c0e4/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、データベースからテーブルの配列が返されます。 Experience Platformに取り込むテーブルを見つけ、その `path` プロパティをメモします。これは、次の手順で構造を調べるときに指定する必要があるからです。

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

データベースからテーブルの構造を調べるには、テーブルのパスをクエリパラメーターとして指定して、GET リクエストを実行します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | データベース接続の ID。 |
| `{TABLE_PATH}` | テーブルのパス。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/6990abad-977d-41b9-a85d-17ea8cf1c0e4/explore?objectType=table&object=test1.Mytable' \
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
    },
    "data": [],
    "cdcMetadata": {
      "columnDetected": true
    }
}
```

## 次の手順

このチュートリアルでは、データベースを探索し、Experience Platformに取り込むテーブルのパスを見つけ、その構造に関する情報を取得しました。 次のチュートリアルでこの情報を使用して [&#x200B; データベースからデータを収集し、Experience Platformに取り込む &#x200B;](../collect/database-nosql.md) ことができます。
