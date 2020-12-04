---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ラベルの端点
topic: developer guide
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 6%

---


# ラベルの端点

データ使用ラベルを使用すると、そのデータに適用される使用ポリシーに従ってデータを分類できます。 の `/labels` エンドポイント [!DNL Policy Service API] を使用すると、エクスペリエンスアプリケーション内のデータ使用ラベルをプログラムで管理できます。

>[!NOTE]
>
>エンドポイント `/labels` は、データ使用ラベルの取得、作成および更新のみに使用されます。 API呼び出しを使用してデータセットとフィールドにラベルを追加する手順については、データセットラベルの [管理に関するガイドを参照してください](../labels/dataset-api.md)。

## はじめに

The API endpoint used in this guide is part of the [[!DNL Policy Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml). 先に進む前に、 [はじめに](getting-started.md)[!DNL Experience Platform] 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## Retrieve a list of labels {#list}

すべてのラベルまたは `core` ラベルをリストするには、それぞれに対してGET要求を行う `custom` か、または行う `/labels/core``/labels/custom`かを指定します。

**API 形式**

```http
GET /labels/core
GET /labels/custom
```

**リクエスト**

次のリクエストでは、組織で作成されたすべてのカスタムラベルをリストします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、システムから取得されたカスタムラベルのリストが返されます。 上の例のリクエストはに対して行われたので `/labels/custom`、以下の応答にはカスタムラベルのみが表示されています。

```json
{
    "_page": {
        "count": 2
    },
    "_links": {
        "page": {
            "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom?{?limit,start,property}",
            "templated": true
        }
    },
    "children": [
        {
            "name": "L1",
            "category": "Custom",
            "friendlyName": "Banking Information",
            "description": "Data containing banking information for a customer.",
            "imsOrg": "{IMS_ORG}",
            "sandboxName": "{SANDBOX_NAME}",
            "created": 1594396718731,
            "createdClient": "{CLIENT_ID}",
            "createdUser": "{USER_ID}",
            "updated": 1594396718731,
            "updatedClient": "{CLIENT_ID}",
            "updatedUser": "{USER_ID}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L1"
                }
            }
        },
        {
            "name": "L2",
            "category": "Custom",
            "friendlyName": "Purchase History Data",
            "description": "Data containing information on past transactions",
            "imsOrg": "{IMS_ORG}",
            "sandboxName": "{SANDBOX_NAME}",
            "created": 1594397415663,
            "createdClient": "{CLIENT_ID}",
            "createdUser": "{USER_ID}",
            "updated": 1594397728708,
            "updatedClient": "{CLIENT_ID}",
            "updatedUser": "{USER_ID}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L2"
                }
            }
        }
    ]
}
```

## ラベルを検索 {#look-up}

特定のラベルを検索するには、そのラベルの `name` プロパティを [!DNL Policy Service] APIへのGET要求のパスに含めます。

**API 形式**

```http
GET /labels/core/{LABEL_NAME}
GET /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | The `name` property of the custom label you want to look up. |

**リクエスト**

次のリクエストは、パスに示されているカスタムラベル `L2`を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L2' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、カスタムラベルの詳細が返されます。

```json
{
    "name": "L2",
    "category": "Custom",
    "friendlyName": "Purchase History Data",
    "description": "Data containing information on past transactions",
    "imsOrg": "{IMS_ORG}",
    "sandboxName": "{SANDBOX_NAME}",
    "created": 1594397415663,
    "createdClient": "{CLIENT_ID}",
    "createdUser": "{USER_ID}",
    "updated": 1594397728708,
    "updatedClient": "{CLIENT_ID}",
    "updatedUser": "{USER_ID}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L2"
        }
    }
}
```

## カスタムラベルの作成または更新 {#create-update}

カスタムラベルを作成または更新するには、 [!DNL Policy Service] APIに対してPUTリクエストを行う必要があります。

**API 形式**

```http
PUT /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | カスタムラベルの `name` プロパティ。 この名前のカスタムラベルが存在しない場合は、新しいラベルが作成されます。 存在する場合は、そのラベルが更新されます。 |

**リクエスト**

次のリクエストは、顧客が選択した支払計画に関する情報を含むデータを記述するための新しいラベルを作成し `L3`ます。

```shell
curl -X PUT \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "L3",
        "category": "Custom",
        "friendlyName": "Payment Plan",
        "description": "Data containing information on selected payment plans."
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ラベルの一意の文字列識別子。 この値は参照目的で使用され、ラベルをデータセットやフィールドに適用するので、短く簡潔にすることをお勧めします。 |
| `category` | ラベルのカテゴリ。 カスタムラベル用に独自のカテゴリを作成できますが、ラベルをUIに表示する `Custom` 場合は、を使用することを強くお勧めします。 |
| `friendlyName` | ラベルのわかりやすい名前。表示目的で使用されます。 |
| `description` | （オプション）詳細なコンテキストを提供するラベルの説明です。 |

**応答** 

正常な応答は、カスタムラベルの詳細を返します。既存のラベルが更新された場合はHTTPコード200(OK)、新しいラベルが作成された場合は201（作成済み）を返します。

```json
{
  "name": "L3",
  "category": "Custom",
  "friendlyName": "Payment Plan",
  "description": "Data containing information on selected payment plans.",
  "imsOrg": "{IMS_ORG}",
  "sandboxName": "{SANDBOX_NAME}",
  "created": 1529696681413,
  "createdClient": "{CLIENT_ID}",
  "createdUser": "{USER_ID}",
  "updated": 1529697651972,
  "updatedClient": "{CLIENT_ID}",
  "updatedUser": "{USER_ID}",
  "_links": {
    "self": {
      "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/labels/custom/L3"
    }
  }
}
```

## 次の手順

このガイドでは、Policy Service APIでの `/labels` エンドポイントの使用について説明しました。 データセットとフィールドにラベルを適用する手順については、『 [データセットラベルAPIガイド](../labels/dataset-api.md)』を参照してください。