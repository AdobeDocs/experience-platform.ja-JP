---
description: Destination SDKを使用して、カスタムのファイル形式オプションとカスタムのファイル名設定を使用して、データランディングゾーン (DLZ) の宛先を構成する方法を説明します。
title: カスタムのファイル形式設定オプションとカスタムのファイル名設定を使用して、データランディングゾーン (DLZ) の宛先を構成します。
exl-id: 3a5c1188-c2b5-4e81-ae41-9fff797f08a6
source-git-commit: d47c82339afa602a9d6914c1dd36a4fc9528ea32
workflow-type: tm+mt
source-wordcount: '705'
ht-degree: 10%

---

# の設定 [!DNL Data Landing Zone (DLZ)] カスタムのファイルフォーマットオプションとカスタムのファイル名設定を使用した宛先

## 概要 {#overview}

このページでは、Destination SDKを使用して [!DNL Data Landing Zone] カスタムの宛先 [ファイル形式オプション](configure-file-formatting-options.md) そして習慣 [ファイル名設定](../../functionality/destination-configuration/batch-configuration.md#file-name-configuration).

このページには、 [!DNL Data Landing Zone] 宛先。 以下の手順で示す設定を編集したり、必要に応じて設定の特定の部分を削除したりできます。

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

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しい宛先サーバー設定を作成します。以下のペイロードには、汎用が含まれています [!DNL Data Landing Zone] 設定、カスタムを使用 [CSV ファイル形式](../../functionality/destination-server/file-formatting.md) ユーザーが設定 UI で定義できるExperience Platformパラメーター。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-server \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \ 
 -d '
{
   "name":"DLZ Destination Server",
   "destinationServerType":"FILE_BASED_DLZ",
   "fileBasedDlzDestination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
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
   }
}'
```

正常な応答は、一意の識別子 (`instanceId`) が含まれています。 この値は、次の手順で必要になるため保存します。

## 手順 2：宛先の構成の作成 {#create-destination-configuration}

前の手順で宛先サーバーとファイルの形式設定を作成した後、 `/destinations` 宛先設定を作成する API エンドポイント。

でサーバー設定を接続するには、以下を実行します。 [手順 1](#create-server-file-configuration) をこの宛先設定に追加するには、 `destinationServerId` の値と、 [手順 1](#create-server-file-configuration).

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
   "name":"DLZ Destination",
   "description":"SSD DLZ Destination",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
       
   ],
   "customerEncryptionConfigurations":[
       
   ],
   "customerDataFields":[
      {
         "name":"path",
         "title":"Folder path",
         "description":"Enter the path to your Data Landing Zone folder",
         "type":"string",
         "isRequired":true,
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
      "documentationLink":"https://www.adobe.io/apis/experienceplatform.html",
      "category":"DLZ",
      "connectionType":"Server-to-server",
      "frequency":"Batch",
      "flowRunsSupported":true,
      "monitoringSupported":true
   },
   "identityNamespaces":{
      "adobe_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      },
      "mobile_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      }
   },
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false
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
         "authenticationRule":"NONE",
         "destinationServerId":"{{instanceID of your destination server}}"
      }
   ],
   "schemaConfig":{
      "profileRequired":true,
      "identityRequired":true,
      "segmentRequired":true
   },
   "batchConfig":{
      "allowMandatoryFieldSelection":true,
      "autoSelectJoinKeyOnPartnerSchemaSelection":false,
      "joinKeyTitle":"DEDUPLICATION KEY",
      "defaultExportMode":"DAILY_FULL_EXPORT",
      "allowedExportModes":[
         "DAILY_FULL_EXPORT",
         "FIRST_FULL_THEN_INCREMENTAL"
      ],
      "allowedScheduleFrequency":[
         "DAILY",
         "EVERY_3_HOURS",
         "EVERY_6_HOURS",
         "EVERY_12_HOURS",
         "EVERY_8_HOURS",
         "ONCE",
         "EVERY_HOUR"
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
      "allowDedupKeyFieldSelection":true
   },
   "backfillHistoricalProfileData":true
}'
```

正常な応答は、一意の識別子 (`instanceId`) が含まれています。 この値は、宛先設定を更新するためにさらに HTTP リクエストを実行する必要がある場合に必要なため保存します。

## 手順 3:Experience PlatformUI の確認 {#verify-ui}

上記の設定に基づいて、Experience Platformカタログに新しいプライベートの宛先カードが表示され、使用できるようになります。

![選択した宛先カードを含む宛先カタログページを示す画面記録。](../../assets/guides/batch/dlz-destination-card.gif)

以下の画像と記録で、 [ファイルベースの宛先のアクティベーションワークフロー](../../../ui/activate-batch-profile-destinations.md) の宛先設定で選択したオプションに一致する。

宛先に関する詳細を入力する際に、表示されるフィールドは設定で設定したカスタムデータフィールドです。

>[!TIP]
>
>カスタムデータフィールドを宛先設定に追加する順序は、UI に反映されません。 カスタムデータフィールドは、次の画面の記録で表示される順序で常に表示されます。

![宛先の詳細を入力](../../assets/guides/batch/file-configuration-options.gif)

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

この記事を読むと、カスタムの作成方法がわかります [!DNL Data Landing Zone] 宛先を指定します。Destination SDKを使用します。 次に、チームが [ファイルベースの宛先のアクティベーションワークフロー](../../../ui/activate-batch-profile-destinations.md) をクリックして、宛先にデータを書き出します。
