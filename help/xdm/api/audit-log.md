---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；監査；監査ログ；変更ログ；rpc;
solution: Experience Platform
title: 監査ログAPIエンドポイント
description: スキーマレジストリAPIの/auditlogエンドポイントを使用すると、既存のXDMリソースに対して行われた変更のリストを時系列で取得できます。
topic-legacy: developer guide
exl-id: 8d33ae7c-0aa4-4f38-a183-a2ff1801e291
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 6%

---

# 監査ログエンドポイント

[!DNL Schema Registry]は、エクスペリエンスデータモデル(XDM)リソースごとに、異なる更新の間に発生したすべての変更のログを保持します。 [!DNL Schema Registry] APIの`/auditlog`エンドポイントを使用すると、IDで指定された任意のクラス、スキーマフィールドグループ、データタイプ、またはスキーマの監査ログを取得できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。続行する前に、[はじめにのガイド](./getting-started.md)を参照して、関連ドキュメントへのリンク、このドキュメントのAPI呼び出し例の読み方、およびExperience PlatformAPIを正しく呼び出すために必要なヘッダーに関する重要な情報を確認してください。

`/auditlog`エンドポイントは、[!DNL Schema Registry]でサポートされるリモートプロシージャコール(RPC)の一部です。 [!DNL Schema Registry] APIの他のエンドポイントとは異なり、RPCエンドポイントは、`Accept`や`Content-Type`のような追加のヘッダーを必要とせず、`CONTAINER_ID`を使用しません。 代わりに、以下のAPI呼び出しで示すように、名前空間`/rpc`を使用する必要があります。

## リソースの監査ログの取得

スキーマライブラリ内の任意のクラス、フィールドグループ、データタイプ、またはスキーマの監査ログを取得するには、`/auditlog`エンドポイントへのGETリクエストのパスでリソースのIDを指定します。

**API 形式**

```http
GET /rpc/auditlog/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | 監査ログを取得するリソースの`meta:altId`またはURLエンコードされた`$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、`Restaurant`フィールドグループの監査ログを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/rpc/auditlog/_{TENANT_ID}.mixins.922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、リソースに対して行われた変更の時系列のリスト（最新から最新のもの）を返します。

```json
[
  {
    "id": "https://ns.adobe.com/{TENANT_ID}/mixins/922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9",
    "auditTrails": [
      {
        "id": "https://ns.adobe.com/{TENANT_ID}/mixins/922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9",
        "xdmType": "mixins",
        "action": "add",
        "path": "/definitions/customFields/properties/_{TENANT_ID}/properties/brand",
        "value": {
          "title": "Brand",
          "description": "",
          "type": "string",
          "isRequired": false,
          "meta:xdmType": "string"
        }
      },
      {
        "id": "https://ns.adobe.com/{TENANT_ID}/mixins/922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9",
        "xdmType": "mixins",
        "action": "add",
        "path": "/meta:usageCount",
        "value": 0
      }
    ],
    "updatedUser": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "updated": 1606255582281,
    "clientId": "{CLIENT_ID}",
    "sandBoxId": "{SANDBOX_ID}"
  }
]
```

| プロパティ | 説明 |
| --- | --- |
| `auditTrails` | オブジェクトの配列。各オブジェクトは、指定されたリソースまたはその依存リソースに対して行われた変更を表します。 |
| `id` | 変更されたリソースの`$id`。 この値は通常、リクエストパスで指定されたリソースを表しますが、変更のソースである場合は、依存リソースを表す場合があります。 |
| `action` | 行われた変更のタイプ。 |
| `path` | 変更または追加された特定のフィールドへのパスを示す[JSONポインター](../../landing/api-fundamentals.md#json-pointer)文字列。 |
| `value` | 新しいフィールドまたは更新されたフィールドに割り当てられた値。 |

{style=&quot;table-layout:auto&quot;}
