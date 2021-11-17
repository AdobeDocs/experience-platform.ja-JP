---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；監査；監査ログ；changelog；変更ログ；rpc;
solution: Experience Platform
title: 監査ログ API エンドポイント
description: スキーマレジストリ API の/auditlog エンドポイントを使用すると、既存の XDM リソースに加えられた変更のリストを時系列で取得できます。
topic-legacy: developer guide
exl-id: 8d33ae7c-0aa4-4f38-a183-a2ff1801e291
source-git-commit: 7abe27d7fcc461becb0495fcd470eaea031b94bc
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 7%

---

# 監査ログエンドポイント

エクスペリエンスデータモデル (XDM) リソースごとに、 [!DNL Schema Registry] は、異なる更新間に発生したすべての変更のログを保持します。 この `/auditlog` エンドポイント [!DNL Schema Registry] API を使用すると、ID で指定されたクラス、スキーマフィールドグループ、データタイプ、スキーマの監査ログを取得できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。続行する前に、 [入門ガイド](./getting-started.md) 関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

この `/auditlog` endpoint は、 [!DNL Schema Registry]. の他のエンドポイントとは異なり、 [!DNL Schema Registry] API、RPC エンドポイントは、 `Accept` または `Content-Type`、およびを使用しない `CONTAINER_ID`. 代わりに、 `/rpc` 名前空間と呼ばれます。

## リソースの監査ログの取得

スキーマライブラリ内の任意のクラス、フィールドグループ、データタイプ、スキーマの監査ログを取得するには、 `/auditlog` endpoint.

**API 形式**

```http
GET /rpc/auditlog/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | この `meta:altId` または URL エンコード済み `$id` を取得します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、スキーマの監査ログを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/rpc/auditlog/_{TENANT_ID}.schemas.50649eb1b040bf042d6400a0335901cd2a97d31a4eac4330 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、リソースに対して行われた変更の時系列のリストを、最新から最新の順に返します。

```json
[
  {
    "id": "https://ns.adobe.com/{TENANT_ID}/schemas/50649eb1b040bf042d6400a0335901cd2a97d31a4eac4330",
    "updatedUser": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "updatedTime": "02-19-2021 05:43:56",
    "requestId": "a14NMF0jd6BIfyXaHdTDl4bC4R0r9rht",
    "clientId": "{CLIENT_ID}",
    "sandBoxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
    "updates": [
      {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/50649eb1b040bf042d6400a0335901cd2a97d31a4eac4330",
        "xdmType": "schemas",
        "action": "remove",
        "path": "/meta:usageCount",
        "value": 0
      }
    ]
  },
  {
    "id": "https://ns.adobe.com/{TENANT_ID}/schemas/50649eb1b040bf042d6400a0335901cd2a97d31a4eac4330",
    "updatedUser": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "updatedTime": "02-19-2021 05:43:56",
    "requestId": "pFQbgmWrdbJrNB9GdxTSGECpXYWspu68",
    "clientId": "{CLIENT_ID}",
    "sandBoxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
    "updates": [
      {
        "id": "https://ns.adobe.com/{TENANT_ID}/classes/11052164b588f0c29584bf6ae1a6663a59aa65426c82389f",
        "xdmType": "classes",
        "action": "remove",
        "path": "/definitions/customFields/properties/_{TENANT_ID}/properties/loyaltySunday_ABC",
        "value": {
          "title": "LoyaltySundayABC",
          "description": "",
          "type": "string",
          "isRequired": false,
          "required": [],
          "meta:xdmType": "string"
        }
      },
      {
        "id": "https://ns.adobe.com/{TENANT_ID}/classes/11052164b588f0c29584bf6ae1a6663a59aa65426c82389f",
        "xdmType": "classes",
        "action": "remove",
        "path": "/definitions/customFields/properties/_{TENANT_ID}/properties/loyaltyMoxee_XYZ",
        "value": {
          "title": "LoyaltyMoxeeXYZ",
          "description": "",
          "type": "string",
          "isRequired": false,
          "required": [],
          "meta:xdmType": "string"
        }
      }
    ]
  }
]
```

| プロパティ | 説明 |
| --- | --- |
| `updates` | オブジェクトの配列。各オブジェクトは、指定したリソースまたはその依存リソースに対する変更を表します。 |
| `id` | この `$id` 変更されたリソースの この値は通常、リクエストパスで指定されたリソースを表しますが、変更のソースである場合は、依存リソースを表すことがあります。 |
| `xdmType` | 変更されたリソースのタイプ。 |
| `action` | 行われた変更のタイプ。 |
| `path` | A [JSON ポインター](../../landing/api-fundamentals.md#json-pointer) 変更または追加された特定のフィールドへのパスを示す文字列。 |
| `value` | 新しいフィールドまたは更新されたフィールドに割り当てられた値。 |

{style=&quot;table-layout:auto&quot;}
