---
description: Destination SDKを使用して、事前定義済みのファイル形式オプションとカスタムファイル名設定でAmazon S3 の宛先を設定する方法を説明します。
title: 事前定義済みのファイル形式オプションとカスタムファイル名設定を使用して、Amazon S3 の宛先を設定します。
exl-id: 0ecd3575-dcda-4e5c-af5c-247d4ea13fa1
source-git-commit: 45ba0db386f065206f89ed30bfe7b0c1b44f6173
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 8%

---

# 事前定義済みのファイル形式オプションとカスタムファイル名設定を使用した、[!DNL Amazon S3] の宛先の設定

## 概要 {#overview}

ここでは、Destination SDKを使用して、事前定義済みのデフォルトの [ ファイル形式オプション ](configure-file-formatting-options.md) カスタムの [ ファイル名設定 ](../../functionality/destination-configuration/batch-configuration.md#file-name-configuration) でAmazon S3 の宛先を設定する方法について説明します。

このページには、[!DNL Amazon S3] の宛先で使用できるすべての設定オプションが表示されます。 必要に応じて、以下の手順に示す設定を編集したり、設定の特定の部分を削除したりできます。

以下で使用されるパラメーターについて詳しくは、[Destinations SDK の設定オプション ](../../functionality/configuration-options.md) を参照してください。

## 前提条件 {#prerequisites}

以下に説明する手順に進む前に、[Destination SDKの概要 ](../../getting-started.md) ページを参照して、Adobe I/ODestination SDK資格情報および認証 API を使用するために必要なその他の前提条件について確認してください。

## 手順 1：サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、`/destination-server` エンドポイントを使用して [ サーバーとファイル設定を作成 ](../../authoring-api/destination-server/create-destination-server.md) します。

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しい宛先サーバー設定を作成します。
以下のペイロードには、Experience PlatformUI でユーザーが定義できる、事前定義されたデフォルトの [CSV ファイル形式 ](../../functionality/destination-server/file-formatting.md) 設定パラメーターを含む、汎用の [!DNL Amazon S3] 設定が含まれています。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-server \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "name": "Amazon S3 destination server with predefined default CSV options",
    "destinationServerType": "FILE_BASED_S3",
    "fileBasedS3Destination": {
        "bucket": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.bucketName}}"
        },
        "path": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.path}}"
        }
    },
    "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}'
```

リクエストが成功した場合は、設定の一意の ID （`instanceId`）を含む、新しい宛先サーバー設定が返されます。 この値は、次の手順で必要になるので保存します。

## 手順 2：宛先の構成の作成 {#create-destination-configuration}

前の手順で宛先サーバーとファイル形式設定を作成したら、`/destinations` API エンドポイントを使用して宛先設定を作成できるようになりました。

[ 手順 1](#create-server-file-configuration) のサーバー設定をこの宛先設定に接続するには、以下の API リクエストの `destinationServerId` 値を、[ 手順 1](#create-server-file-configuration) で宛先サーバーを作成する際に取得した `instanceId` 値に置き換えます。

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destinations
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations \
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
         "name":"bucketName",
         "title":"Enter the name of your Amazon S3 bucket",
         "description":"Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "pattern": "(?=^.{3,63}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])\\.)*([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])$)",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Enter the path to your S3 bucket folder",
         "description":"Enter the path to your S3 bucket folder",
         "type":"string",
         "isRequired":true,
         "pattern": "^[0-9a-zA-Z\\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\\/?)+$",
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
      "flowRunsSupported":true,
      "monitoringSupported":true,
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
         "EVERY_8_HOURS",
         "EVERY_12_HOURS",
         "ONCE"
      ],
      "defaultFrequency":"DAILY",
      "defaultStartTime":"00:00",
      "filenameConfig":{
         "allowedFilenameAppendOptions":[
            "SEGMENT_NAME",
            "DESTINATION_INSTANCE_ID",
            "DESTINATION_INSTANCE_NAME",
            "ORGANIZATION_NAME",
            "SANDBOX_NAME",
            "DATETIME",
            "CUSTOM_TEXT"
         ],
         "defaultFilenameAppendOptions":[
            "DATETIME"
         ],
         "defaultFilename":"%DESTINATION%_%SEGMENT_ID%"
      },
      "backfillHistoricalProfileData":true
   }
}'
```

リクエストが成功した場合は、設定の一意の ID （`instanceId`）を含む、新しい宛先設定が返されます。 宛先設定を更新するための HTTP リクエストを別途行う必要がある場合に、この値を必要に応じて保存します。

## 手順 3:Experience PlatformUI の確認 {#verify-ui}

上記の設定に基づいて、Experience Platformカタログには、使用する新しいプライベート宛先カードが表示されるようになりました。

![ 選択した宛先カードを含む宛先カタログページを示す画面録画。](../../assets/guides/batch/destination-card.gif)

以下の画像と録画では、[ ファイルベースの宛先のアクティベーションワークフロー ](../../../ui/activate-batch-profile-destinations.md) のオプションが、宛先設定で選択したオプションにどのように一致するかを確認してください。

宛先に関する詳細を入力する場合、設定で設定したカスタムデータフィールドが、どのようなフィールドで表示されるかを確認してください。

>[!TIP]
>
>カスタムデータフィールドを宛先設定に追加した順序は、UI には反映されません。 顧客データフィールドは、常に、以下の画面録画で表示されている順序で表示されます。

![ 設定で定義された顧客データフィールドを示す画面録画。](../../assets/guides/batch/file-configuration-options.gif)

書き出し間隔をスケジュールする際には、`batchConfig` 設定で設定したフィールドが各フィールドにどのように表示されるかを確認してください。
![ スケジュール オプションのエクスポート ](../../assets/guides/batch/file-export-scheduling.png)

ファイル名の設定オプションを表示すると、設定で設定した `filenameConfig` のオプションが、表示されるフィールドでどのように表れているかに注意してください。
![ ファイル名設定オプション ](../../assets/guides/batch/file-naming-options.gif)

上記のフィールドを調整する場合は、[ 手順 1](#create-server-file-configuration) および [2](#create-destination-configuration) を繰り返し、必要に応じて設定を変更します。

## 手順 4:（オプション）宛先のPublish {#publish-destination}

>[!NOTE]
>
>自分で使用するためにプライベートな宛先を作成していて、他の顧客が使用できるように宛先カタログに公開する予定がない場合は、この手順は必要ありません。

宛先を設定した後、[destination publishing API](../../publishing-api/create-publishing-request.md) を使用して、設定をレビュー用にAdobeに送信します。

## 手順 5:（オプション）宛先のドキュメント化 {#document-destination}

>[!NOTE]
>
>自分で使用するためにプライベートな宛先を作成していて、他の顧客が使用できるように宛先カタログに公開する予定がない場合は、この手順は必要ありません。

独立系ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）で[製品化統合](../../overview.md#productized-custom-integrations)を作成する場合、[セルフサービスドキュメント化プロセス](../../docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを [Experience Platform 宛先カタログ](../../../catalog/overview.md)に作成します。

## 次の手順 {#next-steps}

この記事を読むことで、Destination SDKを使用したカスタム [!DNL Amazon S3] 宛先の作成方法を理解しました。 次に、チームは [ ファイルベース宛先のアクティベーションワークフロー ](../../../ui/activate-batch-profile-destinations.md) を使用して、宛先にデータを書き出すことができます。
