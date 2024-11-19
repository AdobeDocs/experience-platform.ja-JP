---
title: 監査イベント API エンドポイント
description: Audit Query API を使用してExperience Platform内の監査イベントを取得する方法について説明します。
role: Developer
feature: Audits, API
exl-id: c365b6d8-0432-41a5-9a07-44a995f69b7d
source-git-commit: c0eb5b5c3a1968cae2bc19b7669f70a97379239b
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 20%

---

# 監査イベントエンドポイント

監査ログは、様々なサービスや機能に関するユーザーアクティビティの詳細を提供するために使用されます。 ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーの電子メール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。[!DNL Audit Query] API の `/audit/events` エンドポイントを使用すると、組織のアクティビティのイベントデータを [!DNL Platform] でプログラムによって取得できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Audit Query] API](https://developer.adobe.com/experience-platform-apis/references/audit-query/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 監査イベントのリスト

イベントデータを取得するには、`/audit/events` エンドポイントに対してGETリクエストを実行し、取得するイベントをペイロードで指定します。

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
     "customerAuditLogList": [
       {
         "userEmail": "{USER_ID}",
         "userIpAddresses": [ ],
         "eventType": "Core",
         "id": "32b72208-3035-4bc6-b434-39e34401a864",
         "version": "1.0",
         "imsOrgId": "{ORGANIZATION_ID}",
         "sandboxName": "prod",
         "region": "VA7",
         "requestId": "5NphpgUQdQnjTWOcS9DSMs2wD1EUMlYG",
         "authId": "96715f98-d100-4575-8491-ebbcea654eb9",
         "permissionResource": "Sandbox",
         "permissionType": "RESET",
         "assetType": "Sandbox",
         "assetId": "prod",
         "assetName": "prod",
         "action": "Reset",
         "status": "Allow",
         "failureCode": "",
         "timestamp": "2021-08-04T21:58:09.745+0000"
       },
       {
         "userEmail": "{USER_ID}",
         "userIpAddresses": [ ],
         "eventType": "Core",
         "id": "a178736a-8fa1-47da-bac5-b0d9e741e414",
         "version": "1.0",
         "imsOrgId": "{ORGANIZATION_ID}",
         "sandboxName": "prod",
         "region": "VA7",
         "requestId": "7AlGIAhWvaEzYWHLzvuf26AAFAkqSyKg",
         "authId": "60fc1077-4aef-4e1f-a5ff-f64183e060f4",
         "permissionResource": "Sandbox",
         "permissionType": "RESET",
         "assetType": "Sandbox",
         "assetId": "prod",
         "assetName": "prod",
         "action": "Reset",
         "status": "Allow",
         "failureCode": "",
         "timestamp": "2021-08-04T21:28:00.301+0000"
       },
       {
         "userEmail": "{USER_ID}",
         "userIpAddresses": [ ],
         "eventType": "Core",
         "id": "ccfe8c77-9b93-481d-a561-0b2edf3b77dc",
         "version": "1.0",
         "imsOrgId": "{ORGANIZATION_ID}",
         "sandboxName": "prod",
         "region": "VA7",
         "requestId": "hArqS4CAa8wfRPnKuxV4yaA82atxwzYu",
         "authId": "80b7d887-9338-4cd5-9d79-2483b03f0160",
         "permissionResource": "Sandbox",
         "permissionType": "RESET",
         "assetType": "Sandbox",
         "assetId": "prod",
         "assetName": "prod",
         "action": "Reset",
         "status": "Allow",
         "failureCode": "",
         "timestamp": "2021-08-04T20:58:07.750+0000"
       }
     ]    
   },
   "_links": {
     "self": {
       "href": "https://platform.adobe.io/data/foundation/audit/events?limit=10&start=0&property=type%253D%253Dcore"
     },
     "next": {
       "href": "https://platform.adobe.io/data/foundation/audit/events?queryId=cXVlcnlJZD0xYjA4MDM4MV81ZWNkXzRjNTZfYTM2N18zYWExOWI5YzNhNTlfMTYyODExNDY5MTg1NSZ0b3RhbEVsZW1lbnRzPTI2&start=10&limit=10"
     },
     "page": {
       "href": "https://platform.adobe.io/data/foundation/audit/events?queryId=cXVlcnlJZD0xYjA4MDM4MV81ZWNkXzRjNTZfYTM2N18zYWExOWI5YzNhNTlfMTYyODExNDY5MTg1NSZ0b3RhbEVsZW1lbnRzPTI2&limit=10{&start}",
       "templated": true
     }
  },
  "page": {
    "size": 10,
    "totalElements": 3,
    "totalPages": 1,
    "number": 1
  },
  "queryId": "cXVlcnlJZD0xYjA4MDM4MV81ZWNkXzRjNTZfYTM2N18zYWExOWI5YzNhNTlfMTYyODExNDY5MTg1NSZ0b3RhbEVsZW1lbnRzPTI2"
}
```

| プロパティ | 説明 |
| --- | --- |
| `customerAuditLogList` | リクエストで指定された各イベントをオブジェクトが表す配列。 各オブジェクトには、フィルター設定に関する情報と返されたイベントデータが含まれます。 |
| `userEmail` | イベントを実行したユーザーの電子メール。 |
| `eventType` | イベントのタイプ。 イベントのタイプには、`Core` と `Enhanced` があります。 |
| `imsOrgId` | イベントが発生した組織の ID。 |
| `permissionResource` | 権限を付与した製品または機能がアクションを実行します。 リソースは次のいずれかになります。 <ul><li>`Activation` </li><li>`ActivationAssociation` </li><li>`AnalyticSource` </li><li>`AudienceManagerSource` </li><li>`BizibleSource` </li><li>`CustomerAttributeSource` </li><li>`Dataset` </li><li>`EnterpriseSource` </li><li>`LaunchSource` </li><li>`MarketoSource` </li><li>`ProductProfile` </li><li>`ProfileConfig` </li><li>`Sandbox` </li><li>`Schema` </li><li>`Segment` </li><li>`StreamingSource` </li></ul> |
| `permissionType` | アクションに関係する権限タイプ。 |
| `assetType` | アクションが実行された Platform リソースのタイプ。 |
| `assetId` | アクションが実行された Platform リソースの一意の ID。 |
| `assetName` | アクションが実行された Platform リソースの名前。 |
| `action` | イベントに対して記録されたアクションのタイプ。 アクションは、次のいずれかになります。 <ul><li>`Add` </li><li>`Create` </li><li>`Dataset activate` </li><li>`Dataset remove` </li><li>`Delete` </li><li>`Disable for profile` </li><li>`Enable` </li><li>`Enable for profile` </li><li>`Profile activate` </li><li>`Profile remove` </li><li>`remove` </li><li>`reset` </li><li>`segment activate` </li><li>`segment remove` </li><li>`update` </li></ul> |
| `status` | アクションのステータス。 ステータスは次のいずれかになります。 </li><li>`Allow` </li><li>`Deny` </li><li>`Failure` </li><li>`Success` </li></ul> |

{style="table-layout:auto"}
