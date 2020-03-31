---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マーケティングアクション
topic: developer guide
translation-type: tm+mt
source-git-commit: 08d02e7323f75c450e7a250835f26a569685cdd1

---


# マーケティングアクション

マーケティングアクションは、Adobe Experience Platform Data Governanceの観点から見ると、Experience Platformデータコンシューマーが行うアクションで、データ使用ポリシーの違反を確認する必要があります。

APIでマーケティングアクションを使用する場合は、エンドポイントを使用する必要が `/marketingActions` あります。

## リストのすべてのマーケティングアクション

すべてのマーケティングアクションのリストを表示するには、指定したコンテナに対してGET `/marketingActions/core` リクエストを行 `/marketingActions/custom` うか、指定したアクションのすべてのポリシーを返すことができます。

**API形式**

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、IMS組織で定義されているすべてのリストのカスタムマーケティングアクションを返します。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

responseオブジェクトは、コンテナ内のマーケティングアクションの合計数を提供し`count`( `children` )、配列には、マーケティングアクションの詳細（およびを含む）が含ま `name` れ `href` ています。 このパス(`_links.self.href`)は、データ使用ポリシーの作成時に `marketingActionsRefs` アレイを [完了するために使用されます](policies.md#create-policy)。

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

また、特定のマーケティングアクションの詳細を表示するための参照(GET)リクエストを実行することもできます。 これは、マーケティングアクシ `name` ョンのを使用して行います。 名前が不明な場合は、前述のリスト(GET)リクエストを使用して検索できます。

**API形式**

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

応答オブジェクトには、マーケティングアクションの詳細が含まれます。これには、データ使用ポリシーを定義する際にマーケティングアクションを参照するために必要なパス(`_links.self.href`)が含まれ`marketingActionsRefs`ます。

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

Policy Service APIを使用すると、独自のマーケティングアクションを定義し、既存のマーケティングアクションを更新できます。 作成と更新は、両方ともマーケティングアクションの名前に対するPUT操作を使用して行われます。

**API形式**

```http
PUT /marketingActions/custom/{marketingActionName}
```

**リクエスト**

後続のリクエストでは、リクエストペイロ `name` ード内のがAPI呼び出し内のと同じであ `{marketingActionName}` ることに注意してください。 読み取り専 `id` 用でシステム生成のポリシーとは異なり、マーケティングアクションを作成するには、作成時にマーケティングアクションの意図し _た_ 名前を指定する必要があります。

>[!NOTE] この呼び出しでのPUT `{marketingActionName}` の指定に失敗すると、エンドポイントへのPUTの直接実行が許可されないので、405エラー(Method Not Allowed)が発生 `/marketingActions/custom` します。 また、ペイロード `name` 内のがパス内のと一致しな `{marketingActionName}` い場合は、400エラー(Bad Request)が発生します。

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

正常に作成された場合は、HTTPステータス201（作成済み）が返され、応答本文に新しく作成されたマーケティングアクションの詳細が含まれます。 応答内 `name` のが、要求で送信された内容と一致する必要があります。

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

削除するマーケティングアクションのにDELETEリクエストを送信することで、マーケテ `{marketingActionName}` ィングアクションを削除できます。

>[!NOTE] 既存のポリシーで参照されているマーケティングアクションを削除することはできません。 このように設定すると、400エラー（NGリクエスト）が発生し、削除しようとしているマーケティングアクションへの参照を含むポリシー（または複数のID）の `id` （または複数のID）が含まれるエラーメッセージが表示されます。

**API形式**

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

マーケティングアクションが正常に削除された場合、応答本文は空白になり、HTTPステータス200(OK)が表示されます。

マーケティングアクションを参照(GET)することで、削除を確認できます。 マーケティングアクションが削除されたので、HTTPステータス404（見つかりません）と「見つかりません」というエラーメッセージが表示されます。
