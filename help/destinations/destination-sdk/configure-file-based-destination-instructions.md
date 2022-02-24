---
description: このページでは、Destination SDKを使用してファイルベースの宛先を設定する手順について説明します。
title: （ベータ版）Destination SDKを使用してファイルベースの宛先を設定する
source-git-commit: 92bca3600d854540fd2badd925e453fba41601a7
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 2%

---

# （ベータ版）Destination SDKを使用してファイルベースの宛先を設定する

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience Platform Destination SDKを使用してファイルベースの宛先を設定および送信する機能は、現在、ベータ版です。 ドキュメントと機能は変更される場合があります。

このページでは、 [宛先 SDK の設定オプション](./configuration-options.md) を設定するには、他のDestination SDK機能と API リファレンスドキュメントで [ファイルベースの宛先](../../destinations/destination-types.md#file-based). 手順は、次の順に並べられます。

## 前提条件 {#prerequisites}

以下に示す手順に進む前に、 [Destination SDKの概要](./getting-started.md) ページを参照してください。Adobe I/O認証に必要な資格情報や、Destination SDKAPI を使用するためのその他の前提条件が取得されます。

## Destination SDKの設定オプションを使用して宛先を設定する手順 {#steps}

![Destination SDKエンドポイントの使用手順の説明](./assets/destination-sdk-steps-batch.png)

## 手順 1:サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、 `/destinations-server` endpoint （読み取り） [API リファレンス](./destination-server-api.md)) をクリックします。

次に、 [!DNL Amazon S3] 宛先。 他のタイプのファイルベースの宛先を設定するには、 [サーバーの設定](server-and-file-configuration.md).

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

```json
{
    "name": "S3 destination",
    "destinationServerType": "FILE_BASED_S3",
    "fileBasedS3Destination": {
        "bucket": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.bucket}}"
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
            },
            "lineSep": {
                "templatingStrategy": "NONE",
                "value": "\n"
            }
        }
    }
}
```

## 手順 2:宛先設定の作成 {#create-destination-configuration}

以下に、 `/destinations` API エンドポイント。 この設定について詳しくは、 [宛先の設定](./file-based-destination-configuration.md).

手順 1 のサーバーとファイル設定をこの宛先設定に接続するには、サーバーのインスタンス ID とテンプレート設定を、 `destinationServerId` こちら。

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destinations
```

```json
{
    "name": "Amazon S3 destination",
    "description": "Amazon S3 destination is a fictional destination, used for this example.",
    "releaseNotes": "Test",
    "status": "Test",
    "customerAuthenticationConfigurations": [
        {
            "authType": "S3"
        }
    ],
    "customerEncryptionConfigurations": [],
    "customerDataFields": [
        {
            "name": "bucket",
            "title": "Amazon S3 bucket name",
            "description": "Enter the Amazon S3 Bucket name that will host the exported files.",
            "type": "string",
            "isRequired": true,
            "readOnly": false,
            "hidden": false
        },
        {
            "name": "path",
            "title": "Amazon S3 path",
            "description": "Enter Amazon S3 folder path",
            "type": "string",
            "isRequired": true,
            "pattern": "^[A-Za-z]+$",
            "readOnly": false,
            "hidden": false
        },
        {
            "name": "compression",
            "title": "Select compression type",
            "description": "Select the file compression type used by the exported files.",
            "type": "string",
            "isRequired": true,
            "readOnly": false,
            "enum": [
                "GZIP",
                "NONE",
                "bzip2",
                "lz4",
                "snappy",
                "deflate"
            ]
        },
        {
            "name": "fileType",
            "title": "Select a file format",
            "description": "Select the file format to be used by the exported files.",
            "type": "string",
            "isRequired": true,
            "readOnly": false,
            "hidden": false,
            "enum": [
                "csv",
                "json",
                "parquet"
            ],
            "default": "csv"
        }
    ],
    "uiAttributes": {
        "documentationLink": "https://www.adobe.io/apis/experienceplatform.html",
        "category": "S3",
        "iconUrl": "https://dc5tqsrhldvnl.cloudfront.net/2/90048/da276e30c730ce6cd666c8ca78360df21.png",
        "connectionType": "S3",
        "flowRunsSupported": true,
        "monitoringSupported": true,
        "frequency": "Batch"
    },
    "destinationDelivery": [
        {
            "deliveryMatchers": [
                {
                    "type": "SOURCE",
                    "value": [
                        "batch"
                    ]
                }
            ],
            "authenticationRule": "CUSTOMER_AUTHENTICATION",
            "destinationServerId": "eec25bde-4f56-4c02-a830-9aa9ec73ee9d"
        }
    ],
    "schemaConfig": {
        "profileRequired": true,
        "segmentRequired": true,
        "identityRequired": true
    },
    "batchConfig": {
        "allowMandatoryFieldSelection": true,
        "allowDedupeKeyFieldSelection": true,
        "defaultExportMode": "DAILY_FULL_EXPORT",
        "allowedExportMode": [
            "DAILY_FULL_EXPORT",
            "FIRST_FULL_THEN_INCREMENTAL"
        ],
        "allowedScheduleFrequency": [
            "DAILY",
            "EVERY_3_HOURS",
            "EVERY_6_HOURS",
            "EVERY_8_HOURS",
            "EVERY_12_HOURS",
            "ONCE"
        ],
        "defaultFrequency": "DAILY",
        "defaultStartTime": "00:00"
    },
    "backfillHistoricalProfileData": true
}
```

## 手順 3:オーディエンスメタデータ設定の作成 {#create-audience-metadata-configuration}

一部の宛先では、Destination SDKは、宛先のオーディエンスをプログラムで作成、更新、削除するように、オーディエンスメタデータを設定する必要があります。 参照： [Audience metadata management](./audience-metadata-management.md) を参照してください。

オーディエンスメタデータ設定を使用する場合は、手順 2 で作成した宛先設定に接続する必要があります。 オーディエンスメタデータ設定のインスタンス ID を、 `audienceTemplateId`.

## 手順 4:認証の設定 {#set-up-authentication}

次の項目を指定するかどうかに応じて、 `"authenticationRule": "CUSTOMER_AUTHENTICATION"` または `"authenticationRule": "PLATFORM_AUTHENTICATION"` 上記の宛先設定で、 `/destination` または `/credentials` endpoint.

* 選択した場合 `"authenticationRule": "CUSTOMER_AUTHENTICATION"` 宛先の設定で、ファイルベースの宛先の認証でサポートされる認証のDestination SDKについて、次の節を参照してください。

   * [Amazon S3 認証](authentication-configuration.md#s3)
   * [Azure 接続文字列](authentication-configuration.md#blob)
   * [Azure サービスプリンシパル](authentication-configuration.md#adls)
   * [SSH キーを使用した SFTP 認証](authentication-configuration.md#sftp-ssh)
   * [パスワードを使用した SFTP 認証](authentication-configuration.md#sftp-password)

* 選択した場合 `"authenticationRule": "PLATFORM_AUTHENTICATION"`（を参照） [認証設定](./authentication-configuration.md#when-to-use).


<!-- ## Step 5: Test your destination {#test-destination}

After setting up your destination using the configuration endpoints in the previous steps, you can use the [destination testing tool](./create-template.md) to test the integration between Adobe Experience Platform and your destination.

As part of the process to test your destination, you must use the Experience Platform UI to create segments, which you will activate to your destination. Refer to the two resources below for instructions how to create segments in Experience Platform:

* [Create a segment documentation page](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html?lang=en#create-segment)
* [Create a segment video walkthrough](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en) -->

## 手順 5:宛先を公開 {#publish-destination}

宛先を設定およびテストした後、 [宛先公開 API](./destination-publish-api.md) 設定をレビュー用にAdobeに送信します。

## 手順 6:宛先のドキュメント化 {#document-destination}

独立系ソフトウェアベンダー (ISV) またはシステムインテグレータ (SI) の場合、 [製品化統合](./overview.md#productized-custom-integrations)、 [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md) 宛先の製品ドキュメントページを [Experience Platform先カタログ](/help/destinations/catalog/overview.md).
