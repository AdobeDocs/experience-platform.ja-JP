---
title: 監査イベント書き出し API エンドポイント
description: Audit Query API を使用してExperience Platformに監査イベントを書き出す方法について説明します。
role: Developer
feature: Audits, API
exl-id: 76c5de76-e391-4258-afd8-ddb2c8a9443f
source-git-commit: c0eb5b5c3a1968cae2bc19b7669f70a97379239b
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 4%

---

# 監査イベントのリストのエクスポート

イベントデータを取得するには、`/audit/export` エンドポイントに対してGETリクエストを実行し、取得するイベントをペイロードで指定します。

**API 形式**

```http
GET /audit/export
```

| パラメーター | 説明 |
| --------- | ----------- |
| `timestamp` | タイムスタンプでフィルタリングする場合は、正確な値ではなく、> および &lt; 演算子を使用して範囲を使用することをお勧めします。 <br/> 例：`?property=timestamp<2020-02-08T02:46:48.610862Z&property=timestamp>2020-01-01T02:46:48.610862Z`。 |
| `status` | アクションのステータス。 ステータスは次のいずれかになります。 </li><li>`Allow` </li><li>`Deny` </li><li>`Failure` </li><li>`Success` </li></ul><br/> 例：`?property=status==Deny`。 |
| `action` | イベントに対して記録されたアクションのタイプ。 アクションは、次のいずれかになります。 <ul><li>`Add` </li><li>`Create` </li><li>`Dataset activate` </li><li>`Dataset remove` </li><li>`Delete` </li><li>`Disable for profile` </li><li>`Enable` </li><li>`Enable for profile` </li><li>`Profile activate` </li><li>`Profile remove` </li><li>`Remove` </li><li>`Reset` </li><li>`Segment Activate` </li><li>`Segment remove` </li><li>`Update` </li></ul> 例：`?property=action==Create`。 |
| `user` | イベントを実行したユーザー。 |
| `assetType` | アクションが実行された Platform リソースのタイプ。 <br/> 例：`?property=assetType==<an asset type>`。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/audit/events
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {TRACING_ID}' \
```

**応答**

結果は、書き出し用に CSV ファイルに生成されます。 応答が成功すると、応答本文なしで HTTP 307 が返されます。 書き出しファイルへのリンクが、`Location` 応答ヘッダーに提供されます。
