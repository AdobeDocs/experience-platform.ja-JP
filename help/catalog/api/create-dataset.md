---
keywords: Experience Platform；ホーム；人気のトピック；データセット；データセット；データセットを作成；データセットを作成；データセットを有効にする
solution: Experience Platform
title: API でのデータセットの作成
description: このドキュメントでは、Catalog Service API でデータセットオブジェクトを作成する方法について説明します。
exl-id: f3e5de7f-1781-4898-ac42-063eb51e661a
source-git-commit: 74867f56ee13430cbfd9083a916b7167a9a24c01
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 53%

---

# API でのデータセットの作成

[!DNL Catalog] API を使用してデータセットを作成するには、データセットの基となる [!DNL Experience Data Model] （XDM）スキーマの `$id` 値を知っておく必要があります。 スキーマ ID を取得したら、[!DNL Catalog] API の `/datasets` エンドポイントに対してPOSTリクエストを実行することで、データセットを作成できます。

>[!NOTE]
>
>このドキュメントでは、[!DNL Catalog] でデータセットオブジェクトを作成する方法についてのみ説明します。 データセットの作成、設定、監視の手順について詳しくは、次の[チュートリアル](../datasets/create.md)を参照してください。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name":"LoyaltyMembersDataset",
    "schemaRef": {
        "id": "https://ns.adobe.com/{TENANT_ID}/schemas/719c4e19184402c27595e65b931a142b",
        "contentType": "application/vnd.adobe.xed+json;version=1"
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 作成するデータセットの名前。 |
| `schemaRef.id` | データセットの基になる XDM スキーマの URI `$id` 値。 |
| `schemaRef.contentType` | スキーマの形式とバージョンを示します。 詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。 |

>[!NOTE]
>
>この例では、`containerFormat` プロパティに [Apache Parquet](https://parquet.apache.org/docs/) ファイル形式を使用します。 JSON ファイル形式の使用例については、『[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)』を参照してください。

**応答**

成功した応答は、HTTP Status 201（作成済み）と、新しく作成されたデータセットの ID を `"@/datasets/{DATASET_ID}"` 形式で含む配列で構成される応答オブジェクトを返します。データセット ID は、API 呼び出しでデータセットを参照するために使用される、読み取り専用のシステム生成文字列です。

```JSON
[
    "@/dataSets/5c8c3c555033b814b69f947f"
]
```
