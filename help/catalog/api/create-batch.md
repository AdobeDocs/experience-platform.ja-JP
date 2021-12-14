---
keywords: Experience Platform；ホーム；人気の高いトピック；バッチの作成；カタログサービス；API
solution: Experience Platform
title: API でのバッチの作成
topic-legacy: developer guide
description: カタログ API の/batches エンドポイントにPOSTリクエストを送信して、バッチを作成できます。
exl-id: 1d2cbca9-1cd6-4b89-9b77-3687268bd849
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 51%

---

# バッチの作成

データセットがデータを取得するには、バッチが関連付けられている必要があります。の使用 `id` 既存のデータセットの値の場合、 `/batches` エンドポイント [!DNL Catalog] API

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
  -H 'x-api-key: {API_KEY}' \
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
