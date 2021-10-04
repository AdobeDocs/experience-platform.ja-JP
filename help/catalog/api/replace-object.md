---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログ；api；オブジェクトの置換
solution: Experience Platform
title: カタログオブジェクトの置換
topic-legacy: developer guide
description: PUT リクエストを使用して、カタログオブジェクトのコンテンツを上書きできます。この場合、リソース全体がリクエストペイロードで置き換えられます。
exl-id: cd98d13c-5261-4bff-b5db-af5f06d093c9
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 60%

---

# カタログオブジェクトの置換

PUTリクエストを使用して [!DNL Catalog] オブジェクトの内容を上書きできます。この場合、リソース全体がリクエストペイロードで置き換えられます。

>[!NOTE]
>
>[!DNL Catalog] オブジェクト内の特定のフィールドのみを更新する必要がある場合は、PATCHリクエストを使用する方が効率的です。

**API 形式**

```http
PUT /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 置き換える [!DNL Catalog] オブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストは、ペイロードで指定された値でデータセットを上書きします。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "New Dataset Name",
        "description": "New description for dataset",
        "state": "DRAFT",
        "tags": {
            "adobe/pqs/table": [
                "sample_dataset"
            ]
        },
        "files": "@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"
    }'
```

**応答**

成功した場合は、上書きされたオブジェクトの ID を含む配列が返されます。この ID は、PUT リクエストで送信された ID と一致する必要があります。このオブジェクトに対して GET リクエストを実行すると、その詳細が以前の PUT リクエストのペイロードで提供された詳細に置き換えられたことが示されます。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```
