---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マーケティングアクション
topic: developer guide
translation-type: tm+mt
source-git-commit: 0534fe8dcc11741ddc74749d231e732163adf5b0
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 90%

---


# マーケティングアクション

A marketing action, in the context of the Adobe Experience Platform [!DNL Data Governance], is an action that an [!DNL Experience Platform] data consumer takes, for which there is a need to check for violations of data usage policies.

API でマーケティングアクションを使用する場合は、`/marketingActions` エンドポイントを使用する必要があります。

## すべてのマーケティングアクションのリスト表示

すべてのマーケティングアクションのリストを表示するには、指定されたコンテナのすべてのポリシーを返す `/marketingActions/core` または `/marketingActions/custom` に GET リクエストを送信します。

**API 形式**

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストでは、IMS 組織で定義されているすべてのカスタムマーケティングアクションのリストを返します。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答オブジェクトは、コンテナ内のマーケティングアクションの合計数（`count`）を返します。また、`children` 配列には、各マーケティングアクションの詳細（`name` や `href` など）が含まれています。このパス（`_links.self.href`）は、[データ使用ポリシーの作成 ](policies.md#create-policy)時に `marketingActionsRefs` 配列を完成させるために使用されます。

```JSON
{
    "_page": {
        "start": "sampleMarketingAction",
        "count": 2
    },
    "_links": {
        "page": {
            "href": "https://platform.adobe.io/marketingActions/custom?{?limit,start,property}",
            "templated": true
        }
    },
    "children": [
        {
            "name": "sampleMarketingAction",
            "description": "Marketing Action description.",
            "imsOrg": "{IMS_ORG}",
            "created": 1550714012088,
            "createdClient": "string",
            "createdUser": "string",
            "updated": 1550714012088,
            "updatedClient": "string",
            "updatedUser": "string",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction"
                }
            }
        },
        {
            "name": "newMarketingAction",
            "description": "Another marketing action.",
            "imsOrg": "{IMS_ORG}",
            "created": 1550793833224,
            "createdClient": "string",
            "createdUser": "string",
            "updated": 1550793833224,
            "updatedClient": "string",
            "updatedUser": "string",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/newMarketingAction"
                }
            }
        }
    ]
}
```

## 特定のマーケティングアクションの検索

また、検索（GET）リクエストを実行して、特定のマーケティングアクションの詳細を表示することもできます。これは、マーケティングアクションの `name` を使用しておこないます。名前が不明な場合は、前述のリスト表示（GET）リクエストを使用すれば見つかります。

**API 形式**

```http
GET /marketingActions/core/{marketingActionName}
GET /marketingActions/custom/{marketingActionName}
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/combineData \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答オブジェクトには、マーケティングアクションの詳細が含まれています。これには、データ使用ポリシー（`marketingActionsRefs`）を定義する際にマーケティングアクションを参照するために必要なパス（`_links.self.href`）が含まれます。

```JSON
{
    "name": "combineData",
    "description": "Combine multiple data sources together.",
    "imsOrg": "{IMS_ORG}",
    "created": 1550793805590,
    "createdClient": "string",
    "createdUser": "string",
    "updated": 1550793805590,
    "updatedClient": "string",
    "updatedUser": "string",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/combineData"
        }
    }
}
```

## マーケティングアクションの作成または更新

The [!DNL Policy Service] API allows you to define your own marketing actions, as well as update existing ones. 作成と更新は、どちらも、マーケティングアクションの名前に対する PUT 操作でおこなわれます。

**API 形式**

```http
PUT /marketingActions/custom/{marketingActionName}
```

**リクエスト**

後続のリクエストでは、リクエストペイロード内の `name` が API 呼び出し内の `{marketingActionName}` と同じであることに注意してください。ポリシーの `id` は読み取り専用でシステムで生成されますが、それとは異なり、マーケティングアクションを作成するには、作成時にマーケティングアクションの&#x200B;_意図した_&#x200B;名前を指定する必要があります。

>[!NOTE]
>
> この呼び出しで `{marketingActionName}` を指定できない場合、`/marketingActions/custom` エンドポイントに対して PUT を直接実行できないので、405 エラー（Method Not Allowed）が発生します。また、ペイロード内の `name` がパス内の `{marketingActionName}` と一致しない場合は、400 エラー（Bad Request）が発生します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "crossSiteTargeting",
        "description": "Perform targeting on information obtained across multiple web sites."
      }'
```

**応答**

正常に作成された場合は、HTTP ステータス201（Created）が返され、応答本文に、新しく作成されたマーケティングアクションの詳細が含まれています。応答内の `name` は、リクエストで送信された内容と一致しているはずです。

```JSON
{
    "name": "crossSiteTargeting",
    "description": "Perform targeting on information obtained across multiple web sites.",
    "imsOrg": "{IMS_ORG}",
    "created": 1550713341915,
    "createdClient": "string",
    "createdUser": "string",
    "updated": 1550713856390,
    "updatedClient": "string",
    "updatedUser": "string",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting"
        }
    }
}
```

## マーケティングアクションの削除

削除するマーケティングアクションの `{marketingActionName}` に DELETE リクエストを送信して、マーケティングアクションを削除できます。

>[!NOTE]
>
> 既存のポリシーで参照されているマーケティングアクションを削除することはできません。削除しようとすると、400 エラー（Bad Request）が発生します。また、削除しようとしているマーケティングアクションへの参照を含んだポリシー（複数の場合あり）の `id`（または複数の ID）を明示したエラーメッセージが表示されます。

**API 形式**

```http
DELETE /marketingActions/custom/{marketingActionName}
```

**リクエスト**

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

マーケティングアクションが正常に削除された場合、応答本文は空白になり、HTTP ステータス 200（OK）が表示されます。

マーケティングアクションを検索（GET）してみれば、削除を確認できます。このマーケティングアクションは既に削除されているので、HTTP ステータス 404（Not Found）が返され、「Not Found」というエラーメッセージが表示されます。
