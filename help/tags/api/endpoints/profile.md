---
title: プロファイルエンドポイント
description: Reactor API で /profiles エンドポイントを呼び出す方法を説明します。
source-git-commit: 59592154eeb8592fa171b5488ecb0385e0e59f39
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 100%

---

# プロファイルエンドポイント

Reactor API では、プロファイルは Adobe Experience Platform ユーザーを表します。Reactor API は、ユーザーと権限の独自のデータベースを維持せず、代わりに[アドビの ID 管理システム（IMS）](https://helpx.adobe.com/jp/enterprise/using/identity.html)によって管理される Adobe ID に依存します。

プロファイルには、ログインしたユーザーに関するすべての情報（所属するすべての IMS 組織、各組織内に属する製品プロファイル、各製品プロファイルからの権限など）が含まれます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml) の一部です。続行する前に、API への認証方法に関する重要な情報について、[はじめる前に](../getting-started.md)を確認してください。

## 現在のプロファイルの取得 {#lookup}

`/profile` エンドポイントに GET リクエストを送信することで、現在ログインしているプロファイルの詳細を取得できます。

**API 形式**

```http
GET /profile
```

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、プロファイルの詳細が返されます。

```json
{
  "data": {
    "id": "UR0bd696624e844d6ba5bfc248ba1eca11",
    "type": "users",
    "attributes": {
      "active_org": "{IMS_ORG_1}",
      "expires_in": 0,
      "display_name": "John Smith",
      "job_function": null,
      "email": "jsmith@example.com",
      "organizations": {
        "{IMS_ORG_1}": {
          "name": "Example IMS Org A",
          "admin": true,
          "active": true,
          "login_companies": [

          ],
          "product_contexts": [
            "dma_audiencemanager_int",
            "dma_tartan",
            "dma_dtm",
            "dma_reactor",
            "dma_auditor"
          ],
          "tenant_id": "{TENANT_ID_1}"
        },
        "{IMS_ORG_2}": {
          "name": "Example IMS Org B",
          "admin": false,
          "active": false,
          "login_companies": [

          ],
          "product_contexts": [
            "dma_reactor",
            "dma_auditor",
            "dma_tartan"
          ],
          "tenant_id": "{TENANT_ID_2}"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/profile"
    },
    "meta": {
      "rights": [
        "manage_companies"
      ]
    }
  }
}
```

