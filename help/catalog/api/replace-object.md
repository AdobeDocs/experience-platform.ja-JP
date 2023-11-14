---
keywords: Experience Platform；ホーム；人気のトピック；カタログ；API；オブジェクトの置換
solution: Experience Platform
title: カタログオブジェクトの置換
description: PUT リクエストを使用して、カタログオブジェクトのコンテンツを上書きできます。この場合、リソース全体がリクエストペイロードで置き換えられます。
exl-id: cd98d13c-5261-4bff-b5db-af5f06d093c9
source-git-commit: 2226b1878ef3398554b6cf96ff400cc1767a9e4c
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 60%

---

# カタログオブジェクトの置き換え

次の項目の内容を上書きできます： [!DNL Catalog] PUTリクエストを使用するオブジェクト。リソース全体がリクエストペイロードに置き換えられます。

>[!NOTE]
>
>更新が必要なのが、 [!DNL Catalog] オブジェクトの方が、PATCHリクエストを使用する方が効率的です。

**API 形式**

```http
PUT /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 次のタイプの [!DNL Catalog] 置き換えるオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
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
