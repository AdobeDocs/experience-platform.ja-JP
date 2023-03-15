---
title: スキーマレジストリ API を使用したスキーマへの特定のフィールドの追加
description: スキーマレジストリ API を使用して、既存のフィールドグループから Experience Data Model（XDM）スキーマに個々のフィールドを追加する方法について説明します。
source-git-commit: 4bcd949e901c11bb933000f7ae76f17134dda496
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 100%

---

# スキーマレジストリ API を使用したスキーマへの特定のフィールドの追加

Experience Data Model（XDM）スキーマは基本クラスで構成され、アドビで定義された標準フィールドグループと、組織で定義されたカスタムフィールドグループを使用して追加のフィールドを含めます。

スキーマを作成する際に、特定のフィールドグループの一部のフィールドを使用し、必要のない同じグループの他のフィールドを除外することができます。このチュートリアルでは、スキーマレジストリ API を使用して、フィールドグループからスキーマに個々のフィールドを追加する方法について説明します。

>[!NOTE]
>
>Adobe Experience Platform UI で個々のスキーマフィールドを追加および削除する方法について詳しくは、[フィールドベースのワークフロー](../ui/field-based-workflows.md)（現在はベータ版）を参照してください。

## 前提条件

このチュートリアルでは、[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) を呼び出します。開始する前に、[開発者ガイド](../api/getting-started.md) で、`{TENANT_ID}`、コンテナの概念、リクエストを行うために必要なヘッダーなど、API に対する呼び出しを正常に行うために知っておく必要がある重要な情報を確認してください。

## `meta:refProperty` フィールドについて

特定のスキーマについて、その構造を構成するクラスおよびフィールドグループは、その `allOf` 配列の下で参照します。各コンポーネントは、コンポーネントの URI `$id` を参照する `$ref` プロパティを含むオブジェクトとして表されます。

次の JSON は、単一のクラス（`experienceevent`）とフィールドグループ（`experienceevent-all`）を使用するシンプル化されたスキーマを表しています。

```json
{
  "title": "My Sample Schema",
  "description": "My Sample Description",
  "type": "object",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
    },
    {
      "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all"
    }
  ]
}
```

フィールドグループを参照する `allOf` 配列内の任意のオブジェクトについて、兄弟の `meta:refProperty` フィールドを追加すると、スキーマに含めるグループ内のフィールドを指定できます。

>[!NOTE]
>
>各フィールドは、それぞれのフィールドグループ内のフィールドへのパスを表す JSON ポインター文字列を使用して指定します。文字列の先頭は、スラッシュ（`/`）で始める必要があります。`properties` の名前空間を含めることはできません。例：`/_experience/campaign/message/id`。

文字列として含める場合、`meta:refProperty` はグループ内の単一のフィールドを参照できます。異なる `meta:refProperty` 値を持つ別のオブジェクトで同じ `$ref` 値を使用することにより、同じグループの他のフィールドを含めることができます。

```json
{
  "title": "My Sample Schema",
  "description": "An example schema that uses the XDM ExperienceEvent class.",
  "type": "object",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
    },
    {
      "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
      "meta:refProperty": "/_experience/campaign/message/id"
    },
    {
      "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
      "meta:refProperty": "/_experience/campaign/message/size"
    }
  ]
}
```

または、`meta:refProperty` を配列として指定することもできます。これにより、単一の `allOf` リスト項目内の特定のグループから複数のフィールドを含めるように指定できます。

```json
{
  "title": "My Sample Schema",
  "description": "An example schema that uses the XDM ExperienceEvent class.",
  "type": "object",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
    },
    {
      "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
      "meta:refProperty": [
        "/_experience/campaign/message/id",
        "/_experience/campaign/message/size",
        "/_experience/campaign/message/variant"
      ]
    }
  ]
}
```

## PUT 操作を使用したフィールドの追加

PUT リクエストを使用してスキーマ全体を書き換え、`allOf` に含めるフィールドを設定できます。

**API 形式**

```http
PUT /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 書き換えるスキーマの `meta:altId` または URL エンコードされた `$id`。 |

**リクエスト**

次のリクエストでは、`allOf` 配列の下のフィールドグループに含まれる特定のフィールドを更新します。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "title": "My Sample Schema",
        "description": "My Sample Description",
        "type": "object",
        "allOf": [
          {
            "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
          },
          {
            "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
            "meta:refProperty": [
              "/_experience/campaign/message/id",
              "/_experience/campaign/message/size",
              "/_experience/campaign/message/variant"
            ]
          }
        ]
      }'
```

**応答**

応答が成功すると、更新されたスキーマの詳細が返されます。

```json
{
  "title": "My Sample Schema",
  "description": "My Sample Description",
  "type": "object",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
    },
    {
      "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
      "meta:refProperty": [
        "/_experience/campaign/message/id",
        "/_experience/campaign/message/size",
        "/_experience/campaign/message/variant"
      ]
    }
  ],
  "meta:class": "https://ns.adobe.com/xdm/context/experienceevent",
  "meta:abstract": false,
  "meta:extensible": false,
  "meta:extends": [
      "https://ns.adobe.com/xdm/context/experienceevent",
      "https://ns.adobe.com/xdm/data/time-series"
  ],
  "meta:containerId": "tenant",
  "imsOrg": "{ORG_ID}",
  "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
  "meta:xdmType": "object",
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
  "version": "1.0",
  "meta:resourceType": "schemas",
  "meta:registryMetadata": {
      "repo:createDate": 1552088461236,
      "repo:lastModifiedDate": 1552088470592,
      "xdm:createdClientId": "{CREATED_CLIENT}",
      "xdm:repositoryCreatedBy": "{CREATED_BY}"
  }
}
```

>[!NOTE]
>
>スキーマの PUT リクエストについて詳しくは、[スキーマエンドポイントガイド](../api/schemas.md#put)を参照してください。

## PATCH 操作を使用したフィールドの追加

PATCH リクエストを使用して、他のフィールドを上書きせずに個々のフィールドをスキーマに追加できます。スキーマレジストリは、`add`、`remove`、`replace` など、標準の JSON パッチ操作をすべてサポートしています。JSON パッチについて詳しくは、[API の基本ガイド](../../landing/api-fundamentals.md#json-patch)を参照してください。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 書き換えるスキーマの `meta:altId` または URL エンコードされた `$id`。 |

**リクエスト**

次のリクエストでは、追加するフィールドを指定して、スキーマの `allOf` 配列に新しいオブジェクトを追加します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
        {
          "op": "add",
          "path": "/allOf/-",
          "value":  {
            "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
            "meta:refProperty": [
              "/_experience/campaign/message/id",
              "/_experience/campaign/message/size",
              "/_experience/campaign/message/variant"
            ]
          }
        }
      ]'
```

**応答**

応答が成功すると、更新されたスキーマの詳細が返されます。

```json
{
  "title": "My Sample Schema",
  "description": "My Sample Description",
  "type": "object",
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/context/experienceevent"
    },
    {
      "$ref": "https://ns.adobe.com/experience/campaign/experienceevent-all",
      "meta:refProperty": [
        "/_experience/campaign/message/id",
        "/_experience/campaign/message/size",
        "/_experience/campaign/message/variant"
      ]
    }
  ],
  "meta:class": "https://ns.adobe.com/xdm/context/experienceevent",
  "meta:abstract": false,
  "meta:extensible": false,
  "meta:extends": [
      "https://ns.adobe.com/xdm/context/experienceevent",
      "https://ns.adobe.com/xdm/data/time-series"
  ],
  "meta:containerId": "tenant",
  "imsOrg": "{ORG_ID}",
  "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
  "meta:xdmType": "object",
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
  "version": "1.0",
  "meta:resourceType": "schemas",
  "meta:registryMetadata": {
      "repo:createDate": 1552088461236,
      "repo:lastModifiedDate": 1552088470592,
      "xdm:createdClientId": "{CREATED_CLIENT}",
      "xdm:repositoryCreatedBy": "{CREATED_BY}"
  }
}
```

>[!NOTE]
>
>スキーマの PATCH リクエストについて詳しくは、[スキーマエンドポイントガイド](../api/schemas.md#patch)を参照してください。

## 次の手順

このガイドでは、API 呼び出しを使用して既存のフィールドグループから個々のフィールドをスキーマに追加する方法について説明しました。Platform UI で同様のフィールドベースのタスクを実行する方法について詳しくは、[フィールドベースのワークフロー](../ui/field-based-workflows.md)に関するガイドを参照してください。

スキーマレジストリ API の機能について詳しくは、[API の概要](../api/overview.md)でエンドポイントとプロセスの完全なリストを参照してください。
