---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 2%

---


# データセットの作成

APIを使用してデータセットを作成するには、データセットの基となる [!DNL Catalog] (XDM)スキーマの `$id`[!DNL Experience Data Model] 値を把握しておく必要があります。 スキーマIDを取得したら、APIでエンドポイントにPOSTリクエストを行って、データセットを作成 `/datasets` でき [!DNL Catalog] ます。

>[!NOTE]
>
>このドキュメントでは、でデータセットオブジェクトを作成する方法についてのみ説明 [!DNL Catalog]します。 データセットの作成、設定、監視の手順について詳しくは、次の [チュートリアルを参照してください](../datasets/create.md)。

**API形式**

```HTTP
POST /dataSets
```

**リクエスト**

次のリクエストは、以前に定義したスキーマーを参照するデータセットを作成します。

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
| `schemaRef.id` | データセットの基となるXDMスキーマのURI `$id` 値。 |

>[!NOTE]
>
>次の例では、 [パーケット](https://parquet.apache.org/documentation/latest/) ・ファイル形式を `containerFormat` プロパティに使用します。 JSONファイル形式の使用例については、『 [batch ingestion developer guide](../../ingestion/batch-ingestion/api-overview.md)』を参照してください。

**応答**

正常に完了すると、HTTPステータス201（作成済み）と、新たに作成されたデータセットのIDを形式で含む配列で構成される応答オブジェクトが返され `"@/datasets/{DATASET_ID}"`ます。 データセットIDは、API呼び出しでデータセットを参照するために使用される、読み取り専用の、システム生成の文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```
