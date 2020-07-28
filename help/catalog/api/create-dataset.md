---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 70%

---


# データセットの作成

In order to create a dataset using the [!DNL Catalog] API, you must know the `$id` value of the [!DNL Experience Data Model] (XDM) schema on which the dataset will be based. Once you have the schema ID, you can create a dataset by making a POST request to the `/datasets` endpoint in the [!DNL Catalog] API.

>[!NOTE]
>
>This document only covers how to create a dataset object in [!DNL Catalog]. データセットの作成、設定、監視の手順について詳しくは、次の[チュートリアル](../datasets/create.md)を参照してください。

**API 形式**

```HTTP
POST /dataSets
```

**リクエスト**

次のリクエストは、以前に定義されたスキーマを参照するデータセットを作成します。

```SHELL
curl -X POST \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?requestDataSource=true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name":"LoyaltyMembersDataset",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/719c4e19184402c27595e65b931a142b",
        "contentType": "application/vnd.adobe.xed+json;version=1"
    },
    "fileDescription": {
        "persisted": true,
        "containerFormat": "parquet",
        "format": "parquet"
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 作成するデータセットの名前。 |
| `schemaRef.id` | データセットの基になる XDM スキーマの URI `$id` 値。 |

>[!NOTE]
>
> この例では、[ プロパティに ](https://parquet.apache.org/documentation/latest/) parquet `containerFormat` ファイル形式を使用しています。JSON ファイル形式の使用例については、『[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)』を参照してください。

**応答**

成功した応答は、HTTP Status 201（作成済み）と、新しく作成されたデータセットの ID を `"@/datasets/{DATASET_ID}"` 形式で含む配列で構成される応答オブジェクトを返します。データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```
