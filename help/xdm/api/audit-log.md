---
keywords: Experience Platform；ホーム；人気のあるトピック；API;XDM;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；監査；監査ログ；変更ログ；rpc;
solution: Experience Platform
title: 監査ログAPIエンドポイント
description: スキーマレジストリAPIの/auditlogエンドポイントを使用すると、既存のXDMリソースに対して行われた変更の時系列リストを取得できます。
topic: 開発ガイド
translation-type: tm+mt
source-git-commit: 0727ffa0c72bcb6a85de1a13215b691b97889b70
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 4%

---


# 監査ログエンドポイント

[!DNL Schema Registry]は、各エクスペリエンスデータモデル(XDM)リソースに対して、異なる更新間で発生したすべての変更のログを保持します。 [!DNL Schema Registry] APIの`/auditlog`エンドポイントを使用すると、IDで指定された任意のクラス、ミックスイン、データタイプ、またはスキーマの監査ログを取得できます。

## はじめに

このガイドで使用されるエンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)の一部です。 先に進む前に、[はじめに](./getting-started.md)を読んで、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

`/auditlog`エンドポイントは、[!DNL Schema Registry]でサポートされるリモートプロシージャコール(RPC)の一部です。 [!DNL Schema Registry] APIの他のエンドポイントとは異なり、RPCエンドポイントでは、`Accept`や`Content-Type`のような追加のヘッダーは必要ありません。また、`CONTAINER_ID`を使用しません。 代わりに、以下のAPI呼び出しで示すように、`/rpc`名前空間を使用する必要があります。

## リソースの監査ログの取得

`/auditlog`エンドポイントへのGET要求のパスでリソースのIDを指定すると、スキーマライブラリ内の任意のクラス、ミックスイン、データ型、またはスキーマの監査ログを取得できます。

**API 形式**

```http
GET /rpc/auditlog/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | 監査ログを取得するリソースの`meta:altId`またはURLエンコードされた`$id`。 |

**リクエスト**

次のリクエストは、`Restaurant`ミックスインの監査ログを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/rpc/auditlog/_{TENANT_ID}.mixins.922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に完了した場合は、リソースに対して行われた変更を最新から最新の順に時系列でリストして返します。

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
| `auditTrails` | オブジェクトの配列。各オブジェクトは、指定したリソースまたはその依存リソースに対して行われた変更を表します。 |
| `id` | 変更されたリソースの`$id`。 この値は、通常、要求パスで指定されたリソースを表しますが、変更のソースである場合は、依存リソースを表す場合があります。 |
| `action` | 行われた変更のタイプ。 |
| `path` | 変更または追加された特定のフィールドへのパスを示す[JSONポインター](../../landing/api-fundamentals.md#json-pointer)文字列。 |
| `value` | 新規または更新されたフィールドに割り当てられた値。 |