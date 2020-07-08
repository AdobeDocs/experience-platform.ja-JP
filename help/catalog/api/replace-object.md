---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの置換
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 2%

---


# オブジェクトの置換

PUTリクエストを使用して、Catalogオブジェクトのコンテンツを上書きできます。この場合、リソース全体がリクエストペイロードに置き換えられます。

>[!NOTE]
>
>カタログオブジェクト内の特定のフィールドを更新するだけで済む場合は、PATCHリクエストを使用する方が効率的です。

**API形式**

```http
PUT /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 置き換えるカタログオブジェクトの種類です。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストは、ペイロードに指定された値でデータセットを上書きします。

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

正常に応答すると、上書きされたオブジェクトのIDを含む配列が返されます。 このIDは、PUT要求で送信されたIDと一致する必要があります。 このオブジェクトに対してGETリクエストを実行すると、その詳細が以前のPUTリクエストのペイロードで提供された詳細に置き換えられたことが示されます。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```
