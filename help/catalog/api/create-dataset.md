---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: 6d24637dc6cc282f98288b6416e4a3b7cebe42ea

---


# データセットの作成

Catalog APIを使用してデータセットを作成するには、データセットの基となるExperience Data Model(XDM) `$id` スキーマの値を把握しておく必要があります。 スキーマIDを取得したら、カタログAPIのエンドポイントにPOSTリクエストを送信して、データセ `/datasets` ットを作成できます。

>[!NOTE] このドキュメントでは、Catalogでのデータセットオブジェクトの作成方法についてのみ説明します。 データセットの作成、設定、監視の手順について詳しくは、次のチュートリアルを参照してく [ださい](../datasets/create.md)。

**API形式**

```HTTP
POST /dataSets
```

**リクエスト**

次のリクエストでは、事前に定義されたデータセットを参照するスキーマセットを作成します。

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
| `schemaRef.id` | データセ `$id` ットの基になるXDMスキーマのURI値。 |

>[!NOTE] この例では、プロパティに [パーケット](https://parquet.apache.org/documentation/latest/) ・ファイル形式を使用 `containerFormat` しています。 JSONファイル形式の使用例については、バッチインジェスト開発ガイド [を参照してください](../../ingestion/batch-ingestion/api-overview.md)。

**応答**

成功した応答は、HTTP Status 201(Created)と、新しく作成されたデータセットのIDを形式で含む配列で構成される応答オブジェクトを返しま `"@/datasets/{DATASET_ID}"`す。 データセットIDは、API呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```
