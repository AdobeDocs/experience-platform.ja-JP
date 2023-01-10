---
keywords: Experience Platform；ホーム；人気のトピック；広告システム；広告システム
solution: Experience Platform
title: フローサービス API を使用した広告システムの調査
description: フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされるすべてのソースから接続できます。 このチュートリアルでは、フローサービス API を使用して広告システムを調べます。
exl-id: 3016ce1e-12e6-47ce-a4c5-52f8d440f515
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 20%

---

# を使用した広告システムの調査 [!DNL Flow Service] API

ベース接続が作成されたら、一意のベース接続 ID を使用して、ソースのデータ構造およびコンテンツに移動し、調査できるようになりました。 これにより、データフローを作成してAdobe Experience Platformに引き渡す前に、特定の項目とそれぞれのデータタイプおよび形式を識別できます。

このチュートリアルでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) 広告システムを調査する。

## はじめに

>[!IMPORTANT]
>
>このチュートリアルでは、広告ソースに一意のベース接続 ID が必要です。 この ID がない場合、 [Platform への広告ソースの接続](../../api/create/advertising/ads.md) チュートリアル

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Flow Service] API

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

## データテーブルの調査

広告システムのベース接続を使用して、GETリクエストを実行することでデータテーブルを調べることができます。 次の呼び出しを使用して、検査または取り込むテーブルのパスを見つけます。 [!DNL Platform].

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

正常な応答は、から広告システムへのテーブルの配列です。 に取り込むテーブルを探します。 [!DNL Platform] そしてそれを書き留める `path` プロパティに含める必要がある場合は、次の手順で指定して、その構造を調べます。

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

正常な応答は、テーブルの構造を返します。 各テーブルの列に関する詳細は、 `columns` 配列。

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

このチュートリアルに従って、広告システムを調査し、に取り込むテーブルのパスを見つけました。 [!DNL Platform]、およびその構造に関する情報を取得しました。 次のチュートリアルでこの情報を使用して、 [広告システムからデータを収集し、Platform に取り込む](../collect/advertising.md).
