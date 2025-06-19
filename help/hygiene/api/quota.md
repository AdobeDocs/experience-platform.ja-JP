---
title: Quota API エンドポイント
description: Data Hygiene API の/quota エンドポイントを使用すると、各ジョブタイプの組織の月間割り当て量制限に対する高度なデータライフサイクル管理の使用状況を監視できます。
role: Developer
exl-id: 91858a13-e5ce-4b36-a69c-9da9daf8cd66
source-git-commit: 4d34ae1885f8c4b05c7bb4ff9de9c0c0e26154bd
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 22%

---

# Quota エンドポイント

Data Hygiene API の `/quota` エンドポイントを使用すると、各ジョブタイプの組織の割り当て量制限に対する高度なデータライフサイクル管理の使用状況を監視できます。

クォータの使用状況は、データ ライフサイクル ジョブの種類ごとに追跡されます。 実際の割り当て量の制限は、組織の使用権限によって異なり、定期的に確認する場合があります。 データセットの有効期限は、同時にアクティブなジョブの数にハードリミットが適用されます。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、[概要](./overview.md)で以下の情報を確認してください。

* 関連ドキュメントへのリンク
* このドキュメントのサンプル API 呼び出しの読み方に関するガイド
* Experience Platform API を呼び出すために必要な必須ヘッダーに関する重要な情報

## 割り当て量と処理タイムライン {#quotas}

レコードの削除リクエストは、ライセンスの使用権限に基づいて、割り当て量とサービスレベルの期待に左右されます。 これらの制限は、UI ベースの削除リクエストと API ベースの削除リクエストの両方に適用されます。

>[!TIP]
> 
>このドキュメントでは、使用権限ベースの制限に対して使用状況をクエリする方法を説明します。 割り当て量階層、レコード削除の使用権限、SLAの動作について詳しくは、[UI ベースのレコード削除 ](../ui/record-delete.md#quotas) または [API ベースのレコード削除 ](./workorder.md#quotas) を参照してください。

## 割り当て量のリスト {#list}

`/quota` エンドポイントに対して GET リクエストを実行することで、組織の割り当て量の情報を表示できます。

**API 形式**

```http
GET /quota
GET /quota?quotaType={QUOTA_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUOTA_TYPE}` | 取得する割り当て量のタイプを指定するオプションのクエリパラメーター。`quotaType` パラメーターが指定されていない場合、すべての割り当て量の値が API 応答で返されます。使用できるタイプの値は次のとおりです。<ul><li>`datasetExpirationQuota`：このオブジェクトは、組織で同時にアクティブなデータセットの有効期限の数と、有効期限の合計許容量を表示します。 </li><li>`dailyConsumerDeleteIdentitiesQuota`：このオブジェクトには、組織が今日行ったレコード削除リクエストの合計数と、1 日あたりの合計使用量が表示されます。<br> 注意：許可されたリクエストのみがカウントされます。 作業指示が検証に失敗したために却下された場合、それらの ID 削除は割り当てにカウントされません。</li><li>`monthlyConsumerDeleteIdentitiesQuota`：このオブジェクトは、今月に組織が行ったレコード削除リクエストの合計数と月額使用料の合計を表示します。</li><li>`monthlyUpdatedFieldIdentitiesQuota`：このオブジェクトには、今月お客様の組織が行ったレコード更新リクエストの合計数と、お客様の月間使用許可総額が表示されます。</li></ul> |

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
      "quota": 1000000
    },
    {
      "name": "monthlyConsumerDeleteIdentitiesQuota",
      "description": "The consumed number of deleted identities in all workorder requests for the organization for this month.",
      "consumed": 841,
      "quota": 2000000
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
| -------- | ------- |
| `quotas` | 各データ ライフサイクル ジョブ タイプのクォータ情報を一覧表示します。 各割り当て量のオブジェクトには、次のプロパティが含まれます。<ul><li>`name`: データ ライフサイクル ジョブの種類：<ul><li>`expirationDatasetQuota`：データセット有効期限</li><li>`deleteIdentityWorkOrderDatasetQuota`: レコードの削除</li></ul></li><li>`description`: データ ライフサイクル ジョブ タイプの説明。</li><li>`consumed`：当期実行されたこのタイプのジョブの数。 オブジェクト名は、割り当て量の期間を示します。</li><li>`quota`：組織に対するこのジョブタイプの割り当て。 レコードの削除と更新の場合、割り当て量は、1 か月に実行できるジョブの数を表します。 データセット有効期限の場合、割り当て量は、任意の時点で同時にアクティブにできるジョブの数を表します。</li></ul> |
