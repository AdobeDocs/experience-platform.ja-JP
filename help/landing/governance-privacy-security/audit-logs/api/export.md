---
title: 監査イベント書き出し API エンドポイント
description: Audit Query API を使用して、監査イベントをExperience Platformに書き出す方法を説明します。
source-git-commit: 5b3459711f41430977f9d7b06f8b35801739207c
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 3%

---

# 監査イベントのリストの書き出し

イベントデータを取得するには、 `/audit/export` エンドポイントで、ペイロードで取得するイベントを指定します。

**API 形式**

```http
GET /audit/export
```

| パラメーター | 説明 |
| --------- | ----------- |
| `timestamp` | タイムスタンプでフィルタリングする場合、正確な値ではなく、> および &lt; 演算子を使用して範囲を使用することをお勧めします。 <br/>例：?property=timestamp&lt;2020-02-08T02:46:48.610862Z&amp;property=timestamp>2020-01-01T02:46:48.610862Z |
| `status` | アクションのステータス。 ステータスは、次のいずれかになります。 </li><li>`Allow` </li><li>`Deny` </li><li>`Failure` </li><li>`Success` </li></ul> |
| `action` | イベントに対して記録されたアクションのタイプ。 アクションは、次のいずれかになります。 <ul><li>`Add` </li><li>`Create` </li><li>`Dataset activate` </li><li>`Dataset remove` </li><li>`Delete` </li><li>`Disable for profile` </li><li>`Enable` </li><li>`Enable for profile` </li><li>`Profile activate` </li><li>`Profile remove` </li><li>`remove` </li><li>`reset` </li><li>`segment activate` </li><li>`segment remove` </li><li>`update` </li></ul> |
| `user` | イベントを実行したユーザー。 |
| `assetType` | アクションが実行された Platform リソースのタイプ。 |

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

結果は、書き出し用に CSV ファイルに生成されます。 正常な応答は、HTTP 307 と応答本文を返しません。 書き出しファイルへのリンクは、 `Location` 応答ヘッダー。
