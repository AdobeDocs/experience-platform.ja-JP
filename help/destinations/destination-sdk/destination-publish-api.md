---
description: このページでは、「/authoring/destinations/publish」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 公開先 API エンドポイントの操作
exl-id: 0564a132-42f4-478c-9197-9b051acf093c
source-git-commit: 6dd8a94e46b9bee6d1407e7ec945a722d8d7ecdb
workflow-type: tm+mt
source-wordcount: '757'
ht-degree: 5%

---

# 公開先エンドポイント API の操作 {#publish-destination}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destinations/publish`

このページでは、 `/authoring/destinations/publish` API エンドポイント。

宛先を設定およびテストしたら、レビューおよび公開用にAdobeに送信できます。

公開リクエストを送信するには、次の場合に公開先 API エンドポイントを使用します。
* Destination SDKパートナーは、製品化された宛先をすべてのExperience Platform組織で利用できるようにし、すべてのExperience Platformのお客様が使用できるようにしたいと考えます。
* すべてのサンドボックスにわたって、独自のExperience Platform組織でカスタムの宛先を使用できるようにする。

## 宛先公開 API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## 公開用の宛先設定を送信 {#create}

公開用の宛先設定を送信するには、 `/authoring/destinations/publish` endpoint.

**API 形式**


```http
POST /authoring/destinations/publish
```

**リクエスト**

次のリクエストでは、ペイロードで指定されたパラメーターで設定された組織全体に対して、公開用の宛先を送信します。 以下のペイロードには、 `/authoring/destinations/publish` endpoint.

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
| `destinationId` | 文字列 | 公開用に送信する宛先設定の宛先 ID。 を使用して、宛先設定の宛先 ID を取得する [宛先設定 API リファレンス](./destination-configuration-api.md#retrieve-list). |
| `destinationAccess` | 文字列 | `ALL` または `LIMITED`.宛先をすべてのExperience Platform顧客のカタログに表示するか、特定の組織のみに表示するかを指定します。 <br> **注意**:次を使用する場合、 `LIMITED`の場合、宛先はExperience Platform組織に対してのみ公開されます。 顧客テストのために宛先をExperience Platform組織のサブセットに公開する場合は、Adobeサポートにお問い合わせください。 |
| `allowedOrgs` | 文字列 | 次を使用する場合、 `"destinationAccess":"LIMITED"`「 」で、宛先を使用できるExperience Platform組織を指定します。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 201 と、宛先の公開リクエストの詳細を返します。

## 宛先の公開リクエストのリスト {#retrieve-list}

IMS 組織に対して公開用に送信されたすべての宛先のリストを取得するには、にGETリクエストをおこないます `/authoring/destinations/publish` endpoint.

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

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある公開用に送信された宛先のリストを返します。 1 `configId` は、1 つの宛先の公開要求に対応します。

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
| `publishDetailsList.configId` | 文字列 | 送信された宛先の宛先公開リクエストの一意の ID。 |
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先を使用できるExperience Platform組織を返します。 |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。 指定できる値は次のとおりです。 `TEST`, `REVIEW`, `APPROVED`, `PUBLISHED`, `DENIED`, `REVOKED`, `DEPRECATED`. |
| `publishDetailsList.publishedDate` | 文字列 | 発行のために宛先が送信された日付（エポック時間）。 |

{style=&quot;table-layout:auto&quot;}

## 既存の宛先公開リクエストの更新 {#update}

PUTリクエストを `/authoring/destinations/publish` エンドポイントを作成し、許可された組織を更新する宛先の ID を指定します。 呼び出しの本文で、更新された許可された組織を指定します。

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

## 特定の宛先の公開リクエストのステータスの取得 {#get}

特定の宛先の公開リクエストに関する詳細な情報を取得するには、GETリクエストを `/authoring/destinations/publish` エンドポイントを検索し、公開ステータスを取得する宛先の ID を指定します。

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

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順

このドキュメントを読んだ後、宛先の公開リクエストを送信する方法がわかりました。 Adobe Experience Platformチームが公開リクエストを確認し、5 営業日で連絡します。
