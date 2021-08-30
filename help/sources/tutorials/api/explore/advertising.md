---
keywords: Experience Platform；ホーム；人気のあるトピック；広告システム；広告システム
solution: Experience Platform
title: フローサービスAPIを使用した広告システムの調査
topic-legacy: overview
description: フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。 このチュートリアルでは、フローサービスAPIを使用して広告システムを調べます。
exl-id: 3016ce1e-12e6-47ce-a4c5-52f8d440f515
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用した広告システムの調査

ベース接続が作成されたら、一意のベース接続IDを使用してソースのデータ構造とコンテンツをナビゲートし、調べることができるようになりました。 これにより、データフローを作成してAdobe Experience Platformに引き渡す前に、特定の項目とそれぞれのデータタイプおよび形式を識別できます。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して広告システムを調べます。

## はじめに

>[!IMPORTANT]
このチュートリアルでは、広告ソースに対して一意のベース接続IDを持っている必要があります。 このIDがない場合は、広告ソースをPlatform](../../api/create/advertising/ads.md)に接続する方法に関するチュートリアルを参照してください。[

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して広告システムに正しく接続するために知っておく必要がある追加情報を示します。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../landing/api-guide.md)を参照してください。

## データテーブルの参照

広告システムのベース接続を使用して、GETリクエストを実行することでデータテーブルを調べることができます。 次の呼び出しを使用して、[!DNL Platform]に検査または取り込むテーブルのパスを見つけます。

**API 形式**

```https
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | 広告システムのベース接続のID。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/2484f2df-c057-4ab5-84f2-dfc0577ab592/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、から広告システムへのテーブルの配列です。 [!DNL Platform]に取り込むテーブルを探し、その`path`プロパティをメモします。これは、次の手順でその構造を調べるために指定する必要があるからです。

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

## Inspectテーブルの構造

広告システムからテーブルの構造を調べるには、テーブルのパスをクエリパラメーターとして指定してGETリクエストを実行します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | 広告システムの接続ID。 |
| `{TABLE_PATH}` | 広告システム内のテーブルのパス。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/2484f2df-c057-4ab5-84f2-dfc0577ab592/explore?objectType=table&object=v201809.AD_PERFORMANCE_REPORT' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、テーブルの構造が返されます。 各テーブルの列に関する詳細は、`columns`配列の要素内にあります。

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

このチュートリアルでは、広告システムを調べ、[!DNL Platform]に取り込むテーブルのパスを見つけ、その構造に関する情報を取得しました。 この情報は、次のチュートリアルで[広告システムからデータを収集し、Platform](../collect/advertising.md)に取り込む際に使用できます。
