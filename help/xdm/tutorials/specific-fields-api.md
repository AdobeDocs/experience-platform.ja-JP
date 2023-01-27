---
title: スキーマレジストリ API を使用したスキーマへの特定のフィールドの追加
description: スキーマレジストリ API を使用して、既存のフィールドグループから Experience Data Model(XDM) スキーマに個々のフィールドを追加する方法を説明します。
source-git-commit: 4bcd949e901c11bb933000f7ae76f17134dda496
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 2%

---

# スキーマレジストリ API を使用したスキーマへの特定のフィールドの追加

Experience Data Model(XDM) スキーマは基本クラスで構成され、Adobeで定義された標準フィールドグループと、組織で定義されたカスタムフィールドグループを使用して、追加のフィールドが含まれます。

スキーマを構築する際に、特定のフィールドグループの一部のフィールドを使用し、必要のない同じグループの他のフィールドを除外することができます。 このチュートリアルでは、スキーマレジストリ API を使用して、フィールドグループからスキーマに個々のフィールドを追加する方法について説明します。

>[!NOTE]
>
>Adobe Experience Platform UI で個々のスキーマフィールドを追加および削除する方法について詳しくは、 [フィールドベースのワークフロー](../ui/field-based-workflows.md) （現在はベータ版）。

## 前提条件

このチュートリアルでは、 [スキーマレジストリ API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/). 開始する前に、 [開発者ガイド](../api/getting-started.md) を参照してください。 `{TENANT_ID}`、コンテナの概念、およびリクエストをおこなうために必要なヘッダー。

## について `meta:refProperty` フィールド

任意のスキーマについて、その構造を構成するクラスグループとフィールドグループは、その下で参照されます `allOf` 配列。 各コンポーネントは、 `$ref` コンポーネントの URI を参照するプロパティ `$id`.

次の JSON は、1 つのクラス (`experienceevent`) およびフィールドグループ (`experienceevent-all`):

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

( `allOf` フィールドグループを参照する配列。兄弟を追加できます `meta:refProperty` フィールドを使用して、スキーマに含める必要のあるグループ内のフィールドを指定します。

>[!NOTE]
>
>各フィールドは、それぞれのフィールドグループ内のフィールドへのパスを表す JSON ポインター文字列を使用して指定されます。 文字列の先頭にはスラッシュ (`/`) およびを含めることはできません `properties` 名前空間。 例：`/_experience/campaign/message/id`。

文字列として含める場合、 `meta:refProperty` は、グループ内の単一のフィールドを参照できます。 同じグループの他のフィールドは、同じ `$ref` 異なる `meta:refProperty` の値です。

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

または、 `meta:refProperty` を配列として指定し、1 つの内の特定のグループから含める複数のフィールドを指定できます `allOf` リスト項目：

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

## 「Add fields」操作でのPUT

PUTリクエストを使用して、スキーマ全体を書き換え、に含めるフィールドを設定できます `allOf`.

**API 形式**

```http
PUT /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` 書き換えるスキーマの数を指定します。 |

**リクエスト**

次のリクエストでは、 `allOf` 配列。

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

正常な応答は、更新されたスキーマの詳細を返します。

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
>スキーマのPUTリクエストについて詳しくは、 [スキーマエンドポイントガイド](../api/schemas.md#put).

## 「Add fields」操作でのPATCH

スキーマリクエストを使用すると、PATCHに個々のフィールドを追加する際に、他のフィールドを上書きすることはできません。 スキーマレジストリは、次を含むすべての標準 JSON パッチ操作をサポートしています `add`, `remove`、および `replace`. JSON パッチの詳細については、 [API の基本事項ガイド](../../landing/api-fundamentals.md#json-patch).

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` 書き換えるスキーマの数を指定します。 |

**リクエスト**

次のリクエストは、新しいオブジェクトをスキーマの `allOf` 配列。追加するフィールドを指定します。

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

正常な応答は、更新されたスキーマの詳細を返します。

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
>スキーマのPATCHリクエストについて詳しくは、 [スキーマエンドポイントガイド](../api/schemas.md#patch).

## 次の手順

このガイドでは、API 呼び出しを使用して、既存のフィールドグループからスキーマに個々のフィールドを追加する方法を説明しました。 Platform UI で同様のフィールドベースのタスクを実行する方法について詳しくは、 [フィールドベースのワークフロー](../ui/field-based-workflows.md).

スキーマレジストリ API の機能について詳しくは、 [API の概要](../api/overview.md) エンドポイントとプロセスの完全なリスト。
