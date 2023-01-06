---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: ラベル API エンドポイント
description: Policy Service API を使用して Experience Platform のデータ使用ラベルを管理する方法を説明します。
exl-id: 9a01f65c-01f1-4298-bdcf-b7e00ccfe9f2
source-git-commit: 7b15166ae12d90cbcceb9f5a71730bf91d4560e6
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 100%

---

# ラベルのエンドポイント

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータを分類できます。[!DNL Policy Service API] の `/labels` エンドポイントを使用すると、エクスペリエンスアプリケーション内のデータ使用ラベルをプログラムで管理できます。

>[!NOTE]
>
>`/labels` エンドポイントは、データ使用ラベルの取得、作成、更新のみに使用されます。API 呼び出しを使用してデータセットとフィールドにラベルを追加する方法の手順については、[データセットラベルの管理](../labels/dataset-api.md)のガイドを参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Policy Service API]](https://www.adobe.io/experience-platform-apis/references/policy-service/) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## ラベルのリストの取得 {#list}

`/labels/core` または `/labels/custom` に対してそれぞれ GET リクエストをおこなうことで、`core` または `custom` のすべてのラベルをリストできます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、システムから取得したカスタムラベルのリストが返されます。上記のリクエスト例は `/labels/custom` に対しておこなわれたので、以下の応答はカスタムラベルのみを示しています。

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
            "imsOrg": "{ORG_ID}",
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
            "imsOrg": "{ORG_ID}",
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

## ラベルの検索 {#look-up}

特定のラベルを検索するには、そのラベルの `name` プロパティを [!DNL Policy Service] API への GET リクエストのパスに含めます。

**API 形式**

```http
GET /labels/core/{LABEL_NAME}
GET /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | 検索するサンドボックスの `name` プロパティ。 |

**リクエスト**

次のリクエストは、パスに示されているカスタムラベル `L2` を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L2' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、カスタムラベルの詳細が返されます。

```json
{
    "name": "L2",
    "category": "Custom",
    "friendlyName": "Purchase History Data",
    "description": "Data containing information on past transactions",
    "imsOrg": "{ORG_ID}",
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

## カスタムラベル の作成または更新 {#create-update}

カスタムラベルを作成または更新するには、[!DNL Policy Service] API に PUT リクエストをおこなう必要があります。

**API 形式**

```http
PUT /labels/custom/{LABEL_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{LABEL_NAME}` | カスタムラベルの `name` プロパティ。この名前のカスタムラベルが存在しない場合は、新しいラベルが作成されます。存在する場合は、そのラベルが更新されます。 |

**リクエスト**

次のリクエストは、顧客が選択した支払計画に関する情報を含むデータを記述するための新しいラベル `L3` を作成します。

```shell
curl -X PUT \
  'https://platform.adobe.io/data/foundation/dulepolicy/labels/custom/L3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `name` | ラベルの一意の文字列識別子。この値は検索目的で使用され、ラベルをデータセットとフィールドに適用します。そのため、短く簡潔にすることをお勧めします。 |
| `category` | ラベルのカテゴリ。カスタムラベル用に独自のカテゴリを作成することもできますが、ラベルを UI に表示する場合は `Custom` を使用することを強くお勧めします。 |
| `friendlyName` | 表示目的で使用される、ラベルのわかりやすい名前。 |
| `description` | （オプション）詳細なコンテキストを提供するラベルの説明です。 |

**応答**

応答が成功すると、カスタムラベルの詳細が返されます。既存のラベルが更新された場合は HTTP コード 200（OK）、新しいラベルが作成された場合は 201（作成済み）が返されます。

```json
{
  "name": "L3",
  "category": "Custom",
  "friendlyName": "Payment Plan",
  "description": "Data containing information on selected payment plans.",
  "imsOrg": "{ORG_ID}",
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

このガイドでは、Policy Service API の `/labels` エンドポイントの使用について説明しました。データセットとフィールドにラベルを適用する方法の手順については、[データセットラベル API ガイド](../labels/dataset-api.md)を参照してください。
