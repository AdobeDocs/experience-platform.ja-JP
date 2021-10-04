---
description: このページでは、「/authoring/destinations/publish」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 公開先 API エンドポイントの操作
exl-id: 0564a132-42f4-478c-9197-9b051acf093c
source-git-commit: 9be8636b02a15c8f16499172289413bc8fb5b6f0
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 5%

---

# 公開先エンドポイント API の操作 {#publish-destination}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destinations/publish`

このページでは、`/authoring/destinations/publish` API エンドポイントを使用して実行できるすべての API 操作について説明します。

宛先の設定とテストが完了したら、レビューおよび公開用にAdobeに送信できます。

公開先 API エンドポイントを使用して、次の場合に公開リクエストを送信します。
* 宛先 SDK パートナーは、製品化された宛先を、すべてのExperience Platform組織で、使用するすべてのExperience Platformの顧客が利用できるようにします。
* カスタムの宛先を、すべてのサンドボックスで、独自のExperience Platform組織で使用できるようにする。

## 宛先公開 API 操作の概要 {#get-started}

続行する前に、[ はじめに ](./getting-started.md) を参照して、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、API を正しく呼び出すために知っておく必要がある重要な情報を確認してください。

## 公開用の宛先設定の送信 {#create}

`/authoring/destinations/publish` エンドポイントにPOSTリクエストを送信することで、公開用の宛先設定を送信できます。

**API 形式**


```http
POST /authoring/destinations/publish
```

**リクエスト**

次のリクエストでは、ペイロードで指定されたパラメーターで設定された組織全体に対して、公開用の宛先が送信されます。 以下のペイロードには、`/authoring/destinations/publish` エンドポイントが受け入れるすべてのパラメーターが含まれています。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations/publish \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "destinationAccess":"LIMITED",
   "allowedOrgs":[
      "xyz@AdobeOrg",
      "lmn@AdobeOrg"
   ]
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信する宛先設定の宛先 ID。 [ 宛先設定 API リファレンス ](./destination-configuration-api.md#retrieve-list) を使用して、宛先設定の宛先 ID を取得します。 |
| `destinationAccess` | 文字列 | `ALL` または `LIMITED`.宛先をすべてのExperience Platform顧客のカタログに表示するか、特定の組織のみに表示するかを指定します。<br> **注意**:を使用する場 `LIMITED`合、宛先はExperience Platform組織に対してのみ公開されます。顧客テストのために宛先をExperience Platform組織のサブセットに公開する場合は、Adobeサポートにお問い合わせください。 |
| `allowedOrgs` | 文字列 | `"destinationAccess":"LIMITED"` を使用する場合は、宛先を使用できるExperience Platform組織を指定します。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 201 と、宛先の公開リクエストの詳細を返します。

## 宛先の公開リクエストのリスト {#retrieve-list}

IMS 組織用に公開用に送信されたすべての宛先のリストを取得するには、`/authoring/destinations/publish` エンドポイントにGETリクエストをおこないます。

**API 形式**


```http
GET /authoring/destinations/publish
```

**リクエスト**

次のリクエストは、IMS 組織とサンドボックス設定に基づいて、アクセス権のある公開用に送信された宛先のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/publish \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある公開用に送信された宛先のリストを返します。 1 つの `configId` は、1 つの宛先の公開要求に対応します。

```json
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "publishDetailsList":[
      {
         "configId":"string",
         "allowedOrgs":[
            "xyz@AdobeOrg",
            "lmn@AdobeOrg"
         ],
         "status":"TEST",
         "publishedDate":"1630617746"
      }
   ]
}
    
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信した宛先設定の宛先 ID。 |
| `publishDetailsList.configId` | 文字列 | 送信先の発行リクエストの一意の ID。 |
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先を使用できるExperience Platform組織を返します。 |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。 指定できる値は、`TEST`、`REVIEW`、`APPROVED`、`PUBLISHED`、`DENIED`、`REVOKED`、`DEPRECATED` です。 |
| `publishDetailsList.publishedDate` | 文字列 | 発行の宛先が送信された日付（エポックタイム）。 |

{style=&quot;table-layout:auto&quot;}

## 既存の宛先公開リクエストの更新 {#update}

既存の宛先公開リクエストで許可された組織を更新するには、`/authoring/destinations/publish` エンドポイントにPUTリクエストを送信し、許可された組織を更新する宛先の ID を指定します。 呼び出しの本文で、更新された許可された組織を指定します。

**API 形式**


```http
PUT /authoring/destinations/publish/{DESTINATION_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | 公開リクエストを更新する宛先の ID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された、既存の宛先公開リクエストを更新します。 以下の呼び出しの例では、許可されている組織を更新しています。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/destinations/publish/1230e5e4-4ab8-4655-ae1e-a6296b30f2ec \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "destinationAccess":"LIMITED",
   "allowedOrgs":[
      "abc@AdobeOrg",
      "def@AdobeOrg"
   ]
}
```

## 特定の宛先の公開要求のステータスの取得 {#get}

`/authoring/destinations/publish` エンドポイントにGETリクエストを送信し、公開ステータスを取得する宛先の ID を指定することで、特定の宛先の公開リクエストに関する詳細な情報を取得できます。

**API 形式**


```http
GET /authoring/destinations/publish/{DESTINATION_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | 公開ステータスを取得する宛先の ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/publish/1230e5e4-4ab8-4655-ae1e-a6296b30f2ec \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された宛先の公開リクエストに関する詳細情報を返します。

```json
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "publishDetailsList":[
      {
         "configId":"string",
         "allowedOrgs":[
            "xyz@AdobeOrg",
            "lmn@AdobeOrg"
         ],
         "status":"TEST",
         "publishedDate":"string"
      }
   ]
}
```

## API エラー処理

宛先 SDK API エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 Platform トラブルシューティングガイドの [API ステータスコード ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes) および [ リクエストヘッダーのエラー ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors) を参照してください。

## 次の手順

このドキュメントを読むと、宛先に対して発行リクエストを送信する方法がわかります。 Adobe Experience Platformチームが公開リクエストを確認し、5 営業日で返信します。
