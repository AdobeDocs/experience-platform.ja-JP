---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；監査；監査ログ；変更ログ；変更ログ；rpc;
solution: Experience Platform
title: 監査ログ API エンドポイント
description: スキーマレジストリ API の/auditlog エンドポイントを使用すると、既存の XDM リソースに加えられた変更の時系列リストを取得できます。
exl-id: 8d33ae7c-0aa4-4f38-a183-a2ff1801e291
source-git-commit: 983682489e2c0e70069dbf495ab90fc9555aae2d
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 13%

---

# 監査ログエンドポイント

[!DNL Schema Registry] は、エクスペリエンスデータモデル（XDM）リソースごとに、異なる更新間に発生したすべての変更のログを保持します。 [!DNL Schema Registry] API の `/auditlog` エンドポイントを使用すると、ID で指定された任意のクラス、スキーマフィールドグループ、データタイプ、スキーマの監査ログを取得できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

`/auditlog` エンドポイントは、[!DNL Schema Registry] でサポートされているリモート プロシージャ コール （RPC）の一部です。 [!DNL Schema Registry] API の他のエンドポイントとは異なり、RPC エンドポイントには `Accept` や `Content-Type` などの追加のヘッダーは必要なく、`CONTAINER_ID` も使用しません。 代わりに、以下の API 呼び出しで示すように、`/rpc` 名前空間を使用する必要があります。

## リソースの監査ログの取得

スキーマライブラリ内のクラス、フィールドグループ、データタイプ、スキーマの監査ログを取得するには、`/auditlog` エンドポイントに対するGETリクエストのパスでリソースの ID を指定します。

**API 形式**

```http
GET /rpc/auditlog/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | 監査ログを取得するリソースの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストでは、スキーマの監査ログを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/rpc/auditlog/_{TENANT_ID}.schemas.50649eb1b040bf042d6400a0335901cd2a97d31a4eac4330 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、リソースに加えられた変更のリストが、最新の状態から古い状態へと時系列で返されます。

```json
[
  {
    "id": "https://ns.adobe.com/{TENANT_ID}/schemas/50649eb1b040bf042d6400a0335901cd2a97d31a4eac4330",
    "updatedUser": "{USER_ID}",
    "imsOrg": "{ORG_ID}",
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
    "imsOrg": "{ORG_ID}",
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
| `updates` | オブジェクトの配列。各オブジェクトは、指定されたリソースまたはその依存リソースの 1 つに対する変更を表します。 |
| `id` | 変更されたリソースの `$id`。 この値は、通常、リクエストパスで指定されたリソースを表しますが、変更のソースである場合は、依存リソースを表す場合があります。 |
| `xdmType` | 変更されたリソースのタイプ。 |
| `action` | 行われた変更のタイプ。 |
| `path` | 変更または追加された特定のフィールドへのパスを示す [JSON ポインター &#x200B;](../../landing/api-fundamentals.md#json-pointer) 文字列。 |
| `value` | 新しいフィールドまたは更新されたフィールドに割り当てられた値。 |

{style="table-layout:auto"}
