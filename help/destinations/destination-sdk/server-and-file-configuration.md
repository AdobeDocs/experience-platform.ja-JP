---
description: ファイルベースの宛先のサーバーとファイル構成スペックは、Adobe Experience Platform Destination SDK で /destination-servers エンドポイントを介して構成できます。
title: ファイルベースの宛先サーバー仕様の構成オプション
exl-id: 56434e36-0458-45d9-961d-f6505de998f7
source-git-commit: 29962e07aa50c97b6098f4c892facf48508d28cf
workflow-type: tm+mt
source-wordcount: '1248'
ht-degree: 55%

---

# ファイルベースの宛先サーバー仕様のサーバーとファイルの構成

## 概要 {#overview}

このページでは、ファイルベースの宛先サーバーのすべてのサーバー設定オプションの詳細を説明し、Experience Platformから宛先にファイルを書き出すユーザー向けの様々なファイル設定オプションの設定方法を示します。

ファイルベースの宛先のサーバーおよびファイル構成の仕様は、`/destination-servers` エンドポイントを介して Adobe Experience Platform Destination SDK で構成できます。エンドポイントで実行できる操作の完全なリストについては、[宛先サーバー API エンドポイントの操作](./destination-server-api.md)をお読みください。

以下の節では、Destination SDKでサポートされる各バッチ宛先タイプに固有の宛先サーバー仕様について説明します。

## ファイルベースの Amazon S3 宛先サーバー仕様 {#s3-example}

以下のサンプルは、Amazon S3 の宛先に適した宛先サーバー設定を示しています。

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

以下のサンプルは、SFTP の宛先に適した宛先サーバー設定を示しています。

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

以下のサンプルは、 [!DNL Azure Data Lake Storage] 宛先。

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

以下のサンプルは、 [!DNL Azure Blob Storage] 宛先。

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

以下のサンプルは、 [!DNL Data Landing Zone] ([!DNL DLZ]) の宛先に貼り付けます。

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

以下のサンプルは、 [!DNL Google Cloud Storage] 宛先。

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
>CSV オプションは、CSV ファイルの書き出し時にのみサポートされます。 新しい宛先サーバーを設定する際、`fileConfigurations` セクションは必須ではありません。CSV オプションの API 呼び出しに値を渡さない場合、 [下の参照表](#file-formatting-reference-and-example) が使用されます。

### CSV オプションを使用したファイル設定と `templatingStrategy` に設定 `NONE` {#file-configuration-templating-none}

以下の設定例では、すべての CSV オプションが修正されています。 各 `csvOptions` パラメータは最終的なもので、ユーザーは変更できません。

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
            }
        },
        "maxFileRowCount":5000000
    }
```

### CSV オプションを使用したファイル設定と `templatingStrategy` に設定 `PEBBLE_V1` {#file-configuration-templating-pebble}

以下の設定例では、CSV オプションは修正されていません。 この `value` それぞれの `csvOptions` パラメーターは、 `/destinations` エンドポイント ( 例： `customerData.quote` の `quote` ファイルフォーマットオプションを参照 ) およびユーザーは、Experience PlatformUI を使用して、対応する顧客データフィールドで設定した様々なオプションの中から選択できます。

```json
  "fileConfigurations": {
    "compression": {
      "templatingStrategy": "PEBBLE_V1",
      "value": "{% if customerData contains 'compression' and customerData.compression is not empty %}{{customerData.compression}}{% else %}NONE{% endif %}"
    },
    "fileType": {
      "templatingStrategy": "PEBBLE_V1",
      "value": "{{customerData.fileType}}"
    },
    "csvOptions": {
      "sep": {
        "templatingStrategy": "PEBBLE_V1",
        "value": "{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'delimiter' %}{{customerData.csvOptions.delimiter}}{% else %},{% endif %}"
      },
      "quote": {
        "templatingStrategy": "PEBBLE_V1",
        "value": "{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'quote' %}{{customerData.csvOptions.quote}}{% else %}\"{% endif %}"
      },
      "escape": {
        "templatingStrategy": "PEBBLE_V1",
        "value": "{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'escape' %}{{customerData.csvOptions.escape}}{% else %}\\{% endif %}"
      },
      "nullValue": {
        "templatingStrategy": "PEBBLE_V1",
        "value": "{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'nullValue' %}{{customerData.csvOptions.nullValue}}{% else %}null{% endif %}"
      },
      "emptyValue": {
        "templatingStrategy": "PEBBLE_V1",
        "value": "{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'emptyValue' %}{{customerData.csvOptions.emptyValue}}{% else %}{% endif %}"
      }
    }
  }
```

### サポートされているファイル形式のオプションに関する完全なリファレンスと例 {#file-formatting-reference-and-example}

>[!TIP]
>
>以下に説明する CSV ファイル形式設定オプションも、 [CSV ファイル用 Apache Spark ガイド](https://spark.apache.org/docs/latest/sql-data-sources-csv.html). 以下で使用する説明は、Apache Spark ガイドから取得したものです。

以下に、Destination SDKで使用可能なすべてのファイル形式設定オプションと、各オプションの出力例を示します。

| フィールド | 必須／オプション | 説明 | デフォルト値 | 出力例 1 | 出力例 2 |
|---|---|---|---|---|---|
| `templatingStrategy` | 必須 | 設定する各ファイルフォーマットオプションに対して、パラメーターを追加する必要があります `templatingStrategy`:2 つの値を持つことができます。 <br><ul><li>`NONE`:ユーザーが設定に対して異なる値を選択できない場合は、この値を使用します。 詳しくは、 [この設定](#file-configuration-templating-none) 例えば、ファイル形式設定オプションが修正されている場合などです。</li><li>`PEBBLE_V1`:ユーザーが設定に対して異なる値から選択できるようにする場合は、この値を使用します。 この場合、 `/destination` エンドポイントの設定を使用して、UI の様々なオプションをユーザーに表示します。 詳しくは、 [この設定](#file-configuration-templating-pebble) 例えば、ユーザーがファイル形式設定オプションの異なる値から選択できる場合です。</li></ul> | - | - | - |
| `compression.value` | オプション | データをファイルに保存する際に使用する圧縮コーデック。 サポートされている値：`none`、`bzip2`、`gzip`、`lz4`、`snappy`。 | `none` | - | - |
| `fileType.value` | オプション | 出力ファイル形式を指定します。 サポートされている値： `csv`、`parquet`、`json`。 | `csv` | - | - |
| `csvOptions.quote.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用された値をエスケープするために使用する 1 文字を設定します。区切り記号を値の一部として使用することもできます。 | `null` | - | - |
| `csvOptions.quoteAll.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。すべての値を常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含む値のみをエスケープします。 | `false` | `quoteAll`:`false` —> `male,John,"TestLastName"` | `quoteAll`:`true` —>`"male","John","TestLastName"` |
| `csvOptions.delimiter.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。各フィールドと値の区切り文字を設定します。 この区切り文字には、1 つ以上の文字を使用できます。 | `,` | `delimiter`:`,` —> `comma-separated values"` | `delimiter`:`\t` —> `tab-separated values` |
| `csvOptions.escape.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。 既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。 | `\` | `"escape"`:`"\\"` —> `male,John,"Test,\"LastName5"` | `"escape"`:`"'"` —> `male,John,"Test,'''"LastName5"` |
| `csvOptions.escapeQuotes.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符を含む値を、常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含むすべての値をエスケープします。 | `true` | - | - |
| `csvOptions.header.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。エクスポートされたファイルの最初の行として列の名前を書き込むかどうかを示します。 | `true` | - | - |
| `csvOptions.ignoreLeadingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から先頭の空白をトリミングするかどうかを示します。 | `true` | `ignoreLeadingWhiteSpace`:`true` —> `"male","John","TestLastName"` | `ignoreLeadingWhiteSpace`:`false`—> `"    male","John","TestLastName"` |
| `csvOptions.ignoreTrailingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から末尾の空白をトリミングするかどうかを示します。 | `true` | `ignoreTrailingWhiteSpace`:`true` —> `"male","John","TestLastName"` | `ignoreTrailingWhiteSpace`:`false`—> `"male    ","John","TestLastName"` |
| `csvOptions.nullValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。null 値の文字列表現を設定します。 | `""` | `nullvalue`:`""` —> `male,"",TestLastName` | `nullvalue`:`"NULL"` —> `male,NULL,TestLastName` |
| `csvOptions.dateFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。日付の形式を示します。 | `yyyy-MM-dd` | `dateFormat`:`yyyy-MM-dd` —> `male,TestLastName,John,2022-02-24` | `dateFormat`:`MM/dd/yyyy` —> `male,TestLastName,John,02/24/2022` |
| `csvOptions.timestampFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。タイムスタンプ形式を示す文字列を設定します。 | `yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX]` | - | - |
| `csvOptions.charToEscapeQuoteEscaping.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符文字のエスケープに使用する 1 文字を設定します。 | エスケープ文字と引用符文字が異なる場合は `\`。 エスケープ文字と引用符文字が同じ場合は `\0` を使用します。 | - | - |
| `csvOptions.emptyValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。空の値の文字列表現を設定します。 | `""` | `"emptyValue":""` --> `male,"",John` | `"emptyValue":"empty"` —> `male,empty,John` |

{style=&quot;table-layout:auto&quot;}