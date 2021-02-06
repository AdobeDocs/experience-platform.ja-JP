---
keywords: Experience Platform；ホーム；人気の高いトピック；データセット；データセット；データセットの作成；データセットの作成；データセットの有効化
solution: Experience Platform
title: APIでのデータセットの作成
topic: developer guide
description: このドキュメントでは、Catalog Service APIでデータセットオブジェクトを作成する方法について説明します。
translation-type: tm+mt
source-git-commit: a489ab248793a063295578943ad600d8eacab6a2
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 53%

---


# APIでのデータセットの作成

[!DNL Catalog] APIを使用してデータセットを作成するには、データセットの基になる[!DNL Experience Data Model] (XDM)スキーマの`$id`値を把握しておく必要があります。 スキーマIDを取得したら、[!DNL Catalog] APIの`/datasets`エンドポイントにPOSTリクエストを行って、データセットを作成できます。

>[!NOTE]
>
>このドキュメントでは、[!DNL Catalog]内でデータセットオブジェクトを作成する方法についてのみ説明します。 データセットの作成、設定、監視の手順について詳しくは、次の[チュートリアル](../datasets/create.md)を参照してください。

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
>この例では、`containerFormat`プロパティに[Apache Parket](https://parquet.apache.org/documentation/latest/)ファイル形式を使用しています。 JSON ファイル形式の使用例については、『[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)』を参照してください。

**応答**

成功した応答は、HTTP Status 201（作成済み）と、新しく作成されたデータセットの ID を `"@/datasets/{DATASET_ID}"` 形式で含む配列で構成される応答オブジェクトを返します。データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```
