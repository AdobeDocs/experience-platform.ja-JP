---
description: Destination SDKを使用して、事前に定義されたファイル形式オプションとカスタムファイル名設定を使用して SFTP の宛先を設定する方法について説明します。
title: （ベータ版）事前に定義されたファイル形式オプションとカスタムファイル名の設定を使用して、SFTP の宛先を設定します。
source-git-commit: 7198d8529f64e5a724cbd9c95714b66aa62a29d2
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 10%

---

# （ベータ版）事前に定義されたファイル形式オプションとカスタムのファイル名設定を使用して SFTP の宛先を設定する

## 概要 {#overview}

>[!IMPORTANT]
>
>現在、データを使用してファイルベースの宛先を設定する機能は、ベータ版です。 ドキュメントと機能は変更される場合があります。

このページでは、Destination SDKを使用して、事前に定義されたデフォルトの SFTP の宛先を設定する方法について説明します [ファイル形式オプション](../../server-and-file-configuration.md#file-configuration) そして習慣 [ファイル名設定](../../file-based-destination-configuration.md#file-name-configuration).

このページには、SFTP の宛先で使用できるすべての設定オプションが表示されます。 以下の手順で示す設定を編集したり、必要に応じて設定の特定の部分を削除したりできます。

## 前提条件 {#prerequisites}

以下の手順に進む前に、 [Destination SDKの概要](../../getting-started.md) ページを参照してください。Adobe I/O認証に必要な資格情報や、Destination SDKAPI を使用するためのその他の前提条件が取得されます。

## 手順 1：サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、 `/destination-server` エンドポイント：サーバーとファイルの設定を作成します。 HTTP リクエストのパラメーターについて詳しくは、 [ファイルベースの宛先のサーバーおよびファイル構成の仕様](../../server-and-file-configuration.md#sftp-example) そして関連する [ファイル形式設定](../../server-and-file-configuration.md#file-configuration).

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しい宛先サーバー設定を作成します。以下のペイロードには、事前に定義されたデフォルトの汎用 SFTP 設定が含まれています [CSV ファイル形式](../../server-and-file-configuration.md#file-configuration) ユーザーが設定 UI で定義できるExperience Platformパラメーター。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-server \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "name": "SFTP destination with predefined CSV formatting options",
    "destinationServerType": "FILE_BASED_SFTP",
    "fileBasedSFTPDestination": {
        "hostname": {
            "templatingStrategy": "NONE",
            "value": "{{customerData.hostname}}"
        },
        "rootDirectory": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.remotePath}}"
        },
        "port": 22
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
            },
            "lineSep": {
                "templatingStrategy": "NONE",
                "value": "\n"
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

* [認証設定](../../authentication-configuration.md#sftp)
* [バッチ保存先の設定](../../file-based-destination-configuration.md#batch-configuration)
* [ファイルベースの宛先設定 API の操作](../../destination-configuration-api.md#create-file-based)

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
   "name":"SFTP destination with predefined CSV formatting options",
   "description":"SFTP destination with predefined CSV formatting options",
   "releaseNotes":"",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
      {
         "authType":"SFTP_WITH_PASSWORD"
      },
      {
         "authType":"SFTP_WITH_SSH_KEY"
      }
   ],
   "customerEncryptionConfigurations":[
       
   ],
   "customerDataFields":[
      {
         "name":"remotePath",
         "title":"Root directory",
         "description":"Enter root directory",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"hostname",
         "title":"Hostname",
         "description":"Enter hostname",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-sftp-en",
      "category":"SFTP",
      "connectionType":"SFTP",
      "monitoringSupported":true,
      "flowRunsSupported":true,
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
         "destinationServerId":"{{instanceID of your destination server}}"
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

宛先を設定した後、 [宛先公開 API](../../destination-publish-api.md) 設定をレビュー用にAdobeに送信します。

## 手順 5:（オプション）宛先のドキュメント化 {#document-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

独立系ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）で[製品化統合](../../overview.md#productized-custom-integrations)を作成する場合、[セルフサービスドキュメント化プロセス](../../docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを [Experience Platform 宛先カタログ](../../../catalog/overview.md)に作成します。

## 次の手順 {#next-steps}

この記事を読むと、Destination SDKを使用してカスタム SFTP の宛先を作成する方法がわかります。 次に、チームが [ファイルベースの宛先のアクティベーションワークフロー](../../../ui/activate-batch-profile-destinations.md) をクリックして、宛先にデータを書き出します。