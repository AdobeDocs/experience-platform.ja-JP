---
keywords: Experience Platform；ホーム；人気の高いトピック；e コマース；e コマース
solution: Experience Platform
title: フローサービス API を使用した e コマース接続の調査
topic-legacy: overview
description: このチュートリアルでは、フローサービス API を使用して e コマース接続を調べます。
exl-id: 832ce399-6c9f-40da-8e7c-5434503c16b6
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 35%

---

# を使用した e コマース接続の調査 [!DNL Flow Service] API

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされるすべてのソースから接続できます。

このチュートリアルでは、 [!DNL Flow Service] サードパーティを調べるための API **[!UICONTROL e コマース]** 接続。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Sources]](../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [[!DNL Sandboxes]](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 **[!UICONTROL e コマース]** を使用した接続 [!DNL Flow Service] API

### 接続 ID の取得

を参照するには、 **[!UICONTROL e コマース]** 接続 [!DNL Platform] API を使用する場合は、有効な接続 ID が必要です。 まだ **[!UICONTROL e コマース]** 操作する接続は、次のチュートリアルを通じて作成できます。

* [Shopify](../create/ecommerce/shopify.md)

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## データテーブルの調査

を使用して、 **[!UICONTROL e コマース]** 接続 ID を使用する場合、データリクエストを実行してGETテーブルを調査できます。 次の呼び出しを使用して、検査または取り込むテーブルのパスを見つけます。 [!DNL Platform].

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{CONNECTION_ID}` | お使いの **[!UICONTROL e コマース]** 接続 ID。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/582f4f8d-71e9-4a5c-a164-9d2056318d6c/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、 **[!UICONTROL e コマース]** 接続。 に取り込むテーブルを探します。 [!DNL Platform] そしてそれを書き留める `path` プロパティに含める必要がある場合は、次の手順で指定して、その構造を調べます。

```json
[
    {
        "type": "table",
        "name": "Shopify.Abandoned_Checkout_Discount_Codes",
        "path": "Shopify.Abandoned_Checkout_Discount_Codes",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Shopify.Abandoned_Checkout_Line_Items",
        "path": "Shopify.Abandoned_Checkout_Line_Items",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Shopify.Blogs",
        "path": "Shopify.Blogs",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Shopify.Orders",
        "path": "Shopify.Orders",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## Inspectテーブルの構造

テーブルの構造を **[!UICONTROL e コマース]** 接続、GETリクエストを実行し、 `object` クエリパラメーター。

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | の接続 ID **[!UICONTROL e コマース]** 接続。 |
| `{TABLE_PATH}` | 内のテーブルのパス **[!UICONTROL e コマース]** 接続。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/582f4f8d-71e9-4a5c-a164-9d2056318d6c/explore?objectType=table&object=Orders' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたテーブルの構造を返します。 各テーブルの列に関する詳細は、 `columns` 配列。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "Blog_Id",
                "type": "double",
                "xdm": {
                    "type": "number"
                }
            },
            {
                "name": "Title",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Created_At",
                "type": "string",
                "meta:xdmType": "date-time",
                "xdm": {
                    "type": "string",
                    "format": "date-time"
                }
            },
            {
                "name": "Tags",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            }
        ]
    },
    "data": [
        {
            "Updated_At": "2020-11-05T10:54:36",
            "Title": "News",
            "Commentable": "no",
            "Blog_Id": 5.5458332804E10,
            "Handle": "news",
            "Created_At": "2020-02-14T09:11:15"
        }
    ]
}
```

## 次の手順

このチュートリアルに従って、 **[!UICONTROL e コマース]** 接続で、取り込むテーブルのパスが見つかりました [!DNL Platform]、およびその構造に関する情報を取得しました。 次のチュートリアルでこの情報を使用して、 [e コマースデータを収集して Platform に取り込む](../collect/ecommerce.md).
