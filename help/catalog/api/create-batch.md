---
keywords: Experience Platform；ホーム；人気の高いトピック；作成バッチ；カタログサービス；api
solution: Experience Platform
title: APIでのバッチの作成
topic: developer guide
description: カタログAPIの/batchesエンドポイントにPOSTリクエストを作成して、バッチを作成できます。
translation-type: tm+mt
source-git-commit: 8a213ac0ef1ac0f9c42e4b880b24157d28878bf1
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 51%

---


# バッチの作成

データセットがデータを取得するには、バッチが関連付けられている必要があります。既存のデータセットの`id`値を使用して、[!DNL Catalog] APIの`/batches`エンドポイントへのPOSTリクエストを行うことで、バッチを作成できます。

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
