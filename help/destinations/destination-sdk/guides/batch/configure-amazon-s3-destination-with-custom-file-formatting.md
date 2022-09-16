---
description: Destination SDKを使用して、カスタムのファイル名と書式設定オプションを使用してAmazon S3 の宛先を設定する方法について説明します。
title: （ベータ版）カスタムのファイル名と書式設定オプションを使用してAmazon S3 の宛先を設定します。
exl-id: eed73572-5050-44fa-ba16-90729c65495e
source-git-commit: 6a666c9850a4a55bfa93a54111a597f690eb8431
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 11%

---

# （ベータ版）カスタムのファイル名と書式設定オプションを使用してAmazon S3 の宛先を設定する

## 概要 {#overview}

>[!IMPORTANT]
>
>現在、データを使用してファイルベースの宛先を設定する機能は、ベータ版です。 ドキュメントと機能は変更される場合があります。

このページでは、Destination SDKを使用して、カスタムでAmazon S3 の宛先を設定する方法について説明します [ファイル形式オプション](/help/destinations/destination-sdk/server-and-file-configuration.md#file-configuration) そして習慣 [ファイル名設定](/help/destinations/destination-sdk/file-based-destination-configuration.md#file-name-configuration).

このページには、Amazon S3 の宛先で使用できるすべての設定オプションが表示されます。 以下の手順で示す設定を編集したり、必要に応じて設定の特定の部分を削除したりできます。

## 前提条件 {#prerequisites}

以下の手順に進む前に、 [Destination SDKの概要](/help/destinations/destination-sdk/getting-started.md) ページを参照してください。Adobe I/O認証に必要な資格情報や、Destination SDKAPI を使用するためのその他の前提条件が取得されます。

## 手順 1：サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、 `/destination-server` エンドポイント：サーバーとファイルの設定を作成します。 HTTP リクエストのパラメーターについて詳しくは、 [ファイルベースの宛先のサーバーおよびファイル構成の仕様](/help/destinations/destination-sdk/server-and-file-configuration.md#s3-example) そして関連する [ファイル形式設定](/help/destinations/destination-sdk/server-and-file-configuration.md#file-configuration).

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しい宛先サーバー設定を作成します。以下のペイロードには、カスタムを使用した一般的なAmazon S3 設定が含まれています [CSV ファイル形式](/help/destinations/destination-sdk/server-and-file-configuration.md#file-configuration) ユーザーが設定 UI で定義できるExperience Platformパラメーター。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-server \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d ' {
   "name":"Amazon S3 destination server with custom file formatting options",
   "destinationServerType":"FILE_BASED_S3",
   "fileBasedS3Destination":{
      "bucket":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.bucket}}"
      },
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   },
   "fileConfigurations":{
      "compression":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.compression}}"
      },
      "fileType":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.fileType}}"
      },
      "csvOptions":{
         "sep":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.sep}}"
         },
         "encoding":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.encoding}}"
         },
         "quote":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.quote}}"
         },
         "quoteAll":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.quoteAll}}"
         },
         "escape":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.escape}}"
         },
         "escapeQuotes":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.escapeQuotes}}"
         },
         "header":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.header}}"
         },
         "ignoreLeadingWhiteSpace":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.ignoreLeadingWhiteSpace}}"
         },
         "nullValue":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.nullValue}}"
         },
         "dateFormat":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.dateFormat}}"
         },
         "charToEscapeQuoteEscaping":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.charToEscapeQuoteEscaping}}"
         },
         "emptyValue":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{customerData.dateFormat}}"
         }
      }
   }
}'
```

正常な応答は、一意の識別子 (`instanceId`) が含まれています。 この値は、次の手順で必要になるため保存します。

## 手順 2：宛先の構成の作成 {#create-destination-configuration}

前の手順で宛先サーバーとファイルの形式設定を作成した後、 `/destinations` 宛先設定を作成する API エンドポイント。

でサーバー設定を接続するには、以下を実行します。 [手順 1](#create-server-file-configuration) をこの宛先設定に追加するには、 `destinationServerId` の値と、 [手順 1](#create-server-file-configuration).

以下で使用するパラメーターについて詳しくは、次のページを参照してください。

* [認証設定](/help/destinations/destination-sdk/authentication-configuration.md#s3)
* [バッチ保存先の設定](/help/destinations/destination-sdk/file-based-destination-configuration.md#batch-configuration)
* [ファイルベースの宛先設定 API の操作](/help/destinations/destination-sdk/destination-configuration-api.md#create-file-based)

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
   "name":"Amazon S3 destination with custom file formatting options and custom file name configuration",
   "description":"Amazon S3 destination with custom file formatting options and custom file name configuration",
   "releaseNotes":"Amazon S3 destination with custom file formatting options and custom file name configuration",
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
         "name":"sep",
         "title":"Enter your desired separator for each field and value",
         "description":"Enter your desired separator for each field and value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"encoding",
         "title":"Select the desired CSV file encoding",
         "description":"Select the desired CSV file encoding",
         "type":"string",
         "enum":[
            "UTF-8",
            "UTF-16"
         ],
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"quote",
         "title":"Quoted values escape character",
         "description":"Enter the desired character to be used for escaping quoted values.",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"quoteAll",
         "title":"Escape all quoted values",
         "description":"Select whether to escape all quoted values.",
         "type":"string",
         "enum":[
            "true",
            "false"
         ],
         "default":"true",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"escape",
         "title":"Quote escaping character",
         "description":"Enter the desired character to be used for escaping quotes inside an already quoted value.",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"escapeQuotes",
         "title":"Enclose quoted values within quotes",
         "description":"Select whether values containing quotes should always be enclosed in quotes.",
         "type":"string",
         "enum":[
            "true",
            "false"
         ],
         "isRequired":false,
         "default":"true",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"header",
         "title":"Generate file header.",
         "description":"Select whether to write the names of columns as the first line of the exported files.",
         "type":"string",
         "isRequired":false,
         "enum":[
            "true",
            "false"
         ],
         "readOnly":false,
         "default":"true",
         "hidden":false
      },
      {
         "name":"ignoreLeadingWhiteSpace",
         "title":"Ignore leading white space",
         "description":"Select whether leading whitespaces should be trimmed from exported values.",
         "type":"string",
         "isRequired":false,
         "enum":[
            "true",
            "false"
         ],
         "readOnly":false,
         "default":"true",
         "hidden":false
      },
      {
         "name":"nullValue",
         "title":"NULL value string format",
         "description":"Enter the string representation of a NULL value. ",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"dateFormat",
         "title":"Date format",
         "description":"Enter the desired date format. ",
         "type":"string",
         "default":"yyyy-MM-dd",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"charToEscapeQuoteEscaping",
         "title":"Quote escaping escape character",
         "description":"Enter the desired character to be used for escaping the escaping of a quote character.",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"emptyValue",
         "title":"Empty value string format",
         "description":"Enter the string representation of an empty value.",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "default":"",
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

![選択した宛先カードを含む宛先カタログページを示す画面記録。](../../assets/destination-card.gif)

以下の画像と記録で、 [ファイルベースの宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md) の宛先設定で選択したオプションに一致する。

宛先に関する詳細を入力する際に、表示されるフィールドは設定で設定したカスタムデータフィールドです。

>[!TIP]
>
>カスタムデータフィールドを宛先設定に追加する順序は、UI に反映されません。 カスタムデータフィールドは、次の画面の記録で表示される順序で常に表示されます。

![宛先の詳細を入力](../../assets/file-configuration-options.gif)

書き出し間隔を設定する場合、表示されるフィールドは、 `batchConfig` 設定。
![書き出しスケジュールオプション](../../assets/file-export-scheduling.png)

ファイル名の設定オプションを表示する際に、表示されるフィールドが `filenameConfig` オプションを設定します。
![ファイル名設定オプション](../../assets/file-naming-options.gif)

上記のフィールドを調整する場合は、 [ステップ 1](#create-server-file-configuration) および [2](#create-destination-configuration) を使用して、必要に応じて設定を変更します。

## 手順 4:（オプション）宛先の公開 {#publish-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

宛先を設定した後、 [宛先公開 API](/help/destinations/destination-sdk/destination-publish-api.md) 設定をレビュー用にAdobeに送信します。

## 手順 5:（オプション）宛先のドキュメント化 {#document-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

独立系ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）で[製品化統合](/help/destinations/destination-sdk/overview.md#productized-custom-integrations)を作成する場合、[セルフサービスドキュメント化プロセス](/help/destinations/destination-sdk/docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを [Experience Platform 宛先カタログ](/help/destinations/catalog/overview.md)に作成します。

## 次の手順 {#next-steps}

この記事を読むと、カスタムの作成方法がわかります [!DNL Amazon S3] 宛先を指定します。Destination SDKを使用します。 次に、チームが [ファイルベースの宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md) をクリックして、宛先にデータを書き出します。
