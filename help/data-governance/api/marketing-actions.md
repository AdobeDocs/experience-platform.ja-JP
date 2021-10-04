---
keywords: Experience Platform;ホーム;人気のトピック;ポリシーの適用;マーケティングアクションapi;API ベースの適用;データガバナンス
solution: Experience Platform
title: マーケティングアクション API エンドポイント
topic-legacy: developer guide
description: マーケティングアクションは、Adobe Experience Platform データガバナンスのコンテキストでは、Experience Platform データコンシューマーがおこなうアクションのことで、そのため、データ使用ポリシーの違反がないかを確認する必要があります。
exl-id: bc16b318-d89c-4fe6-bf5a-1a4255312f54
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 100%

---

# マーケティングアクションエンドポイント

マーケティングアクションは、Adobe Experience Platform [!DNL Data Governance] のコンテキストでは、[!DNL Experience Platform] データコンシューマーがおこなうアクションのことです。そのため、データ使用ポリシーの違反がないかを確認する必要があります。

Policy Service API の `/marketingActions` エンドポイントを使用して、組織のマーケティングアクションを管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Policy Service] API](https://www.adobe.io/experience-platform-apis/references/policy-service/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## マーケティングアクションのリストの取得 {#list}

コアマーケティングアクションとカスタムマーケティングアクションのリストは、`/marketingActions/core` と `/marketingActions/custom` のそれぞれに GET リクエストをおこなうことで取得できます。

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

応答が成功すると、取得した各マーケティングアクションの詳細（`name` と `href` を含む）が返されます。`href` 値は、[データ使用ポリシーを作成](policies.md#create-policy)する際のマーケティングアクションの識別に使用されます。

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
| `_page.count` | 返されたマーケティングアクションの合計数。 |
| `children` | 取得したマーケティングアクションの詳細を含むオブジェクトの配列。 |
| `name` | マーケティングアクションの名前。[特定のマーケティングアクションを検索](#lookup)する際に一意の ID として機能します。 |
| `_links.self.href` | マーケティングアクションの URI 参照。[データ使用ポリシーを作成](policies.md#create-policy)する際に `marketingActionsRefs` 配列を完了するのに使用できます。 |

## 特定のマーケティングアクションの検索 {#lookup}

GET リクエストのパスにマーケティングアクションの `name` プロパティを含めて、特定のマーケティングアクションの詳細を調べます。

**API 形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 検索するマーケティングアクションの `name` プロパティ。 |

**リクエスト**

次のリクエストは、`combineData` という名前のカスタムマーケティングアクションを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/combineData \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答オブジェクトには、マーケティングアクションの詳細が含まれています。これには、[データ使用ポリシーを定義](policies.md#create-policy)する際にマーケティングアクションの参照（`marketingActionsRefs`）に必要なパス（`_links.self.href`）が含まれます。

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

## カスタムマーケティングアクションの作成または更新 {#create-update}

PUT リクエストのパスにマーケティングアクションの既存の名前または目的の名前を含めることで、新しいカスタムマーケティングアクションを作成したり、既存のマーケティングアクションを更新したりできます。

**API 形式**

```http
PUT /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 作成または更新するマーケティングアクションの名前。指定した名前のマーケティングアクションがシステム内に既に存在する場合は、そのマーケティングアクションが更新されます。存在しない場合は、指定した名前に対して新しいマーケティングアクションが作成されます。 |

**リクエスト**

次のリクエストは、同じ名前のマーケティングアクションがシステムにまだ存在しない場合に、`crossSiteTargeting` という名前の新しいマーケティングアクションを作成します。`crossSiteTargeting` マーケティングアクションが存在する場合は、この呼び出しによって、ペイロードに指定されたプロパティに基づいてそのマーケティングアクションが更新されます。

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
| `name` | 作成または更新するマーケティングアクションの名前。<br><br>**重要**：このプロパティはパスの `{MARKETING_ACTION_NAME}` と一致する必要があります。一致しない場合は、HTTP 400（無効なリクエスト）エラーが発生します。つまり、マーケティングアクションを作成した後で、その `name` プロパティを変更することはできません。 |
| `description` | マーケティングアクションの詳細なコンテキストを提供する説明（オプション）。 |

**応答** 

成功した応答は、マーケティングアクションの詳細を返します。既存のマーケティングアクションが更新された場合、応答は HTTP ステータス 200（OK）を返します。新しいマーケティングアクションが作成された場合、応答は HTTP ステータス 201（作成済み）を返します。

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

## カスタムマーケティングアクションの削除 {#delete}

カスタムマーケティングアクションを削除するには、DELETE リクエストのパスにその名前を含めます。

>[!NOTE]
>
>既存のポリシーで参照されているマーケティングアクションは削除できません。これらのマーケティングアクションの 1 つを削除しようとすると、HTTP 400（無効なリクエスト）エラーと、マーケティングアクションを参照するすべてのポリシーの ID を含むメッセージが表示されます。

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

成功した応答は、空白の応答本文を持つ HTTP ステータス 200（OK）を返します。

[マーケティングアクションを検索](#look-up)することで、削除を確認できます。マーケティングアクションがシステムから削除された場合は、HTTP 404 （見つかりません）エラーが返されます。
