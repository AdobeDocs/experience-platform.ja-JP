---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 和集合
topic: developer guide
translation-type: tm+mt
source-git-commit: 7f61cee8fb5160d0f393f8392b4ce2462d602981

---


# 和集合

和集合(または和集合表示)は、同じクラス(XDM ExperienceEventまたはXDM Individualプロファイル)を共有し、リアルタイム顧客プロファイルが有効になっているすべてのスキーマのフィールドを集計する、システム生成の読み取り専用のスキーマです [](../../profile/home.md)。

このドキュメントでは、様々な操作のサンプル呼び出しを含む、和集合レジストリAPIでスキーマを操作するための基本的な概念について説明します。 XDMの和集合に関する一般的な情報については、和集合構成の基本の基本スキーマに関する節 [を参照してください](../schema/composition.md#union)。

## 和集合ミックスイン

スキーマレジストリには、次の3つのミックスインが自動的に和集合スキーマ内に含まれます。 `identityMap`、、 `timeSeriesEvents`および `segmentMembership`。

### IDマップ

和集合スキーマ `identityMap` は、和集合の関連レコードスキーマ内の既知のIDを表します。 IDマップは、IDを、IDでキー設定された異なる配列に名前空間します。 リストに表示される各IDは、それ自体が一意の値を含むオブジェクト `id` です。

See the [Identity Service documentation](../../identity-service/home.md) for more information.

### 時系列イベント

この配 `timeSeriesEvents` 列は、その和集合に関連付けられたレコードスキーマに関連付けられた時系列イベントのリストです。 プロファイルデータをデータセットにエクスポートする場合、この配列は各レコードに含まれます。 これは、機械学習など、モデルがレコード属性に加えてプロファイルの行動履歴全体を必要とする様々な使用例に役立ちます。

### セグメントのメンバーシップマップ

マップに `segmentMembership` は、セグメント評価の結果が格納されます。 セグメント化APIを使用してセグメントジョブが正 [常に実行されると](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml)、マップが更新されます。 `segmentMembership` また、評価済みのオーディエンスセグメントをPlatformに取り込んで保存し、Adobe Platform Managerなどの他のソリューションと統合できるようにします。

詳しくは、APIを使用したセグメ [ントの作成に関するチュートリアル](../../segmentation/tutorials/create-a-segment.md) を参照してください。

## メンバーシップのスキーマを有効にする

結合された和集合表示にスキーマを含めるには、和集合の属性に「スキーマ」タグを追加する `meta:immutableTags` 必要があります。 これは、PATCHリクエストを通じて行われ、スキーマを更新し、「 `meta:immutableTags` 和集合」の値を持つアレイを追加します。

>[!NOTE] 不変タグは、設定を意図したが削除されなかったタグです。

**API形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | プロファイルでの使用を有効にする `$id` URLエン `meta:altId` コードされたURIまたはスキーマのURLです。 |

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

成功した応答は、更新されたスキーマの詳細を返します。この中には、文字列値「 `meta:immutableTags` 和集合」を含む配列が含まれています。

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

スキーマに「和集合」タグを設定すると、スキーマレジストリはスキーマの基となるクラスの和集合を自動的に作成し、維持します。 この `$id` 和集合のは、クラスの標準 `$id` に似ていますが、異なるのは2つのアンダースコアと「和集合」(`"__union"`)という単語が付加されるだけです。

使用可能な和集合のリストを表示するには、エンドポイントに対してGETリクエストを実行 `/unions` します。

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

成功した応答は、HTTPステータス200(OK)と、応答本 `results` 体内の配列を返します。 和集合が定義されている場合、各 `title`和集合の、、、およ `$id``meta:altId``version` びは、配列内のオブジェクトとして提供されます。 和集合が定義されていない場合、HTTPステータス200(OK)は引き続き返されますが、配 `results` 列は空になります。

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

## 特定の和集合

特定の和集合を表示するには、Acceptヘッダーに応じて、およびの詳細の一 `$id` 部またはすべてを含むGETリクエストを実行します。

>[!NOTE] 和集合ルックアップは、エンドポイントとエ `/unions` ンドポイントを `/schemas` 使用して使用でき、データセットへのプロファイルエクスポートで使用できます。

**API形式**

```http
GET /tenant/unions/{UNION_ID}
GET /tenant/schemas/{UNION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{UNION_ID}` | 参照する和集合のURLエン `$id` コードされたURIです。 和集合スキーマのURIには「__和集合」が追加されます。 |

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

和集合参照要求は、Acceptヘッ `version` ダーに含める必要があります。

次のAcceptヘッダーを使用して、和集合スキーマの検索を行います。

| 承認 | 説明 |
| -------|------------ |
| application/vnd.adobe.xed+json;version={MAJOR_VERSION} | ANDを使 `$ref` 用し `allOf`た。 タイトルと説明が含まれます。 |
| application/vnd.adobe.xed-full+json;version={MAJOR_VERSION} | `$ref` 属性と解決さ `allOf` れます。 タイトルと説明が含まれます。 |

**応答**

成功した応答は、リクエストパスで指定されたクラスを実装するすべての和集合の `$id` スキーマ表示を返します。

応答の形式は、リクエストで送信されるAcceptヘッダーに依存します。 異なる受け入れヘッダーを試して、回答を比較し、使用事例に最適なヘッダーを判断します。

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

## リストスキーマの和集合

特定の和集合に含まれるスキーマを確認するには、クエリパラメーターを使用してGET要求を実行し、テナントコンテナ内のスキーマをフィルタリングします。

クエリパラメ `property` ーターを使用して、アクセスするスキーマのクラスに等しいフィールドとク `meta:immutableTags` ラスを含 `meta:class` む和集合のみを返すように応答を設定できます。

**API形式**

```http
GET /tenant/schemas?property=meta:immutableTags==union&property=meta:class=={CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | アクセ `$id` スする和集合のクラス。 |

**リクエスト**

次のリクエストは、XDM個々のスキーマクラスの一部であるすべてのプロファイルを検索します。和集合

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

成功した応答は、フィルタされたリストのスキーマを返します。この中には、両方の要件を満たすもののみが含まれます。 複数のパラメーターを使用する場合は、ANDクエリが想定されることに注意してください。 応答の形式は、リクエストで送信されるAcceptヘッダーによって異なります。

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
