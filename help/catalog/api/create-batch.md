---
keywords: Experience Platform;home;popular topics;create batch;catalog service;api
solution: Experience Platform
title: データセットの作成
topic: developer guide
description: データセットがデータを取得するには、バッチが関連付けられている必要があります。既存のデータセットのID値を使用して、カタログAPIの/batchesエンドポイントにPOSTリクエストを行うことで、バッチを作成できます。
translation-type: tm+mt
source-git-commit: 14f99c23cd82894fee5eb5c4093b3c50b95c52e8
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 61%

---


# バッチの作成

データセットがデータを取得するには、バッチが関連付けられている必要があります。Using the `id` value of an existing dataset, you can create a batch by making a POST request to the `/batches` endpoint in the [!DNL Catalog] API.

**API 形式**

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
| `datasetId` | バッチが関連付けられるデータセットの `id`。 |

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたバッチの詳細（読み取り専用の、システムで生成された `id` 文字列を含む）を含む応答オブジェクトを返します。

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
