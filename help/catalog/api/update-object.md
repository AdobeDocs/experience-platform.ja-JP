---
keywords: Experience Platform；ホーム；人気のトピック；カタログ；api；オブジェクトの更新
solution: Experience Platform
title: カタログ オブジェクトを更新する
description: PATCH リクエストのパスに ID を含めることで、カタログオブジェクトの一部を更新できます。このドキュメントでは、フィールドの使用と、カタログオブジェクトに対してPATCH操作を実行するための JSON パッチ表記の使用について説明します。
exl-id: 315de212-bf4d-40d5-a54f-9602a26d6852
source-git-commit: 296a988a67871933723ad0474c113cb93fdbf255
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 74%

---

# Catalog オブジェクトの更新

PATCHリクエストのパスに ID を含めることで、[!DNL Catalog] オブジェクトの一部を更新できます。 このドキュメントでは、カタログオブジェクトに対して PATCH 操作を実行する次の 2 つの方法について説明します。

* フィールドを使用する
* JSON パッチ表記を使用する

>[!NOTE]
>
>オブジェクトのPATCH操作では、相互に関連するオブジェクトを表す展開可能なフィールドを修正することはできません。 相互に関連するオブジェクトは、直接変更する必要があります。

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

## JSON パッチ表記法を使用した更新

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
