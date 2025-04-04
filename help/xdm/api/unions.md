---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマレジストリ；和集合；和集合；segmentMembership;timeSeriesEvents;
solution: Experience Platform
title: 結合 API エンドポイント
description: スキーマレジストリ API の/unions エンドポイントを使用すると、エクスペリエンスアプリケーションの XDM 結合スキーマをプログラムで管理できます。
exl-id: d0ece235-72e8-49d9-856b-5dba44e16ee7
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '900'
ht-degree: 39%

---

# 結合エンドポイント

和集合（または和集合ビュー）は、同じクラス（[!DNL XDM ExperienceEvent] または [!DNL XDM Individual Profile]）を共有し、ス [[!DNL Real-Time Customer Profile]](../../profile/home.md) ーマが有効になっているすべてのスキーマのフィールドを集計する、システム生成の読み取り専用スキーマです。

このドキュメントでは、Schema Registry API で和集合を操作するための基本的な概念と、様々な操作のサンプル呼び出しを示しています。XDM の和集合に関する一般的な情報については、[スキーマ構成の基本](../schema/composition.md#union)の和集合に関する節を参照してください。

## 結合スキーマフィールド

[!DNL Schema Registry] には、結合スキーマ内に、`identityMap`、`timeSeriesEvents`、`segmentMembership` の 3 つのキーフィールドが自動的に含まれます。

### ID マップ

和集合スキーマの `identityMap` は、和集合に関連付けられているレコードスキーマ内の既知の ID を表します。ID マップは、名前空間で特定された異なる配列に ID を分離します。リストされた各 ID は、それ自体が一意の `id` 値を含むオブジェクトです。 詳しくは、[ID サービスに関するドキュメント](../../identity-service/home.md)を参照してください。

### 時系列イベント

`timeSeriesEvents` 配列は、和集合に関連付けられているレコードスキーマに関連する時系列イベントのリストです。プロファイルデータをデータセットに書き出すと、この配列が各レコードに含まれます。 これは、様々な使用事例に役立ちます。例えば、モデルがレコード属性に加えてプロファイルの行動履歴全体を必要とする機械学習などに役立ちます。

### セグメントメンバーシップマップ

`segmentMembership` マップには、セグメント定義の評価結果が格納されます。 [Segmentation API](https://www.adobe.io/experience-platform-apis/references/segmentation/) を使用してセグメントジョブが正常に実行された場合、マップが更新されます。`segmentMembership` た、Experience Platformに取り込まれた事前評価済みオーディエンスを格納して、Adobe Audience Managerなどの他のソリューションと統合できます。 詳しくは、[API を使用したオーディエンスの作成 ](../../segmentation/tutorials/create-a-segment.md) に関するチュートリアルを参照してください。

## 和集合のリストの取得 {#list}

スキーマに `union` タグを設定すると、スキーマの基 [!DNL Schema Registry] となっているクラスの和集合に、スキーマが自動的に追加されます。 該当するクラスに和集合が存在しない場合は、新しい和集合が自動的に作成されます。 和集合の `$id` は、他の [!DNL Schema Registry] リソースの標準 `$id` と似ていますが、唯一の違いは、2 つのアンダースコアと「union」（`__union`）という単語が付加されていることです。

`/tenant/unions` エンドポイントに対してGET リクエストをおこなうと、使用可能な結合のリストを表示できます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-id+json'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 和集合のリストには、次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースをリストする場合は、このヘッダーをお勧めします。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON クラスを、元の `$ref` と `allOf` を含めて返します。 （上限：300） |

{style="table-layout:auto"}

**応答**

リクエストが成功した場合は、HTTP ステータス 200（OK）、およびレスポンス本文に `results` 配列が返されます。結合が定義されている場合、各結合の詳細は配列内のオブジェクトとして提供されます。 和集合が定義されていない場合も、HTTP ステータス 200（OK）が返されますが、`results` 配列は空になります。

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

## 結合の検索 {#lookup}

特定の和集合を表示するには、`$id` と、和集合の詳細の一部またはすべて（Accept ヘッダーにより異なる）を含む GET リクエストを実行します。

>[!NOTE]
>
>和集合検索は `/unions` エンドポイントと `/schemas` エンドポイントを使用して使用でき、データセットへの [!DNL Profile] 書き出しで使用できます。

**API 形式**

```http
GET /tenant/unions/{UNION_ID}
GET /tenant/schemas/{UNION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{UNION_ID}` | 検索する和集合の URL エンコードされた `$id` URI。 和集合スキーマの URI には「__union」が追加されます。 |

{style="table-layout:auto"}

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/unions/https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile__union \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed+json; version=1'
```

和集合の検索リクエストでは、Accept ヘッダーに `version` を含める必要があります。

和集合スキーマの検索では、次の Accept ヘッダーを使用できます。

| Accept | 説明 |
| -------|------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` と `allOf` を含む未処理の和集合。タイトルと説明が含まれます。 |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` 属性と `allOf` が解決されます。タイトルと説明が含まれます。 |

{style="table-layout:auto"}

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

## 和集合メンバーシップのスキーマを有効にする {#enable}

スキーマをそのクラスの和集合に含めるには、`union` タグをスキーマの `meta:immutableTags` 属性に追加する必要があります。 これを実現するには、`union` という単一の文字列値を持つ `meta:immutableTags` 配列を、当該のスキーマに追加するPATCH リクエストを行います。 詳しい例については、[ スキーマエンドポイントガイド ](./schemas.md#union) を参照してください。

## 和集合でのスキーマのリスト {#list-schemas}

特定の結合の一部であるスキーマを確認するには、`/tenant/schemas` エンドポイントに対してGET リクエストを実行します。 `property` クエリパラメーターを使用すると、アクセス先の和集合があるクラスと同等の `meta:immutableTags` フィールドおよび `meta:class` を含むスキーマのみを返すようにレスポンスを設定できます。

**API 形式**

```http
GET /tenant/schemas?property=meta:immutableTags==union&property=meta:class=={CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 和集合対応スキーマのリストを取得するクラスの `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、[!DNL XDM Individual Profile] クラスの和集合に含まれるすべてのスキーマのリストを取得します。

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?property=meta:immutableTags==union&property=meta:class==https://ns.adobe.com/xdm/context/profile' \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 スキーマの一覧表示には、次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースをリストする場合は、このヘッダーをお勧めします。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON スキーマを返します。元の `$ref` と `allOf` が含まれます。 （上限：300） |

{style="table-layout:auto"}

**応答**

応答が成功すると、フィルターされたスキーマのリストが返されます。このリストには、和集合メンバーシップが有効になっている指定されたクラスに属するスキーマのみが含まれます。 複数のクエリパラメーターを使用する場合、AND 関係が想定されることに注意してください。

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
