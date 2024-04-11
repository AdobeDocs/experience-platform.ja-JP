---
title: Quota API エンドポイント
description: Data Hygiene API の/quota エンドポイントを使用すると、各ジョブタイプの組織の月間割り当て量制限に対する高度なデータライフサイクル管理の使用状況を監視できます。
role: Developer
exl-id: 91858a13-e5ce-4b36-a69c-9da9daf8cd66
source-git-commit: 48a83e2b615fc9116a93611a5e6a8e7f78cb4dee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 29%

---

# Quota エンドポイント

この `/quota` data Hygiene API のエンドポイントを使用すると、各ジョブタイプの組織の割り当て量制限に対する高度なデータライフサイクル管理の使用状況を監視できます。

クォータは、次の方法でデータ・ライフサイクル・ジョブ・タイプごとに適用されます。

* レコードの削除と更新は、1 か月あたり一定数のリクエストに制限されています。
* データセットの有効期限には、有効期限が実行されるタイミングに関係なく、同時にアクティブなジョブの数に対して一定の制限があります。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、[概要](./overview.md)で以下の情報を確認してください。

* 関連ドキュメントへのリンク
* このドキュメントのサンプル API 呼び出しの読み方に関するガイド
* Experience PlatformAPI を呼び出すために必要な必須ヘッダーに関する重要な情報

## 割り当て量のリスト {#list}

`/quota` エンドポイントに対して GET リクエストを実行することで、組織の割り当て量の情報を表示できます。

**API 形式**

```http
GET /quota
GET /quota?quotaType={QUOTA_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUOTA_TYPE}` | 取得する割り当て量のタイプを指定するオプションのクエリパラメーター。`quotaType` パラメーターが指定されていない場合、すべての割り当て量の値が API 応答で返されます。使用できるタイプの値は次のとおりです。<ul><li>`datasetExpirationQuota`：このオブジェクトは、組織で同時にアクティブなデータセットの有効期限の数と、有効期限の合計許容量を表示します。 </li><li>`dailyConsumerDeleteIdentitiesQuota`：このオブジェクトには、組織が今日行ったレコード削除リクエストの合計数と、1 日あたりの許容値の合計が表示されます。<br>メモ：許可されたリクエストのみがカウントされます。 作業指示が検証に失敗したために却下された場合、それらの ID 削除は割り当てにカウントされません。</li><li>`monthlyConsumerDeleteIdentitiesQuota`：このオブジェクトは、今月お客様の組織が行ったレコード削除リクエストの合計数と、お客様の月間合計使用量を表示します。</li><li>`monthlyUpdatedFieldIdentitiesQuota`：このオブジェクトは、今月お客様の組織が行ったレコード更新リクエストの合計数と、お客様の月間使用許可合計を表示します。</li></ul> |

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

応答が成功すると、データライフサイクルクォータの詳細が返されます。

```json
{
  "quotas": [
    {
      "name": "datasetExpirationQuota",
      "description": "The number of concurrently active Expiration Dataset Delete in all workorder requests for the organization.",
      "consumed": 12,
      "quota": 50
    },
    {
      "name": "dailyConsumerDeleteIdentitiesQuota",
      "description": "The consumed number of deleted identities in all workorder requests for the organization for today.",
      "consumed": 0,
      "quota": 600000
    },
    {
      "name": "monthlyConsumerDeleteIdentitiesQuota",
      "description": "The consumed number of deleted identities in all workorder requests for the organization for this month.",
      "consumed": 841,
      "quota": 600000
    },
    {
      "name": "monthlyUpdatedFieldIdentitiesQuota",
      "description": "The consumed number of updated identities in all workorder requests for the organization for this month.",
      "consumed": 0,
      "quota": 0
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `quotas` | 各データ ライフサイクル ジョブ タイプのクォータ情報を一覧表示します。 各割り当て量のオブジェクトには、次のプロパティが含まれます。<ul><li>`name`：データライフサイクルジョブのタイプ：<ul><li>`expirationDatasetQuota`：データセット有効期限</li><li>`deleteIdentityWorkOrderDatasetQuota`：レコードの削除</li></ul></li><li>`description`：データライフサイクルジョブタイプの説明。</li><li>`consumed`：当期に実行されたこのタイプのジョブの数。 オブジェクト名は、割り当て量の期間を示します。</li><li>`quota`：組織に対するこのジョブタイプの割り当て。 レコードの削除と更新の場合、割り当て量は、1 か月に実行できるジョブの数を表します。 データセット有効期限の場合、割り当て量は、任意の時点で同時にアクティブにできるジョブの数を表します。</li></ul> |

{style="table-layout:auto"}
