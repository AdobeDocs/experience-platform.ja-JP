---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 和集合
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 1%

---


# 和集合

和集合(または和集合表示)は、同じクラス(XDM ExperienceEventまたはXDM Individualプロファイル)を共有し、 [リアルタイム顧客プロファイルが有効になっているすべてのスキーマのフィールドを集計する、システム生成の読み取り専用スキーマ](../../profile/home.md)です。

このドキュメントでは、様々な操作のサンプル呼び出しを含む、スキーマレジストリAPIの和集合を操作するための基本的な概念について説明します。 XDMの和集合に関する一般的な情報については、スキーマ構成の [基本の和集合に関する節を参照してください](../schema/composition.md#union)。

## 和集合ミックスイン

スキーマレジストリは、和集合スキーマ内に3つのミックスインを自動的に含めます。 `identityMap`、 `timeSeriesEvents`および `segmentMembership`。

### IDマップ

和集合スキーマ `identityMap` とは、和集合の関連レコードスキーマ内の既知のIDを表します。 IDマップは、IDを名前空間でキー設定された異なるアレイに分割します。 リストに表示される各IDは、それ自体に一意の `id` 値を含むオブジェクトです。

See the [Identity Service documentation](../../identity-service/home.md) for more information.

### 時系列イベント

この `timeSeriesEvents` 配列は、和集合に関連付けられているレコードスキーマに関連する時系列イベントのリストです。 プロファイルデータをデータセットにエクスポートする場合、この配列は各レコードに含まれます。 これは、機械学習など、モデルが記録属性に加えてプロファイルの行動履歴全体を必要とする様々な使用例に役立ちます。

### セグメントのメンバーシップマップ

この `segmentMembership` マップには、セグメントの評価結果が格納されます。 セグメント化APIを使用してセグメントジョブが正常に実行されると [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)、マップが更新されます。 `segmentMembership` また、評価済みのオーディエンスセグメントをPlatformに取り込んで保存し、Adobe Audience Managerなどの他のソリューションと統合することもできます。

詳しくは、APIを使用したセグメントの [作成に関するチュートリアルを参照してください](../../segmentation/tutorials/create-a-segment.md) 。

## スキーマの和集合メンバーシップの有効化

結合された和集合表示にスキーマを含めるには、スキーマの `meta:immutableTags` 属性に「和集合」タグを追加する必要があります。 これは、PATCHリクエストを通じて行われ、スキーマを更新し、「和集合」の値を持つ `meta:immutableTags` 配列を追加します。

>[!NOTE]
>
>不変タグとは、設定を意図したが削除されなかったタグです。

**API形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | プロファイルでの使用に有効にする、URLエンコードされた `$id` URI `meta:altId` またはスキーマのURI。 |

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

正常に応答すると、更新されたスキーマの詳細が返されます。この詳細には、文字列値「和集合」を含む `meta:immutableTags` 配列が含まれます。

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

## リスト和集合

スキーマに「和集合」タグを設定すると、スキーマレジストリによって、スキーマの基となるクラスの和集合が自動的に作成され、維持されます。 この和集合 `$id` の値は、クラスの標準 `$id` に似ていますが、2つのアンダースコアと「和集合」(`"__union"`)という単語が付加されるのが唯一の違いです。

使用可能な和集合のリストを表示するには、エンドポイントに対してGETリクエストを実行でき `/unions` ます。

**API形式**

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

成功した応答は、HTTPステータス200(OK)と `results` 配列を応答本文に返します。 和集合を定義した場合、各和集合の、、、 `title`、およびは、配列内 `$id``meta:altId``version` のオブジェクトとして提供されます。 和集合が定義されていない場合、HTTPステータス200 (OK)は引き続き返されますが、 `results` 配列は空になります。

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

特定の和集合を表示するには、Acceptヘッダーに応じて、和集合の一部またはすべての詳細を含むGET要求 `$id` を実行します。

>[!NOTE]
>
>和集合検索は、エンドポイント `/unions``/schemas` とエンドポイントを使用して使用でき、プロファイルをデータセットにエクスポートする際に使用できます。

**API形式**

```http
GET /tenant/unions/{UNION_ID}
GET /tenant/schemas/{UNION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{UNION_ID}` | 参照する和集合のURLエンコードされた `$id` URI。 和集合スキーマのURIには「__和集合」が付加されます。 |

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

和集合参照の要求では、Acceptヘッダーに含める必要 `version` があります。

和集合スキーマの検索には、次のAcceptヘッダーを使用できます。

| 同意 | 説明 |
| -------|------------ |
| application/vnd.adobe.xed+json; version={MAJOR_VERSION} | ANDを使用し `$ref` たRAW `allOf`。 タイトルと説明が含まれます。 |
| application/vnd.adobe.xed-full+json; version={MAJOR_VERSION} | `$ref` 属性を `allOf` 解決します。 タイトルと説明が含まれます。 |

**応答**

成功した応答は、要求パスで指定されたクラスを実装するすべてのスキーマの和集合表示 `$id` を返します。

応答の形式は、要求で送信されるAcceptヘッダーによって異なります。 様々なAcceptヘッダーを使用してテストし、応答を比較し、使用事例に最適なヘッダーを判断します。

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

## 和集合内のリストスキーマ

特定の和集合に属するスキーマを確認するには、クエリパラメーターを使用してGET要求を実行し、テナントコンテナ内のスキーマをフィルタリングします。

クエリパラメーターを使用して、スキーマにアクセスしている和集合と同じフィールドと `property` 等しいフィールドを含むのみを返すよう `meta:immutableTags``meta:class` に応答を設定できます。

**API形式**

```http
GET /tenant/schemas?property=meta:immutableTags==union&property=meta:class=={CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 和集合 `$id` にアクセスするクラスの名前。 |

**リクエスト**

次の要求は、XDM Individualプロファイルクラス和集合に含まれるすべてのスキーマを調べます。

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

成功した応答は、フィルターされたリストのスキーマを返します。この中には、両方の要件を満たす要素のみが含まれます。 複数のクエリパラメーターを使用する場合は、AND関係が想定されます。 応答の形式は、要求で送信されるAcceptヘッダーによって異なります。

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
