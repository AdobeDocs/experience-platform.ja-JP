---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；和集合；和集合；Union;SegmentMembership;timeSeriesEvents;
solution: Experience Platform
title: 和集合 API エンドポイント
description: Schema Registry API の/unions エンドポイントを使用すると、エクスペリエンスアプリケーションで XDM 和集合スキーマをプログラムで管理できます。
exl-id: d0ece235-72e8-49d9-856b-5dba44e16ee7
source-git-commit: 3da2e8f66f08a7bb9533795f7854ad583734911c
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 42%

---

# 和集合エンドポイント

和集合（または和集合表示）は、同じクラス ([!DNL XDM ExperienceEvent] または [!DNL XDM Individual Profile]) とが有効で、 [[!DNL Real-Time Customer Profile]](../../profile/home.md).

このドキュメントでは、Schema Registry API で和集合を操作するための基本的な概念と、様々な操作のサンプル呼び出しを示しています。XDM の和集合に関する一般的な情報については、「[Basics of schema composition](../schema/composition.md#union)」の和集合に関する節を参照してください。

## スキーマフィールドを結合

The [!DNL Schema Registry] 和集合スキーマ内に 3 つのキーフィールドを自動的に含めます。 `identityMap`, `timeSeriesEvents`、および `segmentMembership`.

### ID マップ

和集合スキーマの `identityMap` は、和集合に関連付けられているレコードスキーマ内の既知の ID を表します。ID マップは、名前空間で特定された異なる配列に ID を分離します。リストされる ID 自体は、一意の `id` 値を含むオブジェクトです。詳しくは、[ID サービスに関するドキュメント](../../identity-service/home.md)を参照してください。

### 時系列イベント

`timeSeriesEvents` 配列は、和集合に関連付けられているレコードスキーマに関連する時系列イベントのリストです。プロファイルデータをデータセットに書き出す場合、レコードごとにこの配列が含まれます。 これは、様々な使用事例に役立ちます。例えば、モデルがレコード属性に加えてプロファイルの行動履歴全体を必要とする機械学習などに役立ちます。

### セグメントメンバーシップマップ

The `segmentMembership` map は、セグメント定義の評価結果を格納します。 [Segmentation API](https://www.adobe.io/experience-platform-apis/references/segmentation/) を使用してセグメントジョブが正常に実行された場合、マップが更新されます。`segmentMembership` また、には、Platform に取り込まれた評価済みのオーディエンスも格納されます。これにより、Adobe Audience Managerなどの他のソリューションと統合することができます。 に関するチュートリアルを参照してください。 [API を使用したオーディエンスの作成](../../segmentation/tutorials/create-a-segment.md) を参照してください。

## 和集合のリストの取得 {#list}

次の場合、 `union` タグを使用して、 [!DNL Schema Registry] は、スキーマの基となるクラスの和集合にスキーマを自動的に追加します。 問題のクラスに和集合が存在しない場合、新しい和集合が自動的に作成されます。 The `$id` の場合、和集合は標準に似ています `$id` その他 [!DNL Schema Registry] リソースで使用されます。ただし、2 つのアンダースコアと「和集合」という単語が付加されるという点が異なります (`__union`) をクリックします。

使用可能な和集合のリストを表示するには、 `/tenant/unions` endpoint.

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

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 次の `Accept` 和集合のリストには、次のヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースを一覧表示する場合は、これが推奨されるヘッダーです。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON クラスを元のと共に返します `$ref` および `allOf` 含まれる。 （上限：300） |

{style="table-layout:auto"}

**応答**

リクエストが成功した場合は、HTTP ステータス 200（OK）、およびレスポンス本文に `results` 配列が返されます。和集合が定義されている場合、各和集合の詳細は配列内にオブジェクトとして表示されます。 和集合が定義されていない場合も、HTTP ステータス 200（OK）が返されますが、`results` 配列は空になります。

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

## 和集合の検索 {#lookup}

特定の和集合を表示するには、`$id` と、和集合の詳細の一部またはすべて（Accept ヘッダーにより異なる）を含む GET リクエストを実行します。

>[!NOTE]
>
>和集合の検索は、 `/unions` および `/schemas` エンドポイントがでの使用を有効にします。 [!DNL Profile] をデータセットに書き出します。

**API 形式**

```http
GET /tenant/unions/{UNION_ID}
GET /tenant/schemas/{UNION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{UNION_ID}` | URL エンコードされた `$id` 検索する和集合の URI。 和集合スキーマの URI には「__union」が追加されます。 |

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

スキーマをそのクラスの和集合に含めるには、 `union` タグをスキーマの `meta:immutableTags` 属性。 これを実現するには、PATCHリクエストを実行し、 `meta:immutableTags` 次の単一の文字列値を持つ配列： `union` を問題のスキーマに追加します。 詳しくは、 [スキーマエンドポイントガイド](./schemas.md#union) 詳しい例を示します。

## 和集合でのスキーマのリスト {#list-schemas}

特定の和集合に含まれるスキーマを確認するには、 `/tenant/schemas` endpoint. `property` クエリパラメーターを使用すると、アクセス先の和集合があるクラスと同等の `meta:immutableTags` フィールドおよび `meta:class` を含むスキーマのみを返すようにレスポンスを設定できます。

**API 形式**

```http
GET /tenant/schemas?property=meta:immutableTags==union&property=meta:class=={CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | The `$id` のスキーマを設定します。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、 [!DNL XDM Individual Profile] クラス。

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?property=meta:immutableTags==union&property=meta:class==https://ns.adobe.com/xdm/context/profile' \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 次の `Accept` ヘッダーを使用してスキーマをリストできます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースを一覧表示する場合は、これが推奨されるヘッダーです。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON スキーマを元のと共に返します `$ref` および `allOf` 含まれる。 （上限：300） |

{style="table-layout:auto"}

**応答**

正常な応答は、フィルターされたスキーマのリストを返します。この中には、和集合のメンバーシップが有効になっている、指定されたクラスに属するスキーマのみが含まれます。 複数のクエリーパラメーターを使用する場合は、AND が想定されることに注意してください。

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
