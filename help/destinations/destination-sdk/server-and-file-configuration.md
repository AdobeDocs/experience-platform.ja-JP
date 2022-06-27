---
description: ファイルベースの宛先のサーバーとファイル構成スペックは、Adobe Experience Platform Destination SDK で /destination-servers エンドポイントを介して構成できます。
title: （ベータ版）ファイルベースの宛先サーバーの仕様の構成オプション
exl-id: 56434e36-0458-45d9-961d-f6505de998f7
source-git-commit: 7a72c190d28d63c7bcd1bf12d8a52efc4589b848
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 90%

---

# （ベータ版）ファイルベースの宛先サーバーの仕様に応じたサーバーとファイルの構成

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience Platform Destination SDK を使用して、ファイルベースの宛先を構成および提出する機能は、現在ベータ版となっています。ドキュメントと機能は変更される場合があります。

このページでは、ファイルベースの宛先サーバーのすべてのサーバー構成オプションの詳細を説明し、Experience Platform から宛先にファイルを書き出すユーザー向けの様々なファイル構成オプションの設定方法について説明します。

ファイルベースの宛先のサーバーおよびファイル構成の仕様は、`/destination-servers` エンドポイントを介して Adobe Experience Platform Destination SDK で構成できます。エンドポイントで実行できる操作の完全なリストについては、[宛先サーバー API エンドポイントの操作](./destination-server-api.md)をお読みください。

## ファイルベースの Amazon S3 宛先サーバー仕様 {#s3-example}

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
       // See the file formatting configuration section further below on this page
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Amazon S3] の場合は、これを `FILE_BASED_S3` に設定します。 |
| `fileBasedS3Destination.bucket.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedS3Destination.bucket.value` | 文字列 | この宛先が使用する [!DNL Amazon S3] バケット名。 |
| `fileBasedS3Destination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedS3Destination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | オブジェクト | 詳しくは、 [ファイルフォーマット設定](#file-configuration) この節の詳しい説明は、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ファイルベースの SFTP 宛先サーバー仕様 {#sftp-example}

```json
{
   "name":"File-based SFTP destination server",
   "destinationServerType":"FILE_BASED_SFTP",
   "fileBasedSftpDestination":{
      "rootDirectory":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.rootDirectory}}"
      },
      "hostName":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.hostName}}"
      },
      "port": 22,
      "encryptionMode" : "PGP"
   },
    "fileConfigurations": {
       // See the file formatting configuration section further below on this page
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL SFTP] の宛先の場合、これを `FILE_BASED_SFTP` に設定します。 |
| `fileBasedSftpDestination.rootDirectory.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedSftpDestination.rootDirectory.value` | 文字列 | 宛先ストレージのルートディレクトリ。 |
| `fileBasedSftpDestination.hostName.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedSftpDestination.hostName.value` | 文字列 | 宛先ストレージのホスト名。 |
| `port` | 整数 | SFTP ファイルサーバーのポート。 |
| `encryptionMode` | 文字列 | ファイルの暗号化を使用するかどうかを示します。 サポートされている値。 <ul><li>PGP</li><li>なし</li></ul> |
| `fileConfigurations` | オブジェクト | 詳しくは、 [ファイルフォーマット設定](#file-configuration) この節の詳しい説明は、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ファイルベースの [!DNL Azure Data Lake Storage]（[!DNL ADLS]）宛先サーバーの仕様 {#adls-example}

```json
{
   "name":"ADLS destination server",
   "destinationServerType":"FILE_BASED_ADLS_GEN2",
   "fileBasedAdlsGen2Destination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   },
  "fileConfigurations": {
       // See the file formatting configuration section further below on this page
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Azure Data Lake Storage] の宛先の場合、これを `FILE_BASED_ADLS_GEN2` に設定します。 |
| `fileBasedAdlsGen2Destination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedAdlsGen2Destination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | オブジェクト | 詳しくは、 [ファイルフォーマット設定](#file-configuration) この節の詳しい説明は、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ファイルベースの [!DNL Azure Blob Storage] 宛先サーバーの仕様 {#blob-example}

```json
{
   "name":"Blob destination server",
   "destinationServerType":"FILE_BASED_AZURE_BLOB",
   "fileBasedAzureBlobDestination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      },
      "container":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.container}}"
      }
   },
  "fileConfigurations": {
       // See the file formatting configuration section further below on this page
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Azure Blob Storage] の宛先の場合、これを `FILE_BASED_AZURE_BLOB` に設定します。 |
| `fileBasedAzureBlobDestination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedAzureBlobDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileBasedAzureBlobDestination.container.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedAzureBlobDestination.container.value` | 文字列 | この宛先で使用される [!DNL Azure Blob Storage] コンテナ名。 |
| `fileConfigurations` | オブジェクト | 詳しくは、 [ファイルフォーマット設定](#file-configuration) この節の詳しい説明は、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ファイルベースの [!DNL Data Landing Zone]（[!DNL DLZ]）宛先サーバーの仕様 {#dlz-example}

```json
{
   "name":"DLZ destination server",
   "destinationServerType":"FILE_BASED_DLZ",
   "fileBasedDlzDestination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      },
      "useCase": "Your use case"
   },
   "fileConfigurations": {
       // See the file formatting configuration section further below on this page
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Data Landing Zone] の宛先の場合、これを `FILE_BASED_DLZ` に設定します。 |
| `fileBasedDlzDestination.path.templatingStrategy` | 文字列 | *必須。*`PEBBLE_V1` を使用します。 |
| `fileBasedDlzDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | オブジェクト | 詳しくは、 [ファイルフォーマット設定](#file-configuration) この節の詳しい説明は、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ファイルベースの [!DNL Google Cloud Storage] 宛先サーバーの仕様 {#gcs-example}

```json
{
   "name":"Google Cloud Storage Server",
   "destinationServerType":"FILE_BASED_GOOGLE_CLOUD",
   "fileBasedGoogleCloudStorageDestination":{
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
      // See the file formatting configuration section further below on this page
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Google Cloud Storage] の宛先の場合、これを `FILE_BASED_GOOGLE_CLOUD` に設定します。 |
| `fileBasedGoogleCloudStorageDestination.bucket.templatingStrategy` | 文字列 | *必須。*`PEBBLE_V1` を使用します。 |
| `fileBasedGoogleCloudStorageDestination.bucket.value` | 文字列 | この宛先が使用する [!DNL Google Cloud Storage] バケット名。 |
| `fileBasedGoogleCloudStorageDestination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedGoogleCloudStorageDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | オブジェクト | 詳しくは、 [ファイルフォーマット設定](#file-configuration) この節の詳しい説明は、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ファイル形式設定 {#file-configuration}

この節では、書き出された `CSV` ファイルのフォーマット設定について説明します。Experience Platform から受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、ユーザー側のファイル受け取りシステムの要件に合わせて変更することができます。

>[!NOTE]
>
>CSV オプションは、CSV ファイルの書き出し時にのみサポートされます。 新しい宛先サーバーを設定する際、`fileConfigurations` セクションは必須ではありません。CSV オプションの API 呼び出しで値を渡さない場合、以下の表のデフォルト値が使用されます。

```json
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
        },
        "maxFileRowCount":5000000
    }
```

| フィールド | 必須／オプション | 説明 | デフォルト値 |
|---|---|---|---|
| `compression.value` | オプション | データをファイルに保存する際に使用する圧縮コーデック。 サポートされている値：`none`、`bzip2`、`gzip`、`lz4`、`snappy`。 | `none` |
| `fileType.value` | オプション | 出力ファイル形式を指定します。 サポートされている値： `csv`、`parquet`、`json`。 | `csv` |
| `csvOptions.quote.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用された値をエスケープするために使用する 1 文字を設定します。区切り記号を値の一部として使用することもできます。 | `null` |
| `csvOptions.quoteAll.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。すべての値を常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含む値のみをエスケープします。 | `false` |
| `csvOptions.escape.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。 既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。 | `\` |
| `csvOptions.escapeQuotes.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符を含む値を、常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含むすべての値をエスケープします。 | `true` |
| `csvOptions.header.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。列の名前を最初の行として書き込むかどうかを示します。 | `true` |
| `csvOptions.ignoreLeadingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から先頭の空白をトリミングするかどうかを示します。 | `true` |
| `csvOptions.ignoreTrailingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から末尾の空白をトリミングするかどうかを示します。 | `true` |
| `csvOptions.nullValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。null 値の文字列表現を設定します。 | `""` |
| `csvOptions.dateFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。日付の形式を示します。 | `yyyy-MM-dd` |
| `csvOptions.timestampFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。タイムスタンプ形式を示す文字列を設定します。 | `yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX]` |
| `csvOptions.charToEscapeQuoteEscaping.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符文字のエスケープに使用する 1 文字を設定します。 | エスケープ文字と引用符文字が異なる場合は `\`。 エスケープ文字と引用符文字が同じ場合は `\0` を使用します。 |
| `csvOptions.emptyValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。空の値の文字列表現を設定します。 | `""` |
| `csvOptions.lineSep.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。書き込みに使用する行区切り記号を定義します。 最大長は 1 文字です。 | `\n` |
| `maxFileRowCount` | オプション | エクスポートするファイルに含めることができる最大行数。 宛先プラットフォームのファイルサイズの要件に基づいて、これを設定します。 | なし |

{style=&quot;table-layout:auto&quot;}