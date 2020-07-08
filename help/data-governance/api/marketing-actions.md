---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マーケティングアクション
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---


# マーケティングアクション

Adobe Experience Platformデータガバナンスの観点から見たマーケティングアクションは、 [!DNL Experience Platform] データコンシューマが行うアクションで、データ使用ポリシーの違反を確認する必要があります。

APIでマーケティングアクションを使用するには、エンドポイントを使用する必要があり `/marketingActions` ます。

## すべてのマーケティングアクションのリスト

すべてのマーケティングアクションのリストを表示するために、指定したコンテナに対してGETリクエストを行う `/marketingActions/core` か、指定したのすべてのポリシー `/marketingActions/custom` を返すことができます。

**API形式**

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、IMS組織によって定義されたすべてのカスタムマーケティングアクションのリストを返します。

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

responseオブジェクトは、コンテナ内のマーケティングアクションの合計数を提供し`count`、配列には、マーケティングアクションの詳細（およびを含む） `children` が含まれ `name``href` ます。 このパス(`_links.self.href`)は、データ使用ポリシーの `marketingActionsRefs` 作成時に [アレイを完了するために使用されます](policies.md#create-policy)。

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

また、特定のマーケティングアクションの詳細を表示するためのルックアップ(GET)リクエストを実行することもできます。 これは、マーケティングアクション `name` のを使用して行います。 名前が不明な場合は、前述のリスト(GET)リクエストを使用して見つかります。

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

responseオブジェクトには、マーケティングアクションの詳細が含まれます。この詳細には、データ使用ポリシーを定義する際のマーケティングアクションを参照するために必要なパス(`_links.self.href`)が含まれ`marketingActionsRefs`ます。

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

この [!DNL Policy Service] APIを使用すると、既存のマーケティングアクションを更新するだけでなく、独自のマーケティングアクションを定義することもできます。 作成と更新は、どちらもマーケティングアクションの名前に対するPUT操作を使用して行います。

**API形式**

```http
PUT /marketingActions/custom/{marketingActionName}
```

**リクエスト**

後続のリクエストでは、リクエストペイロード `name` 内の値がAPI呼び出し内の値と同じであるこ `{marketingActionName}` とに注意してください。 読み取り専用でシステム生成 `id` のポリシーとは異なり、マーケティングアクションを作成するには、作成時にマーケティングアクションの __ 目的の名前を指定する必要があります。

>[!NOTE]
>
>この呼び出しでPUTを指定し `{marketingActionName}` ないと、エンドポイントに対して直接PUTを実行できないので、405エラー(Method Not Allowed)が発生し `/marketingActions/custom` ます。 また、ペイロード `name` 内のがパス内のと一致しない場合 `{marketingActionName}` は、400エラー(Bad Request)が発生します。

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

正常に作成されると、HTTPステータス201（作成済み）が返され、応答本文には、新しく作成されたマーケティングアクションの詳細が含まれます。 応答 `name` 内の値は、要求で送信された値と一致する必要があります。

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

削除するマーケティングアクションのにDELETEリクエストを送信することで、マーケティングアクション `{marketingActionName}` を削除できます。

>[!NOTE]
>
>既存のポリシーで参照されているマーケティングアクションは削除できません。 そうすると、400エラー（NGリクエスト）が発生し、削除しようとしているマーケティングアクションへの参照を含むポリシー（またはポリシー）の( `id` または複数のID)が含まれるエラーメッセージが表示されます。

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

マーケティングアクションが正常に削除されると、応答本文は空白になり、HTTPステータスは200(OK)になります。

マーケティングアクションをルックアップ(GET)して、削除を確認できます。 マーケティングアクションが削除されたので、HTTPステータス404（見つかりません）と共に「見つかりません」というエラーメッセージが表示されます。
