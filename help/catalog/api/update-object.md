---
keywords: Experience Platform；ホーム；人気のトピック；カタログ；api；オブジェクトの更新
solution: Experience Platform
title: カタログ オブジェクトを更新する
description: PATCH リクエストのパスに ID を含めることで、カタログオブジェクトの一部を更新できます。このドキュメントでは、フィールドの使用と、カタログオブジェクトに対してPATCH操作を実行するための JSON パッチ表記の使用について説明します。
exl-id: 315de212-bf4d-40d5-a54f-9602a26d6852
source-git-commit: 5534cd5d5b0122ccbfcb3bae6c664c9f2d6eda8a
workflow-type: tm+mt
source-wordcount: '692'
ht-degree: 41%

---

# Catalog オブジェクトの更新

PATCH リクエストのパスに ID を含めることで、[!DNL Catalog] オブジェクトの一部を更新できます。 このドキュメントでは、カタログオブジェクトに対して PATCH 操作を実行する次の 2 つの方法について説明します。

* フィールドを使用する
* JSON パッチ表記を使用する

>[!NOTE]
>
>オブジェクトに対するPATCHの操作では、相互に関連するオブジェクトを表す展開可能なフィールドを変更することはできません。 相互に関連するオブジェクトは、直接変更する必要があります。

## フィールドを使用した更新

次の呼び出し例は、フィールドと値を使用してオブジェクトを更新する方法を示しています。

**API 形式**

```http
PATCH /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 更新するオブジェ [!DNL Catalog] トのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、データセットの `name` フィールドと `description` フィールドが、ペイロードで指定された値に更新されます。更新しないオブジェクトフィールドは、ペイロードから除外できます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
       "name":"Updated Dataset Name",
       "description":"Updated description for Sample Dataset"
      }'
```

**応答**

リクエストが成功した場合、更新されたデータセットの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致します。このデータセットに対して GET リクエストを実行すると、`name` および `description` のみが更新され、他の値は変更されていないことが示されるようになりました。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

## JSON パッチ表記法を使用した更新 {#patch-notation}

次の呼び出し例は、[RFC-6902](https://tools.ietf.org/html/rfc6902) で説明されている JSON パッチを使用してオブジェクトを更新する方法を示しています。

JSON パッチ構文について詳しくは、[API の基本ガイド ](../../landing/api-fundamentals.md#json-patch) を参照してください。

**API 形式**

```http
PATCH /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 更新するオブジェ [!DNL Catalog] トのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、データセットの `name` フィールドと `description` フィールドが、各 JSON パッチオブジェクトで指定された値に更新されます。JSON パッチを使用する場合は、Content-Type ヘッダーも `application/json-patch+json` に設定する必要があります。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json-patch+json' \
  -d '[
        { "op": "add", "path": "/name", "value": "New Dataset Name" },
        { "op": "add", "path": "/description", "value": "New description for dataset" }
      ]'
```

**応答**

リクエストが成功した場合、更新されたオブジェクトの ID を含む配列が返されます。この ID は、PATCH リクエストで送信された ID と一致します。このオブジェクトに対して GET リクエストを実行すると、`name` および `description` のみが更新され、他の値は変更されていないことが示されるようになりました。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

## PATCH v2 表記を使用して更新 {#patch-v2-notation}

`/v2/dataSets/{DATASET_ID}` エンドポイントを使用すると、複雑なデータセット属性や深くネストされたデータセット属性を、より柔軟に更新できます。

通常、深くネストされたフィールド（`a.b.c.d` など）を更新する場合、パスの各レベルが既に存在している必要があります。 いずれかのレベルが欠落している場合は、最終値を設定する前に各レベルを手動で作成する必要があります。 これには多くの場合、複数の操作が必要で、複雑さが増し、ミスの可能性が高まります。

`/v2/dataSets/{DATASET_ID}` エンドポイントは、パスで欠落しているレベルを自動的に作成します。 `d` を設定する前に、`b` と `c` を手動で確認して追加する代わりに、PATCH `v2` の操作によって自動的に追加されます。

PATCH リクエストを `/v2/dataSets/{DATASET_ID}` エンドポイントに送信する場合は、最終的な構造を送信するだけで、更新を適用する前に不足している部分が自動的に入力されます。

>[!NOTE]
>
>`/v2/dataSets/{id}` エンドポイントの場合、`If-Match` ヘッダーと `If-None-Match` ヘッダーはオプションです。 このエンドポイントへのPATCH リクエストは更新を動的に結合し、最新のデータセットバージョンを取得せずに変更を可能にします。 これにより、同時更新によるデータ損失のリスクは軽減されますが、`If-Match` を最新の `etag` で使用すると、変更が特定のバージョンにのみ適用されるようにすることができます。 または、データセット `If-None-Match` 最後の既知のバージョン以降に変更されていない場合、更新を防ぐことができます。

**API 形式**

```http
PATCH /V2/DATASETS/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 更新するデータセットの識別子。 |

**リクエスト**

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/catalog/v2/dataSets/67b3077efa10d92ab7a71858 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "extensions": {
            "adobe_lakeHouse": {
            "rowExpiration": {
                "ttlValue": "P9Y"
            }
            }
        }
    }'
```

**応答**

正常な応答では、更新されたデータセットの ID を含む配列が返されます。これは、PATCH リクエストで送信された ID と一致している必要があります。 このオブジェクトに対してGET リクエストを実行すると、事前の手動の作成手順を必要とせずに、`extensions.adobe_lakeHouse.rowExpiration` オブジェクトが作成されたことが示されるようになりました。

```json
[
    "@/dataSets/67b3077efa10d92ab7a71858"
]
```

### 更新前後のデータセットの例

次の JSON の例では、`extensions.adobe_lakeHouse.rowExpiration` オブジェクトがデータセットに存在しないPATCH リクエストの前 **データセット構造を示しています**

+++選択すると例が表示されます

```JSON
{
    "67b3077efa10d92ab7a71858": {
        "name": "Acme Sales Data",
        "description": "This dataset contains sales transaction records for Acme Corporation.",
        "tags": {
            "adobe/siphon/table/format": [
                "delta"
            ],
            "adobe/pqs/table": [
                "testdataset_20250217_095510_966"
            ]
        },
        "classification": {
            "dataBehavior": "time-series",
            "managedBy": "CUSTOMER"
        },
        "createdUser": "{USER_ID}",
        "imsOrg": "{ORG_ID}",
        "sandboxId": "{SANDBOX_ID}",
        "createdClient": "{CLIENT_ID}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "extensions": {
            "adobe_lakeHouse": {},
            "adobe_unifiedProfile": {}
        },
        "created": 1739786110978,
        "updated": 1739786111203,
        "viewId": "{VIEW_ID}",
        "basePath": "{STORAGE_PATH}",
        "fileDescription": {},
        "files": "@/dataSetFiles?dataSetId=67b3077efa10d92ab7a71858",
        "schemaRef": {
            "id": "{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed+json; version=1"
        },
        "persistence": {
            "adls": {
                "location": "{STORAGE_PATH}",
                "adlsType": "GEN2",
                "credentials": "@/dataSets/67b3077efa10d92ab7a71858/credentials"
            }
        }
    }
}
```

+++

次の JSON は、PATCH リクエストの後のデータセット構造を示しています **** この更新により、以前の手動作成手順を実行しなくても、見つからない `extensions.adobe_lakeHouse.rowExpiration` オブジェクトが自動的に作成されます。 この例では、`/v2/` PATCH リクエストによって複数の操作が不要になり、更新がより簡単かつ効率的になる仕組みを示しています。


+++選択すると例が表示されます

```JSON
{
    "67b3077efa10d92ab7a71858": {
        "name": "Acme Sales Data",
        "description": "This dataset contains sales transaction records for Acme Corporation.",
        "tags": {
            "adobe/siphon/table/format": [
                "delta"
            ],
            "adobe/pqs/table": [
                "testdataset_20250217_095510_966"
            ]
        },
        "imsOrg": "{ORG_ID}",
        "sandboxId": "{SANDBOX_ID}",
        "extensions": {
            "adobe_lakeHouse": {
                "rowExpiration": {
                    "ttlValue": "{TTL_VALUE}"
                }
            },
            "adobe_unifiedProfile": {}
        },
        "version": "{VERSION}",
        "created": "{CREATED_TIMESTAMP}",
        "updated": "{UPDATED_TIMESTAMP}",
        "createdClient": "{CLIENT_ID}",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "classification": {
            "dataBehavior": "time-series",
            "managedBy": "CUSTOMER"
        },
        "viewId": "{VIEW_ID}",
        "basePath": "{STORAGE_PATH}",
        "fileDescription": {},
        "files": "@/dataSetFiles?dataSetId=67b3077efa10d92ab7a71858",
        "schemaRef": {
            "id": "{SCHEMA_ID}",
            "contentType": "{CONTENT_TYPE}"
        },
        "persistence": {
            "adls": {
                "location": "{STORAGE_PATH}",
                "adlsType": "{STORAGE_TYPE}",
                "credentials": "@/dataSets/67b3077efa10d92ab7a71858/credentials"
            }
        }
    }
}
```

+++
