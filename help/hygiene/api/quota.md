---
title: クォータ API エンドポイント
description: データ衛生 API の/quota エンドポイントを使用すると、組織の各ジョブタイプの月別クォータ制限に対するデータ衛生状態の使用状況を監視できます。
source-git-commit: 364ada0c354ddba8a855945f4f806f5600f21416
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 13%

---

# クォータエンドポイント

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

この `/quota` Data Whealthy API のエンドポイントを使用すると、組織の各ジョブタイプの割り当て制限に対するデータ衛生状態の使用状況を監視できます。

クォータは、次の方法で、各データの衛生ジョブの種類に対して適用されます。

* 消費者による削除およびフィールドの更新は、1 ヶ月あたりのリクエストの数に制限されます。
* データセット有効期限には、有効期限が実行されるタイミングに関係なく、同時にアクティブなジョブの数に対して一定の制限があります。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、 [概要](./overview.md) を参照してください。

* 関連ドキュメントへのリンク
* このドキュメントのサンプル API 呼び出しに関する読み方のガイド
* 任意のヘッダー API を正しく呼び出すために必要な必須Experience Platformに関する重要な情報

## 割り当てのリスト {#list}

組織のクォータ情報を表示するには、 `/quota` endpoint.

**API 形式**

```http
GET /quota
GET /quota?quotaType={QUOTA_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUOTA_TYPE}` | 取得するクォータの種類を指定するオプションのクエリパラメータです。 指定しない場合 `quotaType` パラメーターが指定された場合、すべてのクォータ値が API 応答で返されます。 使用できるタイプ値は次のとおりです。<ul><li>`expirationDatasetQuota`: データセット有効期限</li><li>`deleteIdentityWorkOrderDatasetQuota`:消費者の削除</li><li>`fieldUpdateWorkOrderDatasetQuota`:フィールドの更新</li></ul> |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/quota \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json'
```

**応答**

正常な応答は、データの衛生上の割り当ての詳細を返します。

```json
{
  "quotas": [
    {
      "name": "expirationDatasetQuota",
      "description": "The number of concurrently active Expiration Dataset Delete Work Order requests for the organization.",
      "consumed": 3154,
      "quota": 10000
    },
    {
      "name": "deleteIdentityWorkOrderQuota",
      "description": "The number of Consumer Delete Work Order requests for the organization for this month.",
      "consumed": 390,
      "quota": 10000
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `quotas` | 各データ衛生ジョブタイプのクォータ情報をリストします。 各クォータオブジェクトには、次のプロパティが含まれます。<ul><li>`name`:データ衛生ジョブのタイプ：<ul><li>`expirationDatasetQuota`: データセット有効期限</li><li>`deleteIdentityWorkOrderDatasetQuota`:消費者の削除</li></ul></li><li>`description`:データ衛生ジョブタイプの説明。</li><li>`consumed`:現在の月次期間に実行された、このタイプのジョブの数。</li><li>`quota`:このジョブタイプのクォータ制限。 消費者の削除およびフィールドの更新の場合、これは各月に実行できるジョブの数を表します。 データセット有効期限の場合、これは、任意の時点で同時にアクティブにできるジョブの数を表します。</li></ul> |

{style=&quot;table-layout:auto&quot;}
