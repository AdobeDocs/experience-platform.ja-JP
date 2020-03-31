---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマ
topic: developer guide
translation-type: tm+mt
source-git-commit: 162316c3b908ffa87d8df4dff72e26ba237993db

---


# スキーマ

スキーマは、エクスペリエンスプラットフォームに取り込むデータの青写真と見なすことができます。 各スキーマは、クラスと0個以上のミックスインで構成されます。 つまり、スキーマを定義するためにミックスインを追加する必要はありませんが、ほとんどの場合、少なくとも1つのミックスインが使用されます。

スキーマ構成プロセスは、クラスの割り当てから開始されます。 このクラスは、データ（レコードまたは時系列）の主要な動作要素と、取り込まれるデータを記述するために必要な最小フィールドを定義します。

**API形式**

```http
POST /tenant/schemas
```

**リクエスト**

リクエストには、クラスのを参 `allOf` 照する属性を含める `$id` 必要があります。 この属性は、スキーマが実装する「基本クラス」を定義します。 この例では、基本クラスは、以前に作成された「プロパティ情報」クラスです。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property Information",
        "description": "Property-related information.",
        "type": "object",
        "allOf": [ 
          { 
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590" 
          } 
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `allOf > $ref` | 新し `$id` い実装のクラスのスキーマ値。 |

**応答**

正常な応答は、HTTPステータス201（作成済み）と、新しく作成されたスキーマの詳細（、、など）を含むペイロ `$id`ードを `meta:altId`返しま `version`す。 これらの値は読み取り専用で、割り当てはスキーマレジストリです。

```JSON
{
    "title": "Property Information",
    "description": "Property-related information.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
        "https://ns.adobe.com/xdm/data/record"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.0",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552088461236,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

リストに対してGET要求を実行すると、テナントコンテナ内のすべてのスキーマにProperty Informationスキーマが含まれるようになりました。また、URLエンコードされた `$id` URIを使用して参照(GET)要求を実行し、新しいスキーマを直接表示できます。 すべての参照リクエストの「受 `version` け入れ」ヘッダーにを必ず含めてください。
