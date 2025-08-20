---
title: 監査イベント API エンドポイント
description: Audit Query API を使用してExperience Platformの監査イベントを取得する方法について説明します。
role: Developer
feature: Audits, API
exl-id: c365b6d8-0432-41a5-9a07-44a995f69b7d
source-git-commit: dec895e3ea625fb86d1891bad713185d39c47c81
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 20%

---

# 監査イベントエンドポイント

監査ログは、様々なサービスや機能に関するユーザーアクティビティの詳細を提供するために使用されます。 ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーの電子メール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。`/audit/events` API の [!DNL Audit Query] エンドポイントを使用すると、組織のアクティビティのイベントデータを [!DNL Experience Platform] でプログラムによって取得できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Audit Query] API](https://developer.adobe.com/experience-platform-apis/references/audit-query/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 監査イベントのリスト

イベントデータを取得するには、`/audit/events` エンドポイントに対してGET リクエストを実行し、取得するイベントをペイロードで指定します。

**API 形式**

```http
GET /audit/events
```

[!DNL Audit Query] API では、イベントをリストする際にクエリパラメーターを使用して結果をページおよびフィルターする機能をサポートしています。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 デフォルト `limit` は 50 です。 |
| `start` | 返される検索結果の最初の項目へのポインター。 結果の次のページにアクセスするには、このパラメーターを limit で示されるのと同じ量だけ増分する必要があります。 例：limit=50 を指定したリクエストの結果の次のページにアクセスするには、パラメーター start=50 を使用し、その後のページにはパラメーター start=100 を使用します。 |
| `queryId` | /audit/events エンドポイントに対してクエリを実行する場合、応答には queryId 文字列プロパティが含まれます。 同じクエリを別の呼び出しで実行するには、検索パラメーターを手動で再度設定する代わりに、ID 値を単一のクエリパラメーターとして含めることができます。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/audit/events?limit=10
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {TRACING_ID}' \
```

**応答**

応答が成功すると、リクエストで指定された指標とフィルターの結果のデータポイントが返されます。

```json
{
  "_embedded": {
    "events": [
      {
        "id": "6ecc125d-da03-4882-a944-88c707ddc3f7",
        "requestId": "5YGdpTX5PvRrdqCfrCT8p8lWphZPzxl8",
        "permissionResource": "Dataset",
        "permissionType": "WRITE",
        "assetType": "Dataset",
        "action": "Create",
        "status": "Allow",
        "failureCode": "",
        "timestamp": "2025-06-24T16:50:28.318+0000",
        "version": "1.0",
        "imsOrgId": "{ORGANIZATION_ID}",
        "region": "VA7",
        "authId": "e6b46821-e2b4-4729-952f-2e4afd713b31",
        "assetId": "685ad754fb1abe2b263df4b3",
        "assetName": "my-dataset",
        "sandboxName": "prod",
        "sandboxId": "{SANDBOX_ID}",
        "userEmail": "{USER_EMAIL}",
        "userIpAddresses": [
          "130.*.*.*",
          "10.*.*.*"
        ],
        "enhancedEvents": [
          {
            "id": "0ee91e42-ac46-4f35-a01a-f74a1569c404",
            "requestId": "5YGdpTX5PvRrdqCfrCT8p8lWphZPzxl8",
            "permissionResource": "Dataset",
            "permissionType": "Write",
            "assetType": "Dataset",
            "action": "Create",
            "status": "Success",
            "failureCode": "",
            "timestamp": "2025-06-24T16:50:28.883+0000",
            "assetId": "685ad754fb1abe2b263df4b3",
            "assetName": "my-dataset"
          }
        ]
      }
    ]
  },
  "_links": {
    "self": {
      "href": "https://platform.adobe.io/data/foundation/audit/events?property=user%253D%253Ddraghici%2540adobe.com"
    },
    "page": {
      "href": "https://platform.adobe.io/data/foundation/audit/events?queryId=b3JkZXJCeVJ1bGVzPSZwcm9wZXJ0eT11c2VyPT1kcmFnaGljaUBhZG9iZS5jb20mdGltZXN0YW1wSW5kZXg9MTc1MDc4MzgyODMxOCZ0b3RhbEVsZW1lbnRzPTE3&limit=50{&start}",
      "templated": true
    }
  },
  "page": {
    "size": 1,
    "totalElements": 1,
    "totalPages": 1,
    "number": 1
  },
  "queryId": "b3JkZXJCeVJ1bGVzPSZwcm9wZXJ0eT11c2VyPT1kcmFnaGljaUBhZG9iZS5jb20mdGltZXN0YW1wSW5kZXg9MTc1MDc4MzgyODMxOCZ0b3RhbEVsZW1lbnRzPTE3"
}
```

| プロパティ | 説明 |
| --- | --- |
| `events` | リクエストで指定された各イベントをオブジェクトが表す配列。 各オブジェクトには、フィルター設定に関する情報と返されたイベントデータが含まれます。 |
| `userEmail` | イベントを実行したユーザーの電子メール。 |
| `eventType` | イベントのタイプ。 イベントのタイプには、`Core` と `Enhanced` があります。 |
| `imsOrgId` | イベントが発生した組織の ID。 |
| `permissionResource` | 権限を付与した製品または機能がアクションを実行します。 リソースは次のいずれかになります。 <ul><li>`Activation` </li><li>`ActivationAssociation` </li><li>`AnalyticSource` </li><li>`AudienceManagerSource` </li><li>`BizibleSource` </li><li>`CustomerAttributeSource` </li><li>`Dataset` </li><li>`EnterpriseSource` </li><li>`LaunchSource` </li><li>`MarketoSource` </li><li>`ProductProfile` </li><li>`ProfileConfig` </li><li>`Sandbox` </li><li>`Schema` </li><li>`Segment` </li><li>`StreamingSource` </li></ul> |
| `permissionType` | アクションに関係する権限タイプ。 |
| `assetType` | アクションが実行されたExperience Platform リソースのタイプ。 |
| `assetId` | アクションが実行されたExperience Platform リソースの一意の ID。 |
| `assetName` | アクションが実行されたExperience Platform リソースの名前。 |
| `action` | イベントに対して記録されたアクションのタイプ。 アクションは、次のいずれかになります。 <ul><li>`Add` </li><li>`Create` </li><li>`Dataset activate` </li><li>`Dataset remove` </li><li>`Delete` </li><li>`Disable for profile` </li><li>`Enable` </li><li>`Enable for profile` </li><li>`Profile activate` </li><li>`Profile remove` </li><li>`remove` </li><li>`reset` </li><li>`segment activate` </li><li>`segment remove` </li><li>`update` </li></ul> |
| `status` | アクションのステータス。 ステータスは次のいずれかになります。 </li><li>`Allow` </li><li>`Deny` </li><li>`Failure` </li><li>`Success` </li></ul> |

{style="table-layout:auto"}
