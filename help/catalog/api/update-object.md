---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログ；api；オブジェクトの更新
solution: Experience Platform
title: カタログオブジェクトの更新
topic-legacy: developer guide
description: PATCH リクエストのパスに ID を含めることで、カタログオブジェクトの一部を更新できます。このドキュメントでは、フィールドの使用、およびカタログオブジェクトに対するPATCH操作の実行に JSON パッチ表記を使用する方法について説明します。
exl-id: 315de212-bf4d-40d5-a54f-9602a26d6852
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 81%

---

# カタログオブジェクトの更新

[!DNL Catalog] オブジェクトの一部を更新するには、その ID をPATCHリクエストのパスに含めます。 このドキュメントでは、カタログオブジェクトに対して PATCH 操作を実行する次の 2 つの方法について説明します。

* フィールドを使用する
* JSON パッチ表記を使用する

>[!NOTE]
>
>オブジェクトに対して PATCH 操作を実行しても、相互に関連するオブジェクトを表す拡張可能フィールドは変更されません。相互に関連するオブジェクトは、直接変更する必要があります。

## フィールドを使用した更新

次の呼び出し例は、フィールドと値を使用してオブジェクトを更新する方法を示しています。

**API 形式**

```http
PATCH /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 更新する [!DNL Catalog] オブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、データセットの `name` フィールドと `description` フィールドが、ペイロードで指定された値に更新されます。更新しないオブジェクトフィールドは、ペイロードから除外できます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

## JSON パッチ表記法を使用した更新

次の呼び出し例は、[RFC-6902](https://tools.ietf.org/html/rfc6902) で説明されている JSON パッチを使用してオブジェクトを更新する方法を示しています。

<!-- (Include once API fundamentals guide is published) 

For more information on JSON Patch syntax, see the [API fundamentals guide](). 

-->

**API 形式**

```http
PATCH /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 更新する [!DNL Catalog] オブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、データセットの `name` フィールドと `description` フィールドが、各 JSON パッチオブジェクトで指定された値に更新されます。JSON パッチを使用する場合は、Content-Type ヘッダーも `application/json-patch+json` に設定する必要があります。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
