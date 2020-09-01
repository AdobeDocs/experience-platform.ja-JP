---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;union;Union;unions;Unions;segmentMembership;timeSeriesEvents;
solution: Experience Platform
title: 和集合
description: 和集合 (または和集合表示)は、同じクラス(XDM ExperienceEventまたはXDM Individualプロファイル)を共有し、リアルタイム顧客プロファイルが有効になっているすべてのスキーマのフィールドを集計する、システム生成の読み取り専用スキーマです。
topic: developer guide
translation-type: tm+mt
source-git-commit: 74a4a3cc713cc068be30379e8ee11572f8bb0c63
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 81%

---


# 和集合

和集合 (or union views) are system-generated, read-only schemas that aggregate the fields of all schemas which share the same class ([!DNL XDM ExperienceEvent] or [!DNL XDM Individual Profile]) and are enabled for [[!DNL Real-time Customer Profile]](../../profile/home.md).

このドキュメントでは、Schema Registry API で和集合を操作するための基本的な概念と、様々な操作のサンプル呼び出しを示しています。XDM の和集合に関する一般的な情報については、「[Basics of schema composition](../schema/composition.md#union)」の和集合に関する節を参照してください。

## 和集合 Mixin

The [!DNL Schema Registry] automatically includes three mixins within the union schema: `identityMap`, `timeSeriesEvents`, and `segmentMembership`.

### ID マップ

和集合スキーマの `identityMap` は、和集合に関連付けられているレコードスキーマ内の既知の ID を表します。ID マップは、名前空間で特定された異なる配列に ID を分離します。リストされる ID 自体は、一意の `id` 値を含むオブジェクトです。

詳しくは、[ID サービスに関するドキュメント](../../identity-service/home.md)を参照してください。

### 時系列イベント

`timeSeriesEvents` 配列は、和集合に関連付けられているレコードスキーマに関連する時系列イベントのリストです。When [!DNL Profile] data is exported to datasets, this array is included for each record. これは、様々な使用事例に役立ちます。例えば、モデルがレコード属性に加えてプロファイルの行動履歴全体を必要とする機械学習などに役立ちます。

### セグメントメンバーシップマップ

`segmentMembership` マップには、セグメント評価の結果が格納されます。[Segmentation API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml) を使用してセグメントジョブが正常に実行された場合、マップが更新されます。また、`segmentMembership` には、Platform に取り込まれた評価済みのオーディエンスセグメントも格納されます。このため、Adobe Audience Manager などの他のソリューションと統合することができます。

詳しくは、[API を使用したセグメ ントの作成](../../segmentation/tutorials/create-a-segment.md)に関するチュートリアルを参照してください。

## 和集合メンバーシップのスキーマを有効にする

結合された和集合表示にスキーマを含めるには、スキーマの `meta:immutableTags` 属性に「union」タグを追加する必要があります。これは、PATCH リクエストを通じてスキーマを更新し、「和集合」の値を持つ `meta:immutableTags` 配列を追加することによっておこなわれます。

>[!NOTE]
>
>Immutable タグは、設定を目的としたもので、削除には使用できません。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | The URL-encoded `$id` URI or `meta:altId` of the schema you want to enable for use in [!DNL Profile]. |

**リクエスト**

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        { "op": "add", "path": "/meta:immutableTags", "value": ["union"]}
      ]'
```

**応答**

リクエストが成功した場合は、更新されたスキーマの詳細が返されます。この中には、文字列値「union」を含む `meta:immutableTags` 配列があります。

```JSON
{
    "title": "Property Information",
    "description": "Property-related information.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:immutableTags": [
        "union"
    ],
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.2",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552091263267,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

## 和集合をリストする

When you set the &quot;union&quot; tag on a schema, the [!DNL Schema Registry] automatically creates and maintains a union for the class upon which the schema is based. 和集合の `$id` は、クラスの標準 `$id` に似ていますが、2 個のアンダースコアと単語「union」（`"__union"`）が付加されるという点が異なります。

使用可能な和集合のリストを表示するには、`/unions` エンドポイントに GET リクエストを実行します。

**API 形式**

```http
GET /tenant/unions
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/unions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-id+json'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 200（OK）、およびレスポンス本文に `results` 配列が返されます。和集合が定義されている場合、各和集合の `title`、`$id`、`meta:altId` および `version` が、配列内にオブジェクトとして表示されます。和集合が定義されていない場合も、HTTP ステータス 200（OK）が返されますが、`results` 配列は空になります。

```JSON
{
    "results": [
        {
            "title": "XDM Individual Profile",
            "$id": "https://ns.adobe.com/xdm/context/profile__union",
            "meta:altId": "_xdm.context.profile__union",
            "version": "1"
        },
        {
            "title": "Property",
            "$id": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590__union",
            "meta:altId": "_{TENANT_ID}.classes.19e1d8b5098a7a76e2c10a81cbc99590__union",
            "version": "1"
        }
    ]
}
```

## 特定の和集合の検索

特定の和集合を表示するには、`$id` と、和集合の詳細の一部またはすべて（Accept ヘッダーにより異なる）を含む GET リクエストを実行します。

>[!NOTE]
>
>Union lookups are available using the `/unions` and `/schemas` endpoint to enable them for use in [!DNL Profile] exports into a dataset.

**API 形式**

```http
GET /tenant/unions/{UNION_ID}
GET /tenant/schemas/{UNION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{UNION_ID}` | 検索する和集合の URL エンコードされた `$id` URI。和集合スキーマの URI には「__union」が追加されます。 |

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/unions/https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile__union \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed+json; version=1'
```

和集合の検索リクエストでは、Accept ヘッダーに `version` を含める必要があります。

和集合スキーマの検索では、次の Accept ヘッダーを使用できます。

| Accept | 説明 |
| -------|------------ |
| application/vnd.adobe.xed+json; version={MAJOR_VERSION} | `$ref` と `allOf` を含む未処理の和集合。タイトルと説明が含まれます。 |
| application/vnd.adobe.xed-full+json; version={MAJOR_VERSION} | `$ref` 属性と `allOf` が解決されます。タイトルと説明が含まれます。 |

**応答**

リクエストが成功した場合は、リクエストパスで指定した `$id` のクラスを実装するすべてのスキーマの和集合表示が返されます。

レスポンスの形式は、リクエストで送信した Accept ヘッダーにより異なります。異なる Accept ヘッダーを試して、応答を比較し、使用事例に最適なヘッダーを判断します。

```JSON
{
    "type": "object",
    "description": "Union view of all schemas that extend https://ns.adobe.com/xdm/context/profile",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/477bb01d7125b015b4feba7bccc2e599"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details"
        }
    ],
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/common/extensible",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/{TENANT_ID}/mixins/477bb01d7125b015b4feba7bccc2e599",
        "https://ns.adobe.com/xdm/context/profile-personal-details"
    ],
    "title": "Union object for https://ns.adobe.com/xdm/context/profile",
    "$id": "https://ns.adobe.com/xdm/context/profile__union",
    "meta:containerId": "tenant",
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:altId": "_xdm.context.profile__union",
    "version": "1.0",
    "meta:resourceType": "unions",
    "meta:registryMetadata": {}
}
```

## 和集合内のスキーマをリストする

特定の和集合に含まれるスキーマを確認するには、クエリパラメーターを使用して GET リクエストを実行し、テナントコンテナ内のスキーマをフィルタリングします。

`property` クエリパラメーターを使用すると、アクセス先の和集合があるクラスと同等の `meta:immutableTags` フィールドおよび `meta:class` を含むスキーマのみを返すようにレスポンスを設定できます。

**API 形式**

```http
GET /tenant/schemas?property=meta:immutableTags==union&property=meta:class=={CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | アクセス先の和集合があるクラスの `$id`。 |

**リクエスト**

The following request looks up all schemas that are part of the [!DNL XDM Individual Profile] class union.

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?property=meta:immutableTags==union&property=meta:class==https://ns.adobe.com/xdm/context/profile' \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、フィルタリングされたスキーマのリストが返されます。この中には、両方の要件を満たすスキーマのみが含まれます。複数のクエリパラメーターを使用する場合は、AND 関係であると見なされることに注意してください。レスポンスの形式は、リクエストで送信した Accept ヘッダーにより異なります。

```JSON
{
    "results": [
        {
            "title": "Schema 1",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/142afb78d8b368a5ba97a6cc8fc7e033",
            "meta:altId": "_{TENANT_ID}.schemas.142afb78d8b368a5ba97a6cc8fc7e033",
            "version": "1.2"
        },
        {
            "title": "Schema 2",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/e7297a6ddfc7812ab3a7b504a1ab98da",
            "meta:altId": "_{TENANT_ID}.schemas.e7297a6ddfc7812ab3a7b504a1ab98da",
            "version": "1.5"
        },
        {
            "title": "Schema 3",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/50f960bb810e99a21737254866a477bf",
            "meta:altId": "_{TENANT_ID}.schemas.50f960bb810e99a21737254866a477bf",
            "version": "1.2"
        },
        {
            "title": "Schema 4",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/a39655ca8ea3d5c1f36a463b45fccca8",
            "meta:altId": "_{TENANT_ID}.schemas.a39655ca8ea3d5c1f36a463b45fccca8",
            "version": "1.1"
        },
        {
            "title": "Schema 5",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/c063fac45c6d6285ef33b0e2af09f633",
            "meta:altId": "_{TENANT_ID}.schemas.c063fac45c6d6285ef33b0e2af09f633",
            "version": "1.2"
        },
        {
            "title": "Schema 6",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/dfebb19b93827b70bbad006137812537",
            "meta:altId": "_{TENANT_ID}.schemas.dfebb19b93827b70bbad006137812537",
            "version": "1.7"
        }
    ],
    "_links": {
        "global_schemas": {
            "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/schemas?property=meta:immutableTags==union&property=meta:class==https://ns.adobe.com/xdm/context/profile"
        }
    }
}
```
