---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの更新
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 3%

---


# オブジェクトの更新

カタログオブジェクトの一部を更新するには、PATCHリクエストのパスにIDを含めます。 このドキュメントでは、カタログ・オブジェクトに対してPATCH操作を実行する2つの方法を説明します。

* フィールドの使用
* JSONパッチ表記の使用

>[!NOTE]
>
>オブジェクトに対するPATCH操作では、拡大可能なフィールドを変更できません。これは、相互に関連するオブジェクトを表します。  相互に関連するオブジェクトに対する変更は、直接行う必要があります。

## フィールドを使用した更新

次の呼び出し例は、フィールドと値を使用してオブジェクトを更新する方法を示しています。

**API形式**

```http
PATCH /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 更新するカタログオブジェクトの種類です。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、データセットの各 `name``description` フィールドを、ペイロードに指定された値に更新します。 更新しないオブジェクトフィールドは、ペイロードから除外できます。

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

正常に完了すると、更新されたデータセットのIDを含む配列が返されます。 このIDは、PATCH要求で送信されたIDと一致する必要があります。 このデータセットに対してGETリクエストを実行すると、およ `name` びが更新され、その他すべての値は変更されないことが示され `description` るようになりました。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

## JSONパッチ表記を使用した更新

次の呼び出し例は、 [RFC-6902で概要を説明しているJSONパッチを使用してオブジェクトを更新する方法を示しています](https://tools.ietf.org/html/rfc6902)。

<!-- (Include once API fundamentals guide is published) 

For more information on JSON Patch syntax, see the [API fundamentals guide](). 

-->

**API形式**

```http
PATCH /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 更新するカタログオブジェクトの種類です。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、データセットのフィールド `name``description` とフィールドを、各JSONパッチオブジェクトに指定された値に更新します。 JSONパッチを使用する場合は、Content-Typeヘッダーもに設定する必要があり `application/json-patch+json`ます。

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

正常に応答すると、更新されたオブジェクトのIDを含む配列が返されます。 このIDは、PATCH要求で送信されたIDと一致する必要があります。 このオブジェクトに対してGETリクエストを実行すると、およ `name` びが更新され、その他すべての値は変更されないことが示され `description` るようになりました。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```
