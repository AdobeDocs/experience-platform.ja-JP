---
description: このページでは、「/authoring/destinations/publish」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 公開先 API エンドポイントの操作
exl-id: 0564a132-42f4-478c-9197-9b051acf093c
source-git-commit: 702a5b7154724faa9f5e6847b462e0ae90475571
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 5%

---

# 公開先エンドポイント API の操作 {#publish-destination}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destinations/publish`

このページでは、 `/authoring/destinations/publish` API エンドポイント。

宛先を設定およびテストしたら、レビューおよび公開用にAdobeに送信できます。 読み取り [送信してレビュー用に、Destination SDKで作成した宛先を送信](./submit-destination.md) 宛先の送信プロセスの一環として実行する必要があるその他のすべての手順については、を参照してください。

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
   "destinationAccess":"ALL"
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信する宛先設定の宛先 ID。 を使用して、宛先設定の宛先 ID を取得する [宛先設定 API リファレンス](./destination-configuration-api.md#retrieve-list). |
| `destinationAccess` | 文字列 | 用途 `ALL` を設定します。 |

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
         "configId":"123cs780-ce29-434f-921e-4ed6ec2a6c35",
         "allowedOrgs": [
            "*"
         ],    
         "status":"PUBLISHED",
         "destinationType": "PUBLIC",
         "publishedDate":"1630617746"
      }
   ]
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信した宛先設定の宛先 ID。 |
| `publishDetailsList.configId` | 文字列 | 送信された宛先の宛先公開リクエストの一意の ID。 |
| `publishDetailsList.allowedOrgs` | 文字列 | 宛先が使用可能なExperience Platform組織を返します。 <br> <ul><li> の場合 `"destinationType": "PUBLIC"`の場合、このパラメーターは `"*"`：宛先は、すべてのExperience Platform組織で使用できます。</li><li> の場合 `"destinationType": "DEV"`の場合、このパラメーターは宛先の作成およびテストに使用した組織の組織 ID を返します。</li></ul> |
| `publishDetailsList.status` | 文字列 | 宛先の公開リクエストのステータス。 指定できる値は次のとおりです。 `TEST`, `REVIEW`, `APPROVED`, `PUBLISHED`, `DENIED`, `REVOKED`, `DEPRECATED`. 値を持つ宛先 `PUBLISHED` は実稼動環境であり、Experience Platformのお客様が使用できます。 |
| `publishDetailsList.destinationType` | 文字列 | 宛先のタイプ。 値は `DEV` および `PUBLIC`. `DEV` は、Experience Platform組織の宛先に対応します。 `PUBLIC` は、公開用に送信した宛先に対応します。 Git では、これら 2 つのオプションを考えて、 `DEV` バージョンは、ローカルのオーサリングブランチを表し、 `PUBLIC` version は、リモートメインブランチを表します。 |
| `publishDetailsList.publishedDate` | 文字列 | 発行のために宛先が送信された日付（エポック時間）。 |

{style=&quot;table-layout:auto&quot;}

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
         "configId":"ab41387c0-4772-4709-a3ce-6d5fee654520",
         "allowedOrgs":[
            "716543205DB85F7F0A495E5B@AdobeOrg"
         ],
         "status":"TEST",
         "destinationType":"DEV"
      },
      {
         "configId":"cd568c67-f25e-47e4-b9a2-d79297a20b27",
         "allowedOrgs":[
            "*"
         ],
         "status":"DEPRECATED",
         "destinationType":"PUBLIC",
         "publishedDate":1630525501009
      },
      {
         "configId":"ef6f07154-09bc-4bee-8baf-828ea9c92fba",
         "allowedOrgs":[
            "*"
         ],
         "status":"PUBLISHED",
         "destinationType":"PUBLIC",
         "publishedDate":1630531586002
      }
   ]
}
```

## API エラー処理

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順

このドキュメントを読んだ後、宛先の公開リクエストを送信する方法がわかりました。 Adobe Experience Platformチームが公開リクエストを確認し、5 営業日で連絡します。
