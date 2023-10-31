---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、既存の宛先設定を更新するために使用される API 呼び出しの例を示します。
title: 宛先設定の更新
exl-id: d7f18689-9806-4f73-a63a-fa112569819c
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 100%

---

# 宛先設定の更新

このページでは、`/authoring/destinations` API エンドポイントを使用して、既存の宛先設定を更新するために使用できる API リクエストおよびペイロードの例を示します。

>[!TIP]
>
>製品化された宛先や公開されている宛先に対する更新操作は、[公開 API](../../publishing-api/create-publishing-request.md) を使用してアドビレビュー用に更新を送信した後でのみ、表示されます。

宛先設定の機能について詳しくは、以下の記事を参照してください。

* [顧客認証設定](../../functionality/destination-configuration/customer-authentication.md)
* [OAuth 2 認証](../../functionality/destination-configuration/oauth2-authentication.md)
* [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md)
* [UI 属性](../../functionality/destination-configuration/ui-attributes.md)
* [スキーマ設定](../../functionality/destination-configuration/schema-configuration.md)
* [ID 名前空間設定](../../functionality/destination-configuration/identity-namespace-configuration.md)
* [宛先配信](../../functionality/destination-configuration/destination-delivery.md)
* [オーディエンスメタデータ設定](../../functionality/destination-configuration/audience-metadata-configuration.md)
* [オーディエンスメタデータ設定](../../functionality/destination-configuration/audience-metadata-configuration.md)
* [集計ポリシー](../../functionality/destination-configuration/aggregation-policy.md)
* [バッチ設定](../../functionality/destination-configuration/batch-configuration.md)
* [プロファイル選定履歴](../../functionality/destination-configuration/historical-profile-qualifications.md)

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 宛先設定 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 宛先設定の更新 {#update}

更新されたペイロードで `/authoring/destinations` エンドポイントに `PUT` リクエストを行うことで、[既存の](create-destination-configuration.md)宛先設定を更新できます。

>[!TIP]
>
>API エンドポイント：`platform.adobe.io/data/core/activation/authoring/destinations`

既存の宛先設定およびその関連する `{INSTANCE_ID}` を取得するには、[宛先設定の取得](retrieve-destination-configuration.md)に関する記事を参照してください。

**API 形式**

```http
PUT /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先設定の ID。既存の宛先設定およびその関連する `{INSTANCE_ID}` を取得するには、[宛先設定の取得](retrieve-destination-configuration.md)を参照してください。 |

+++リクエスト

以下のリクエストは、[この例](create-destination-configuration.md#create)で作成した宛先を別の `filenameConfig` オプションで更新します。

```shell {line-numbers="true" highlight="115-128"}
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Amazon S3 destination with predefined CSV formatting options",
   "description":"Amazon S3 destination with predefined CSV formatting options",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
      {
         "authType":"S3"
      }
   ],
   "customerEncryptionConfigurations":[
       
   ],
   "customerDataFields":[
      {
         "name":"bucket",
         "title":"Enter the name of your Amazon S3 bucket",
         "description":"Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Enter the path to your S3 bucket folder",
         "description":"Enter the path to your S3 bucket folder",
         "type":"string",
         "isRequired":true,
         "pattern":"^[A-Za-z]+$",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"compression",
         "title":"Compression format",
         "description":"Select the desired file compression format.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "enum":[
            "SNAPPY",
            "GZIP",
            "DEFLATE",
            "NONE"
         ]
      },
      {
         "name":"fileType",
         "title":"File type",
         "description":"Select the exported file type.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false,
         "enum":[
            "csv",
            "json",
            "parquet"
         ],
         "default":"csv"
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-amazon-s3-en",
      "category":"cloudStorage",
      "icon":{
         "key":"amazonS3"
      },
      "connectionType":"S3",
      "frequency":"Batch"
   },
   "destinationDelivery":[
      {
         "deliveryMatchers":[
            {
               "type":"SOURCE",
               "value":[
                  "batch"
               ]
            }
         ],
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}"
      }
   ],
   "schemaConfig":{
      "profileRequired":true,
      "segmentRequired":true,
      "identityRequired":true
   },
   "batchConfig":{
      "allowMandatoryFieldSelection":true,
      "allowDedupeKeyFieldSelection":true,
      "defaultExportMode":"DAILY_FULL_EXPORT",
      "allowedExportMode":[
         "DAILY_FULL_EXPORT",
         "FIRST_FULL_THEN_INCREMENTAL"
      ],
      "allowedScheduleFrequency":[
         "DAILY",
         "EVERY_3_HOURS",
         "EVERY_6_HOURS",
         "ONCE"
      ],
      "defaultFrequency":"DAILY",
      "defaultStartTime":"00:00",
      "filenameConfig":{
         "allowedFilenameAppendOptions":[
            "SEGMENT_NAME",
            "DESTINATION_INSTANCE_NAME",
            "ORGANIZATION_NAME",
            "SANDBOX_NAME",
            "DATETIME",
            "CUSTOM_TEXT"
         ],
         "defaultFilenameAppendOptions":[
            "DATETIME"
         ],
         "defaultFilename":"%DESTINATION%_%SEGMENT_ID%_%DESTINATION_INSTANCE_ID%,"
      },
      "backfillHistoricalProfileData":true
   }
}'
```

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、更新された宛先設定の詳細と共に返されます。

+++

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、Destination SDK `/authoring/destinations` API エンドポイントを使用した、宛先設定の更新方法を確認しました。

このエンドポイントでできることについて詳しくは、以下の記事を参照してください。

* [宛先設定の作成](create-destination-configuration.md)
* [宛先設定の取得](retrieve-destination-configuration.md)
* [宛先設定の削除](delete-destination-configuration.md)
