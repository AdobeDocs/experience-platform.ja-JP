---
title: 会社エンドポイント
description: Reactor APIで/companiesエンドポイントを呼び出す方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 6%

---

# 会社エンドポイント

会社は、顧客組織（通常はビジネス）を表します。 Reactor APIでは、これらの会社はIMS組織IDを持つ1:1と一致します。 APIユーザーは、アクセス権のある会社のみを表示できます。 会社には、多くの[プロパティ](./properties.md)が含まれる場合があります。 プロパティは、1つの会社に属します。

Reactor APIの`/companies`エンドポイントを使用すると、エクスペリエンスアプリケーション内でアクセス権のある会社をプログラムで取得できます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml)の一部です。 続行する前に、APIへの認証方法に関する重要な情報について、[はじめにのガイド](../getting-started.md)を参照してください。

## 会社のリストの取得 {#list}

`/companies`エンドポイントにGETリクエストを送信することで、使用権限のある会社のリストを作成できます。 ほとんどの場合、1つだけです。

**API 形式**

```http
GET /companies
```

>[!NOTE]
>
>クエリパラメーターを使用して、リストに表示される会社を次の属性に基づいてフィルタリングできます。<ul><li>`created_at`</li><li>`name`</li><li>`org_id`</li><li>`token`</li><li>`updated_at`</li></ul>詳しくは、[応答のフィルタリング](../guides/filtering.md)に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/companies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、アクセス権のある会社のリストを返します。

```json
{
  "data": [
    {
      "id": "COdb0cd64ad4524440be94b8496416ec7d",
      "type": "companies",
      "attributes": {
        "created_at": "2020-08-13T17:13:30.667Z",
        "name": "Example Company",
        "org_id": "{IMS_ORG}",
        "updated_at": "2020-08-13T17:13:30.667Z",
        "token": "d5a4f682bbae",
        "cjm_enabled": false,
        "edge_enabled": false,
        "edge_events_allotment": null,
        "edge_fanout_ratio": null
      },
      "relationships": {
        "properties": {
          "links": {
            "related": "https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d/properties"
          }
        }
      },
      "links": {
        "self": "https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d",
        "properties": "https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d/properties"
      },
      "meta": {
        "rights": [
          "develop_extensions",
          "manage_properties"
        ],
        "platform_rights": {
          "web": [
            "develop_extensions",
            "manage_properties"
          ],
          "mobile": [
            "develop_extensions",
            "manage_properties"
          ]
        }
      }
    }
  ],
  "meta": {
    "pagination": {
      "current_page": 1,
      "next_page": null,
      "prev_page": null,
      "total_pages": 1,
      "total_count": 1
    }
  }
}
```

## 会社の検索 {#lookup}

特定の会社を検索するには、会社のIDをGETリクエストのパスに含めます。

**API 形式**

```http
GET /companies/{COMPANY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{COMPANY_ID}` | 検索する会社の`id`値。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

成功応答は、会社の詳細を返します。

```json
{
  "data": {
    "id": "COdb0cd64ad4524440be94b8496416ec7d",
    "type": "companies",
    "attributes": {
      "created_at": "2020-08-13T17:13:30.667Z",
      "name": "Example Company",
      "org_id": "{IMS_ORG}",
      "updated_at": "2020-08-13T17:13:30.667Z",
      "token": "d5a4f682bbae",
      "cjm_enabled": false,
      "edge_enabled": false,
      "edge_events_allotment": null,
      "edge_fanout_ratio": null
    },
    "relationships": {
      "properties": {
        "links": {
          "related": "https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d/properties"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d",
      "properties": "https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d/properties"
    },
    "meta": {
      "rights": [
        "develop_extensions",
        "manage_properties"
      ],
      "platform_rights": {
        "web": [
          "develop_extensions",
          "manage_properties"
        ],
        "mobile": [
          "develop_extensions",
          "manage_properties"
        ]
      }
    }
  }
}
```
