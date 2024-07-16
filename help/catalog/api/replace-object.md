---
keywords: Experience Platform；ホーム；人気のトピック；カタログ；api；オブジェクトの置き換え
solution: Experience Platform
title: Catalog オブジェクトを置換する
description: PUT リクエストを使用して、カタログオブジェクトのコンテンツを上書きできます。この場合、リソース全体がリクエストペイロードで置き換えられます。
exl-id: cd98d13c-5261-4bff-b5db-af5f06d093c9
source-git-commit: 2d6167ee7aaa0b79514be6e532e61602ae5cc640
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 60%

---

# Catalog オブジェクトを置換する

リソースリクエストを使用して [!DNL Catalog] オブジェクトの内容を上書きできます。その際、PUT全体がリクエストペイロードに置き換えられます。

>[!NOTE]
>
>[!DNL Catalog] オブジェクト内のいくつかの特定のフィールドのみを更新する必要がある場合は、PATCHリクエストを使用する方が効率的な場合があります。

**API 形式**

```http
PUT /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 置き換え [!DNL Catalog] オブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストは、ペイロードで指定された値でデータセットを上書きします。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "New Dataset Name",
        "description": "New description for dataset",
        "tags": {
            "adobe/pqs/table": [
                "sample_dataset"
            ]
        },
        "files": "@/dataSetFiles?dataSetId=5ba9452f7de80400007fc52a"
    }'
```

**応答**

成功した場合は、上書きされたオブジェクトの ID を含む配列が返されます。この ID は、PUT リクエストで送信された ID と一致する必要があります。このオブジェクトに対して GET リクエストを実行すると、その詳細が以前の PUT リクエストのペイロードで提供された詳細に置き換えられたことが示されます。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```
