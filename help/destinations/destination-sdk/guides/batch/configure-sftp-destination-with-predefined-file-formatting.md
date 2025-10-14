---
description: Destination SDKを使用して、事前定義済みのファイル形式オプションとカスタムファイル名設定で SFTP 宛先を設定する方法を説明します。
title: 事前定義済みのファイル形式オプションとカスタムファイル名設定を使用して、SFTP の宛先を設定します。
exl-id: 6e0fe019-7fbb-48e4-9469-6cc7fc3cb6e4
source-git-commit: d47c82339afa602a9d6914c1dd36a4fc9528ea32
workflow-type: tm+mt
source-wordcount: '713'
ht-degree: 10%

---

# 事前定義済みファイル形式オプションとカスタムファイル名設定を使用した、 SFTP の宛先の設定

## 概要 {#overview}

ここでは、Destination SDKを使用して、事前定義済みのデフォルトの [&#x200B; ファイル形式オプション &#x200B;](configure-file-formatting-options.md) カスタムの [&#x200B; ファイル名設定 &#x200B;](../../functionality/destination-configuration/batch-configuration.md#file-name-configuration) で SFTP 宛先を設定する方法について説明します。

このページには、SFTP 宛先に使用できるすべての設定オプションが表示されます。 必要に応じて、以下の手順に示す設定を編集したり、設定の特定の部分を削除したりできます。

以下で使用されるパラメーターについて詳しくは、[Destinations SDK の設定オプション &#x200B;](../../functionality/configuration-options.md) を参照してください。

## 前提条件 {#prerequisites}

以下に説明する手順に進む前に、[Destination SDKの概要 &#x200B;](../../getting-started.md) ページを参照して、Adobe I/ODestination SDK資格情報および認証 API を使用するために必要なその他の前提条件について確認してください。

## 手順 1：サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、`/destination-server` エンドポイントを使用して [&#x200B; サーバーとファイル設定を作成 &#x200B;](../../authoring-api/destination-server/create-destination-server.md) します。

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しい宛先サーバー設定を作成します。
以下のペイロードには、ユーザーがExperience PlatformUI で定義できる、事前定義済みのデフォルトの [CSV ファイル形式 &#x200B;](../../functionality/destination-server/file-formatting.md) 設定パラメーターが含まれる汎用 SFTP 設定が含まれています。

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
            }
        }
    }
}'
```

リクエストが成功した場合は、設定の一意の ID （`instanceId`）を含む、新しい宛先サーバー設定が返されます。 この値は、次の手順で必要になるので保存します。

## 手順 2：宛先の構成の作成 {#create-destination-configuration}

前の手順で宛先サーバーとファイル形式設定を作成したら、`/destinations` API エンドポイントを使用して宛先設定を作成できるようになりました。

[&#x200B; 手順 1](#create-server-file-configuration) のサーバー設定をこの宛先設定に接続するには、以下の API リクエストの `destinationServerId` の値を、[&#x200B; 手順 1](#create-server-file-configuration) で宛先サーバーを作成する際に取得した値に置き換えます。

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

リクエストが成功した場合は、設定の一意の ID （`instanceId`）を含む、新しい宛先設定が返されます。 宛先設定を更新するための HTTP リクエストを別途行う必要がある場合に、この値を必要に応じて保存します。

## 手順 3:Experience PlatformUI の確認 {#verify-ui}

上記の設定に基づいて、Experience Platformカタログには、使用する新しいプライベート宛先カードが表示されるようになりました。

![&#x200B; 選択した宛先カードを含む宛先カタログページを示す画面録画。](../../assets/guides/batch/destination-card.gif)

以下の画像と録画では、[&#x200B; ファイルベースの宛先のアクティベーションワークフロー &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md) のオプションが、宛先設定で選択したオプションにどのように一致するかを確認してください。

宛先に関する詳細を入力する場合、設定で設定したカスタムデータフィールドが、どのようなフィールドで表示されるかを確認してください。

>[!TIP]
>
>カスタムデータフィールドを宛先設定に追加した順序は、UI には反映されません。 カスタムデータフィールドは、常に、以下の画面録画で表示される順序で表示されます。

![&#x200B; 宛先の詳細の入力 &#x200B;](../../assets/guides/batch/file-configuration-options.gif)

書き出し間隔をスケジュールする際には、`batchConfig` 設定で設定したフィールドが各フィールドにどのように表示されるかを確認してください。
![&#x200B; スケジュール オプションのエクスポート &#x200B;](../../assets/guides/batch/file-export-scheduling.png)

ファイル名の設定オプションを表示すると、設定で設定した `filenameConfig` のオプションが、表示されるフィールドでどのように表れているかに注意してください。
![&#x200B; ファイル名設定オプション &#x200B;](../../assets/guides/batch/file-naming-options.gif)

上記のフィールドを調整する場合は、[&#x200B; 手順 1](#create-server-file-configuration) および [2](#create-destination-configuration) を繰り返し、必要に応じて設定を変更します。

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

この記事では、Destination SDKを使用したカスタム SFTP 宛先の作成方法を確認しました。 次に、チームは [&#x200B; ファイルベース宛先のアクティベーションワークフロー &#x200B;](../../../ui/activate-batch-profile-destinations.md) を使用して、宛先にデータを書き出すことができます。
