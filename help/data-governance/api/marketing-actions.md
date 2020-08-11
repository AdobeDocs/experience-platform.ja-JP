---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マーケティングアクション
topic: developer guide
translation-type: tm+mt
source-git-commit: cb3a17aa08c67c66101cbf3842bf306ebcca0305
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 6%

---


# マーケティングアクション endpoint

A marketing action, in the context of the Adobe Experience Platform [!DNL Data Governance], is an action that an [!DNL Experience Platform] data consumer takes, for which there is a need to check for violations of data usage policies.

ポリシーサービスAPIのエンドポイントを使用して、組織のマーケティングアクションを管理でき `/marketingActions` ます。

## はじめに

The API endpoints used in this guide are part of the [[!DNL Policy Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml). 先に進む前に、 [はじめに](./getting-started.md)[!DNL Experience Platform] 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## マーケティングアクションのリストの取得 {#list}

コアマーケティングアクションのリストを取得するには、またはに対してそれぞれGETリクエストを作成し `/marketingActions/core` ま `/marketingActions/custom`す。

**API 形式**

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、組織が管理するカスタムマーケティングアクションのリストを取得します。

```sh
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した応答は、取得した各マーケティングアクションの詳細（およびを含む）を返 `name` し `href`ます。 この `href` 値は、データ使用ポリシーを [作成する際のマーケティングアクションを識別するために使用されます](policies.md#create-policy)。

```json
{
    "_page": {
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

| プロパティ | 説明 |
| --- | --- |
| `_page.count` | 返されたマーケティングアクションの合計数です。 |
| `children` | 取得したマーケティングアクションの詳細を含むオブジェクトの配列。 |
| `name` | マーケティングアクションの名前。特定のマーケティングアクションを [検索する際に一意の識別子として機能し](#lookup)ます。 |
| `_links.self.href` | マーケティングアクションのURI参照。データ使用ポリシーの `marketingActionsRefs` 作成時に配列を完成させるのに使用できます [](policies.md#create-policy)。 |

## 特定のマーケティングアクションの検索 {#lookup}

特定のマーケティングアクションの詳細を調べるには、GETリクエストのパスにマーケティングアクションの `name` プロパティを含めます。

**API 形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | The `name` property of the marketing action you want to look up. |

**リクエスト**

次のリクエストは、という名前のカスタムマーケティングアクションを取得し `combineData`ます。

```sh
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/combineData \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

The response object contains the details for the marketing action, including the path (`_links.self.href`) needed to reference the marketing action when [defining a data usage policy](policies.md#create-policy) (`marketingActionsRefs`).

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

## Create or update a custom marketing action {#create-update}

PUTリクエストのパスにマーケティングアクションの既存の名前または目的の名前を含めることで、新しいカスタムマーケティングアクションを作成したり、既存のマーケティングアクションを更新したりできます。

**API 形式**

```http
PUT /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 作成または更新するマーケティングアクションの名前。 指定した名前のマーケティングアクションが既にシステムに存在する場合は、そのマーケティングアクションが更新されます。 存在しない場合は、指定した名前に対して新しいマーケティングアクションが作成されます。 |

**リクエスト**

次のリクエストでは、同じ名前のマーケティングアクションがシステムにまだ存在しない場合 `crossSiteTargeting`、という名前の新しいマーケティングアクションが作成されます。 マーケティングアクションが存在する場合、代わりに、この呼び出しによって、ペイロードで提供されたプロパティに基づいて、そのマーケティングアクションが更新されます。 `crossSiteTargeting`

```sh
curl -X PUT \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "name": "crossSiteTargeting",
        "description": "Perform targeting on information obtained across multiple web sites."
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 作成または更新するマーケティングアクションの名前。 <br><br>**重要&#x200B;**:このプロパティはパス`{MARKETING_ACTION_NAME}`内のものと一致する必要があります。一致しない場合は、HTTP 400(Bad Request)エラーが発生します。 つまり、マーケティングアクションが作成されると、その`name`プロパティを変更できません。 |
| `description` | マーケティングアクションの詳細なコンテキストを提供する説明です（オプション）。 |

**応答** 

成功した応答は、マーケティングアクションの詳細を返します。 既存のマーケティングアクションが更新された場合、応答はHTTPステータス200(OK)を返します。 新しいマーケティングアクションが作成された場合、応答はHTTPステータス201（作成済み）を返します。

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

## Delete a custom marketing action {#delete}

DELETEリクエストのパスに名前を含めると、カスタムマーケティングアクションを削除できます。

>[!NOTE]
>
>既存のポリシーで参照されているマーケティングアクションは削除できません。 これらのマーケティングアクションの1つを削除しようとすると、HTTP 400(Bad Request)エラーと、マーケティングアクションを参照するすべてのポリシーのIDを含むメッセージが表示されます。

**API 形式**

```http
DELETE /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 削除するマーケティングアクションの名前。 |

**リクエスト**

```sh
curl -X DELETE \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の応答本文を持つHTTP Status 200(OK)を返します。

You can confirm the deletion by attempting to [look up the marketing action](#look-up). マーケティングアクションがシステムから削除された場合は、HTTP 404 （見つかりません）エラーが表示されます。
