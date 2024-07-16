---
keywords: Experience Platform；ホーム；人気のトピック；広告システム；Advertising system
solution: Experience Platform
title: Flow Service API を使用したAdvertising システムの探索
description: フローサービスは、様々な異なるソースから顧客データを収集し、Adobe Experience Platformで一元化するために使用します。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされているすべてのソースを接続できます。 このチュートリアルでは、Flow Service API を使用して、広告システムを調べます。
exl-id: 3016ce1e-12e6-47ce-a4c5-52f8d440f515
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 19%

---

# [!DNL Flow Service] API を使用した広告システムの調査

ベース接続を作成したら、一意のベース接続 ID を使用して、ソースのデータ構造とコンテンツを移動および探索できるようになりました。 これにより、データフローを作成してAdobe Experience Platformに取り込む前に、特定の項目とそれぞれのデータタイプおよびフォーマットを識別できます。

このチュートリアルでは [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、広告システムを調べます。

## はじめに

>[!IMPORTANT]
>
>このチュートリアルでは、広告ソースの一意のベース接続 ID が必要です。 この ID を持っていない場合は、[ 広告ソースの Platform への接続 ](../../api/create/advertising/ads.md) チュートリアルを参照してください。

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して広告システムに正常に接続するために必要な追加情報を示しています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

## データテーブルの探索

広告システムのベース接続を使用すると、データリクエストを実行してGETテーブルを調べることができます。 次の呼び出しを使用して、検査または [!DNL Platform] に取り込むテーブルのパスを検索します。

**API 形式**

```https
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | 広告システムのベース接続の ID。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/2484f2df-c057-4ab5-84f2-dfc0577ab592/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、から広告システムへのテーブルの配列です。 [!DNL Platform] に取り込むテーブルを見つけ、その `path` プロパティをメモします。これは、次の手順でその構造を検査するためにテーブルを指定する必要があるからです。

```json
[
    {
        "type": "table",
        "name": "v201809.ACCOUNT_PERFORMANCE_REPORT",
        "path": "v201809.ACCOUNT_PERFORMANCE_REPORT",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "v201809.ADGROUP_PERFORMANCE_REPORT",
        "path": "v201809.ADGROUP_PERFORMANCE_REPORT",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "v201809.AD_CUSTOMIZERS_FEED_ITEM_REPORT",
        "path": "v201809.AD_CUSTOMIZERS_FEED_ITEM_REPORT",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "v201809.AD_PERFORMANCE_REPORT",
        "path": "v201809.AD_PERFORMANCE_REPORT",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## テーブルの構造のInspect

広告システムからテーブルの構造を調べるには、テーブルのパスをクエリパラメーターとして指定してGETリクエストを実行します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | 広告システムの接続 ID。 |
| `{TABLE_PATH}` | 広告システム内のテーブルのパス。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/2484f2df-c057-4ab5-84f2-dfc0577ab592/explore?objectType=table&object=v201809.AD_PERFORMANCE_REPORT' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、テーブルの構造が返されます。 テーブルの各列に関する詳細は、`columns` 配列の要素内にあります。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "CallOnlyPhoneNumber",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "AdGroupId",
                "type": "long",
                "xdm": {
                    "type": "integer",
                    "minimum": -9007199254740992,
                    "maximum": 9007199254740991
                }
            },
            {
                "name": "AdGroupName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Date",
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

このチュートリアルに従って、広告システムを探索し、取り込むテーブルのパスを見つけ [!DNL Platform]、その構造に関する情報を取得しました。 次のチュートリアルでは、この情報を使用して [ 広告システムからデータを収集し、Platform に取り込む ](../collect/advertising.md) ことができます。
