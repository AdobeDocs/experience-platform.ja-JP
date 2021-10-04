---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；監査；監査ログ；変更ログ；rpc;
solution: Experience Platform
title: 監査ログ API エンドポイント
description: スキーマレジストリ API の/auditlog エンドポイントを使用すると、既存の XDM リソースに加えられた変更のリストを時系列で取得できます。
topic-legacy: developer guide
exl-id: 8d33ae7c-0aa4-4f38-a183-a2ff1801e291
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 7%

---

# 監査ログエンドポイント

[!DNL Schema Registry] は、各エクスペリエンスデータモデル (XDM) リソースに対して、異なる更新間に発生したすべての変更のログを保持します。 [!DNL Schema Registry] API の `/auditlog` エンドポイントを使用すると、ID で指定された任意のクラス、スキーマフィールドグループ、データタイプ、スキーマの監査ログを取得できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。続行する前に、関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方、およびExperience PlatformAPI を正しく呼び出すために必要なヘッダーに関する重要な情報については、[ はじめに ](./getting-started.md) を参照してください。

`/auditlog` エンドポイントは、[!DNL Schema Registry] でサポートされるリモートプロシージャコール (RPC) の一部です。 [!DNL Schema Registry] API の他のエンドポイントとは異なり、RPC エンドポイントは `Accept` や `Content-Type` のような追加のヘッダーを必要とせず、`CONTAINER_ID` を使用しません。 代わりに、以下の API 呼び出しで示すように、 `/rpc` 名前空間を使用する必要があります。

## リソースの監査ログの取得

`/auditlog` エンドポイントへのGETリクエストのパスでリソースの ID を指定することで、スキーマライブラリ内の任意のクラス、フィールドグループ、データ型、またはスキーマの監査ログを取得できます。

**API 形式**

```http
GET /rpc/auditlog/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | 監査ログを取得するリソースの `meta:altId` または URL エンコードされた `$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、`Restaurant` フィールドグループの監査ログを取得します。

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
| `auditTrails` | オブジェクトの配列。各オブジェクトは、指定したリソースまたはその依存リソースに対する変更を表します。 |
| `id` | 変更されたリソースの `$id`。 この値は通常、リクエストパスで指定されたリソースを表しますが、変更のソースである場合は、依存リソースを表す場合があります。 |
| `action` | 加えられた変更のタイプ。 |
| `path` | 変更または追加された特定のフィールドへのパスを示す [JSON ポインター ](../../landing/api-fundamentals.md#json-pointer) 文字列。 |
| `value` | 新規または更新されたフィールドに割り当てられた値。 |

{style=&quot;table-layout:auto&quot;}
