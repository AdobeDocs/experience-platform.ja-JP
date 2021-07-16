---
title: プロファイルエンドポイント
description: Reactor APIで/profilesエンドポイントを呼び出す方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 6%

---

# プロファイルエンドポイント

Reactor APIでは、プロファイルはAdobe Experience Platformユーザーを表します。 Reactor APIは、ユーザーと権限の独自のAdobeを維持するのではなく、[AdobeのID管理システム(IMS)](https://helpx.adobe.com/jp/enterprise/using/identity.html)によって管理されるデータベースIDに依存します。

プロファイルには、ログインしたユーザーに関するすべての情報（所属するすべてのIMS組織、各組織内に属する製品プロファイル、各製品プロファイルからの権限など）が含まれます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml)の一部です。 続行する前に、APIへの認証方法に関する重要な情報について、[はじめにのガイド](../getting-started.md)を参照してください。

## 現在のプロファイルの取得 {#lookup}

`/profile`エンドポイントにGETリクエストを送信することで、現在ログインしているプロファイルの詳細を取得できます。

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
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、プロファイルの詳細を返します。

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

