---
description: Destination SDKを使用して、事前に定義されたファイル形式オプションとカスタムファイル名設定を使用してAmazon S3 の宛先を設定する方法について説明します。
title: 事前定義済みのファイル形式オプションとカスタムファイル名設定を使用した Amazon S3 の宛先の設定.
exl-id: 0ecd3575-dcda-4e5c-af5c-247d4ea13fa1
source-git-commit: d47c82339afa602a9d6914c1dd36a4fc9528ea32
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 12%

---

# の設定 [!DNL Amazon S3] 定義済みのファイル形式オプションとカスタムファイル名設定を使用した宛先

## 概要 {#overview}

このページでは、Destination SDKを使用して、事前に定義されたデフォルトのAmazon S3 の宛先を設定する方法について説明します [ファイル形式オプション](configure-file-formatting-options.md) そして習慣 [ファイル名設定](../../functionality/destination-configuration/batch-configuration.md#file-name-configuration).

このページには、 [!DNL Amazon S3] 宛先。 以下の手順で示す設定を編集したり、必要に応じて設定の特定の部分を削除したりできます。

以下で使用するパラメーターについて詳しくは、 [宛先 SDK の設定オプション](../../functionality/configuration-options.md).

## 前提条件 {#prerequisites}

以下の手順に進む前に、 [Destination SDKの概要](../../getting-started.md) ページを参照してください。Adobe I/O認証に必要な資格情報や、Destination SDKAPI を使用するためのその他の前提条件が取得されます。

## 手順 1：サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、 `/destination-server` endpoint to [サーバーとファイルの設定を作成する](../../authoring-api/destination-server/create-destination-server.md).

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しい宛先サーバー設定を作成します。以下のペイロードには、汎用が含まれています [!DNL Amazon S3] 設定、定義済み、デフォルト [CSV ファイル形式](../../functionality/destination-server/file-formatting.md) ユーザーが設定 UI で定義できるExperience Platformパラメーター。

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
        "bucketName": {
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

正常な応答は、一意の識別子 (`instanceId`) が含まれています。 この値は、次の手順で必要になるため保存します。

## 手順 2：宛先の構成の作成 {#create-destination-configuration}

前の手順で宛先サーバーとファイルの形式設定を作成した後、 `/destinations` 宛先設定を作成する API エンドポイント。

でサーバー設定を接続するには、以下を実行します。 [手順 1](#create-server-file-configuration) をこの宛先設定に追加するには、 `destinationServerId` 以下の API リクエストの値に、 `instanceId` の宛先サーバーを作成する際に取得される値 [手順 1](#create-server-file-configuration).

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

正常な応答は、一意の識別子 (`instanceId`) が含まれています。 この値は、宛先設定を更新するためにさらに HTTP リクエストを実行する必要がある場合に必要なため保存します。

## 手順 3:Experience PlatformUI の確認 {#verify-ui}

上記の設定に基づいて、Experience Platformカタログに新しいプライベートの宛先カードが表示され、使用できるようになります。

![選択した宛先カードを含む宛先カタログページを示す画面記録。](../../assets/guides/batch/destination-card.gif)

以下の画像と記録で、 [ファイルベースの宛先のアクティベーションワークフロー](../../../ui/activate-batch-profile-destinations.md) の宛先設定で選択したオプションに一致する。

宛先に関する詳細を入力する際に、表示されるフィールドは設定で設定したカスタムデータフィールドです。

>[!TIP]
>
>カスタムデータフィールドを宛先設定に追加する順序は、UI に反映されません。 顧客データフィールドは、次の画面記録に表示される順序で常に表示されます。

![設定で定義された顧客データフィールドを示す画面記録。](../../assets/guides/batch/file-configuration-options.gif)

書き出し間隔を設定する場合、表示されるフィールドは、 `batchConfig` 設定。
![書き出しスケジュールオプション](../../assets/guides/batch/file-export-scheduling.png)

ファイル名の設定オプションを表示する際に、表示されるフィールドが `filenameConfig` オプションを設定します。
![ファイル名設定オプション](../../assets/guides/batch/file-naming-options.gif)

上記のフィールドを調整する場合は、 [ステップ 1](#create-server-file-configuration) および [2](#create-destination-configuration) を使用して、必要に応じて設定を変更します。

## 手順 4:（オプション）宛先の公開 {#publish-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

宛先を設定した後、 [宛先公開 API](../../publishing-api/create-publishing-request.md) 設定をレビュー用にAdobeに送信します。

## 手順 5:（オプション）宛先のドキュメント化 {#document-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

独立系ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）で[製品化統合](../../overview.md#productized-custom-integrations)を作成する場合、[セルフサービスドキュメント化プロセス](../../docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを [Experience Platform 宛先カタログ](../../../catalog/overview.md)に作成します。

## 次の手順 {#next-steps}

この記事を読むと、カスタムの作成方法がわかります [!DNL Amazon S3] 宛先を指定します。Destination SDKを使用します。 次に、チームが [ファイルベースの宛先のアクティベーションワークフロー](../../../ui/activate-batch-profile-destinations.md) をクリックして、宛先にデータを書き出します。
