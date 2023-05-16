---
description: このページでは、Adobe Experience Platform Destination SDKを通じて既存の宛先設定を更新するための API 呼び出しの例を示します。
title: 宛先設定の更新
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 28%

---


# 宛先設定の更新

このページでは、 `/authoring/destinations` API エンドポイント。

>[!TIP]
>
>製品化/公開先に対する更新操作は、 [公開 API](../../publishing-api/create-publishing-request.md) 「更新」を送信して、Adobeレビューを行います。

宛先設定の機能について詳しくは、次の記事を参照してください。

* [顧客認証設定](../../functionality/destination-configuration/customer-authentication.md)
* [OAuth 2 認証](../../functionality/destination-configuration/oauth2-authentication.md)
* [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md)
* [UI 属性](../../functionality/destination-configuration/ui-attributes.md)
* [スキーマ設定](../../functionality/destination-configuration/schema-configuration.md)
* [ID 名前空間の設定](../../functionality/destination-configuration/identity-namespace-configuration.md)
* [宛先配信](../../functionality/destination-configuration/destination-delivery.md)
* [オーディエンスメタデータの設定](../../functionality/destination-configuration/audience-metadata-configuration.md)
* [オーディエンスメタデータの設定](../../functionality/destination-configuration/audience-metadata-configuration.md)
* [集計ポリシー](../../functionality/destination-configuration/aggregation-policy.md)
* [バッチ設定](../../functionality/destination-configuration/batch-configuration.md)
* [プロファイル選定履歴](../../functionality/destination-configuration/historical-profile-qualifications.md)

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先設定 API 操作の概要 {#get-started}

続ける前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 宛先設定の更新 {#update}

以下の項目を更新し、 [既存](create-destination-configuration.md) 宛先の設定を行う `PUT` にリクエスト `/authoring/destinations` エンドポイントと、更新されたペイロード。

>[!TIP]
>
>API エンドポイント: `platform.adobe.io/data/core/activation/authoring/destinations`

既存の宛先設定とそれに対応する設定を取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [宛先設定の取得](retrieve-destination-configuration.md).

**API 形式**

```http
PUT /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先設定の ID。既存の宛先設定とそれに対応する設定を取得するには `{INSTANCE_ID}`を参照してください。 [宛先設定の取得](retrieve-destination-configuration.md). |

+++リクエスト

次のリクエストは、で作成した宛先を更新します。 [この例](create-destination-configuration.md#create) 異なる `filenameConfig` オプション。

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

正常な応答は、HTTP ステータス 200 と、更新された宛先設定の詳細を返します。

+++

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読んだ後、Destination SDKを使用して宛先設定を更新する方法がわかりました。 `/authoring/destinations` API エンドポイント。

このエンドポイントで実行できる操作について詳しくは、次の記事を参照してください。

* [宛先設定の作成](create-destination-configuration.md)
* [宛先設定の取得](retrieve-destination-configuration.md)
* [宛先設定の削除](delete-destination-configuration.md)