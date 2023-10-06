---
title: Quota API エンドポイント
description: データ衛生 API の/quota エンドポイントを使用すると、高度なデータライフサイクル管理の使用状況を、組織の各ジョブタイプの月別の割当制限に照らして監視できます。
exl-id: 91858a13-e5ce-4b36-a69c-9da9daf8cd66
source-git-commit: 566f1b6478cd0de0691cfb2301d5b86fbbfece52
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 58%

---

# Quota エンドポイント

The `/quota` Data Whealthy API のエンドポイントを使用すると、組織の各ジョブタイプの割り当て制限に照らして、Advanced data lifecycle management の使用状況を監視できます。

クォータは、次の方法で、各データライフサイクルジョブの種類に対して適用されます。

* レコードの削除と更新は、1 ヶ月あたりのリクエストの数に制限されます。
* データセットの有効期限には、有効期限が実行されるタイミングに関係なく、同時にアクティブなジョブの数に対して一定の制限があります。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、[概要](./overview.md)で以下の情報を確認してください。

* 関連ドキュメントへのリンク
* このドキュメントのサンプル API 呼び出しの読み方に関するガイド
* Experience Platform API を正常に呼び出すために必要な必須ヘッダーに関する重要な情報

## 割り当て量のリスト {#list}

`/quota` エンドポイントに対して GET リクエストを実行することで、組織の割り当て量の情報を表示できます。

**API 形式**

```http
GET /quota
GET /quota?quotaType={QUOTA_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUOTA_TYPE}` | 取得する割り当て量のタイプを指定するオプションのクエリパラメーター。`quotaType` パラメーターが指定されていない場合、すべての割り当て量の値が API 応答で返されます。使用できるタイプの値は次のとおりです。<ul><li>`expirationDatasetQuota`：データセット有効期限</li><li>`deleteIdentityWorkOrderDatasetQuota`: レコード削除</li><li>`fieldUpdateWorkOrderDatasetQuota`：レコードの更新</li></ul> |

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

正常な応答は、データライフサイクルクォータの詳細を返します。

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
      "description": "The number of Record Delete Work Order requests for the organization for this month.",
      "consumed": 390,
      "quota": 10000
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `quotas` | 各データライフサイクルジョブの種類のクォータ情報を表示します。 各割り当て量のオブジェクトには、次のプロパティが含まれます。<ul><li>`name`：データライフサイクルジョブのタイプ。<ul><li>`expirationDatasetQuota`：データセット有効期限</li><li>`deleteIdentityWorkOrderDatasetQuota`: レコード削除</li></ul></li><li>`description`：データライフサイクルジョブタイプの説明。</li><li>`consumed`：今月実行されたこのタイプのジョブの数。</li><li>`quota`：このジョブタイプの割り当て量の制限。レコードの削除および更新の場合、これは、各月の期間に実行できるジョブの数を表します。 データセットの有効期限の場合は、任意の時点で同時にアクティブにできるジョブの数を表します。</li></ul> |

{style="table-layout:auto"}
