---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: a25ca22fb8ec9eb95f74e4fd76a7f18e87343085
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 4%

---


# バッチの作成

データセットがデータを取り込むには、データセットにバッチが関連付けられている必要があります。 既存のデータセットの `id` 値を使用して、カタログAPIのエンドポイントにPOSTリクエストを行うことで、バッチを作成でき `/batches` ます。

**API形式**

```HTTP
POST /batches
```

**リクエスト**

```SHELL
curl -X POST 'https://platform.adobe.io/data/foundation/import/batches' \
  -H 'accept: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key : {API_KEY}' \
  -H 'content-type: application/json' \
  -d '{
        "datasetId":"5c8c3c555033b814b69f947f"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `datasetId` | バッチ `id` が関連付けられるデータセットの名前。 |

**応答**

正常な応答は、HTTPステータス201（作成済み）と、新しく作成されたバッチの詳細（読み取り専用の、システムで生成された文字列を含む） `id`を含む応答オブジェクトを返します。

```JSON
{
    "id": "5d01230fc78a4e4f8c0c6b387b4b8d1c",
    "imsOrg": "{IMS_ORG}",
    "updated": 1552694873602,
    "status": "loading",
    "created": 1552694873602,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "5c8c3c555033b814b69f947f"
        }
    ],
    "version": "1.0.0",
    "tags": {
        "acp_producer": [
            "{CREATED_CLIENT}"
        ],
        "acp_stagePath": [
            "{CREATED_CLIENT}/stage/5d01230fc78a4e4f8c0c6b387b4b8d1c"
        ],
        "use_plan_b_batch_status": [
            "false"
        ]
    },
    "createdUser": "{CREATED_BY}",
    "updatedUser": "{CREATED_BY}",
    "externalId": "5d01230fc78a4e4f8c0c6b387b4b8d1c",
    "createdClient": "{CREATED_CLIENT}",
    "inputFormat": {
        "format": "parquet"
    }
}
```
