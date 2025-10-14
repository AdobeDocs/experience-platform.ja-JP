---
title: Quota API エンドポイント
description: Data Hygiene API の/quota エンドポイントを使用すると、各ジョブタイプの組織の月間割り当て量制限に対する高度なデータライフサイクル管理の使用状況を監視できます。
role: Developer
exl-id: 91858a13-e5ce-4b36-a69c-9da9daf8cd66
source-git-commit: 2c2907c5ed38ce4c87b1b73f96e1a0e64586cb97
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 23%

---

# Quota エンドポイント

Data Hygiene API の `/quota` エンドポイントを使用して、組織の現在の使用状況と制限を確認します。 クォータは、Privacy and Security Shield または Healthcare Shield などの使用権限によって異なります。

現在の割り当て量の消費を監視して、各ジョブタイプの組織制限への準拠を確認します。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、[概要](./overview.md)で以下の情報を確認してください。

* 関連ドキュメントのリンク
* サンプル API 呼び出しの読み方
* Experience Platform API 呼び出しに必要なヘッダー

## 割り当て量と処理タイムライン {#quotas}

レコードの削除リクエストは、ライセンスの使用権限に基づいて、割り当て量とサービスレベルの期待に左右されます。 これらの制限は、UI ベースの削除リクエストと API ベースの削除リクエストの両方に適用されます。

>[!TIP]
>
>このドキュメントでは、使用権限ベースの制限に対して使用状況をクエリする方法を説明します。 割当層、レコード削除の使用権限、SLAの動作について詳しくは、[UI ベースのレコード削除 &#x200B;](../ui/record-delete.md#quotas) または [API ベースのレコード削除 &#x200B;](./workorder.md#quotas) のドキュメントを参照してください。

## 割り当て量のリスト {#list}

`/quota` エンドポイントに対して GET リクエストを実行することで、組織の割り当て量の情報を表示できます。

**API 形式**

```http
GET /quota
GET /quota?quotaType={QUOTA_TYPE}
```

>[!NOTE]
>
>クォータの消費量は、毎日および毎月 1 日の 00:00 GMT にリセットされます。
>
>許可されたリクエストのみが割り当てにカウントされます。 却下された作業指示は、割り当てを減らしません。

| パラメーター | 説明 |
| --- | --- |
| `{QUOTA_TYPE}` | 取得する割り当て量のタイプを指定するオプションのクエリパラメーター。`quotaType` パラメーターが指定されていない場合、すべての割り当て量の値が API 応答で返されます。使用できる値は次のとおりです。<ul><li>`datasetExpirationQuota`: アクティブなデータセット有効期限の数と合計許容値。</li><li>`dailyConsumerDeleteIdentitiesQuota`：今日および 1 日の割り当て量のレコード削除数。</li><li>`monthlyConsumerDeleteIdentitiesQuota`：今月のレコード削除数と月間割り当て量。</li></ul> |

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
      "description": "The number of concurrently active dataset-expiration delete operations in all work order requests for the organization.",
      "consumed": 11,
      "quota": 75
    },
    {
      "name": "dailyConsumerDeleteIdentitiesQuota",
      "description": "The consumed number of deleted identities in all work order requests for the organization for today.",
      "consumed": 314,
      "quota": 700000
    },
    {
      "name": "monthlyConsumerDeleteIdentitiesQuota",
      "description": "The consumed number of deleted identities in all work order requests for the organization this month.",
      "consumed": 2764,
      "quota": 12000000
    }
  ]
}
```

| プロパティ | 説明 |
| -------- | ------- |
| `quotas` | 各データ ライフサイクル ジョブ タイプのクォータ情報を一覧表示します。 各割り当て量のオブジェクトには、次のプロパティが含まれます。<ul><li>`name`</li><li>`description`</li><li>`consumed`</li><li>`quota`</li></ul> |
| `name` | 割り当て量の種類の識別子。 |
| `description` | このクォータ制限の説明。 |
| `consumed` | 現在消費されている割り当て量。 消費金額は、その月の 1 日の 00:00 GMT と 00:00 GMT に日次割り当てにリセットされます。 |
| `quota` | 組織に対するこのクォータの種類の最大割り当て。 |
