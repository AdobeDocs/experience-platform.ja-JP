---
keywords: Experience Platform；ホーム；人気の高いトピック；ポリシーの適用；マーケティングアクションapi;APIベースの適用；データガバナンス
solution: Experience Platform
title: マーケティングアクションAPIエンドポイント
topic-legacy: developer guide
description: マーケティングアクションは、Adobe Experience Platform データガバナンスのコンテキストでは、Experience Platform データコンシューマーがおこなうアクションのことで、そのため、データ使用ポリシーの違反がないかを確認する必要があります。
exl-id: bc16b318-d89c-4fe6-bf5a-1a4255312f54
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 10%

---

# マーケティングアクションエンドポイント

Adobe Experience Platform[!DNL Data Governance]に関連するマーケティングアクションは、[!DNL Experience Platform]データ消費者が行うアクションで、データ使用ポリシーの違反をチェックする必要があります。

Policy Service APIの`/marketingActions`エンドポイントを使用して、組織のマーケティングアクションを管理できます。

## はじめに

このガイドで使用されるAPIエンドポイントは、[[!DNL Policy Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)の一部です。 先に進む前に、[はじめにガイド](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しの読み方、および任意の[!DNL Experience Platform] APIの呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## マーケティングアクションのリストを取得{#list}

コアマーケティングアクションまたはカスタムマーケティングアクションのリストは、それぞれ`/marketingActions/core`または`/marketingActions/custom`にGETリクエストを行うことで取得できます。

**API 形式**

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、組織が管理するカスタムマーケティングアクションのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、取得した各マーケティングアクションの詳細（`name`と`href`を含む）を返します。 `href`値は、[データ使用ポリシー](policies.md#create-policy)を作成する際のマーケティングアクションの識別に使用されます。

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
| `name` | マーケティングアクションの名前。[特定のマーケティングアクション](#lookup)を検索する際に一意の識別子として機能します。 |
| `_links.self.href` | マーケティングアクションのURI参照。[データ使用ポリシー](policies.md#create-policy)の作成時に`marketingActionsRefs`配列を完了するのに使用できます。 |

## 特定のマーケティングアクションの検索 {#lookup}

GETリクエストのパスにマーケティングアクションの`name`プロパティを含めて、特定のマーケティングアクションの詳細を調べます。

**API 形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 検索するマーケティングアクションの`name`プロパティ。 |

**リクエスト**

次のリクエストは、`combineData`という名前のカスタムマーケティングアクションを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/combineData \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

responseオブジェクトには、マーケティングアクションの詳細が含まれます。この中には、[データ使用ポリシー](policies.md#create-policy) (`marketingActionsRefs`)を定義する際に、マーケティングアクションを参照するために必要なパス(`_links.self.href`)が含まれます。

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

## カスタムマーケティングアクション{#create-update}の作成または更新

PUTリクエストのパスにマーケティングアクションの既存の名前または目的の名前を含めることで、新しいカスタムマーケティングアクションを作成したり、既存のマーケティングアクションを更新したりできます。

**API 形式**

```http
PUT /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 作成または更新するマーケティングアクションの名前。 指定した名前のマーケティングアクションが既にシステムに存在する場合は、そのマーケティングアクションが更新されます。 存在しない場合は、指定した名前に対して新しいマーケティングアクションが作成されます。 |

**リクエスト**

次のリクエストは、同じ名前のマーケティングアクションがシステムにまだ存在しない場合に、`crossSiteTargeting`という名前の新しいマーケティングアクションを作成します。 `crossSiteTargeting`マーケティングアクションが存在する場合、代わりに、この呼び出しによって、ペイロードで提供されたプロパティに基づいてそのマーケティングアクションが更新されます。

```shell
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
| `name` | 作成または更新するマーケティングアクションの名前。 <br><br>**重要**:このプロパティはパス `{MARKETING_ACTION_NAME}` 内のものと一致する必要があります。一致しない場合は、HTTP 400(Bad Request)エラーが発生します。つまり、マーケティングアクションが作成されると、その`name`プロパティは変更できません。 |
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

## カスタムマーケティングアクションの削除{#delete}

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

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の応答本文を持つHTTP Status 200(OK)を返します。

[マーケティングアクション](#look-up)を検索して削除を確認できます。 マーケティングアクションがシステムから削除された場合は、HTTP 404 （見つかりません）エラーが表示されます。
