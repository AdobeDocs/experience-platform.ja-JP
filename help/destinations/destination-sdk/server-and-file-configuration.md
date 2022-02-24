---
description: ファイルベースの宛先のサーバーおよびファイル設定仕様は、 /destination-servers エンドポイントを介してAdobe Experience Platform Destination SDKで設定できます。
title: （ベータ版）ファイルベースの宛先サーバー仕様の構成オプション
source-git-commit: bc357e2e93b80edb5f7825bf2dee692f14bd7297
workflow-type: tm+mt
source-wordcount: '748'
ht-degree: 12%

---

# （ベータ版）ファイルベースの宛先サーバーの仕様に応じたサーバーとファイルの構成

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience Platform Destination SDKを使用してファイルベースの宛先を設定および送信する機能は、現在、ベータ版です。 ドキュメントと機能は変更される場合があります。

このページでは、ファイルベースの宛先サーバーのすべてのサーバー構成オプションの詳細を説明し、Experience Platformから宛先にファイルを書き出すユーザー向けの様々なファイル構成オプションの設定方法を示します。

ファイルベースの宛先のサーバーおよびファイル設定仕様は、 `/destination-servers` endpoint. 読み取り [宛先サーバー API エンドポイントの操作](./destination-server-api.md) エンドポイントで実行できる操作の完全なリストについては、を参照してください。

## ファイルベースのAmazon S3 宛先サーバー仕様 {#s3-example}

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
       // see File-based destinations file configuration
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 の場合 [!DNL Amazon S3]を設定し、 `FILE_BASED_S3`. |
| `fileBasedS3Destination.bucket.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedS3Destination.bucket.value` | 文字列 | の名前 [!DNL Amazon S3] この宛先で使用するバケット。 |
| `fileBasedS3Destination.path.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedS3Destination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |

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
       // see File-based destinations file configuration
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 の場合 [!DNL SFTP] 宛先，次に設定 `FILE_BASED_SFTP`. |
| `fileBasedSftpDestination.rootDirectory.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedSftpDestination.rootDirectory.value` | 文字列 | 宛先ストレージのルートディレクトリ。 |
| `fileBasedSftpDestination.hostName.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedSftpDestination.hostName.value` | 文字列 | 宛先ストレージのホスト名。 |
| `port` | 整数 | SFTP ファイルサーバーのポート。 |
| `encryptionMode` | 文字列 | ファイルの暗号化を使用するかどうかを示します。 サポートされている値： <ul><li>PGP</li><li>なし</li></ul> |

## ファイルベース [!DNL Azure Data Lake Storage] ([!DNL ADLS]) 宛先サーバーの仕様 {#adls-example}

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
       // see File-based destinations file configuration
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 の場合 [!DNL Azure Data Lake Storage] 宛先，次に設定 `FILE_BASED_ADLS_GEN2`. |
| `fileBasedAdlsGen2Destination.path.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedAdlsGen2Destination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |

## ファイルベース [!DNL Azure Blob Storage] 宛先サーバーの仕様 {#blob-example}

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
       // see File-based destinations file configuration
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 の場合 [!DNL Azure Blob Storage] 宛先，次に設定 `FILE_BASED_AZURE_BLOB`. |
| `fileBasedAzureBlobDestination.path.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedAzureBlobDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileBasedAzureBlobDestination.container.templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `fileBasedAzureBlobDestination.container.value` | 文字列 | の名前 [!DNL Azure Blob Storage] この宛先で使用するコンテナ。 |

## ファイルベース [!DNL Data Landing Zone] ([!DNL DLZ]) 宛先サーバーの仕様 {#dlz-example}

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
       // see File-based destinations file configuration
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 の場合 [!DNL Data Landing Zone] 宛先，次に設定 `FILE_BASED_DLZ`. |
| `fileBasedDlzDestination.path.templatingStrategy` | 文字列 | *必須*  用途 `PEBBLE_V1`. |
| `fileBasedDlzDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |

## ファイルベースの宛先ファイル設定 {#file-configuration}

ここでは、書き出すファイルのフォーマット設定について説明します `CSV` ファイル。 Experience Platformから受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、ユーザー側のファイル受け取りシステムの要件に合わせて変更できます。

>[!NOTE]
>
>CSV オプションは、CSV ファイルの書き出し時にのみサポートされます。 この `fileConfigurations` 新しい宛先サーバーを設定する場合、セクションは必須ではありません。 CSV オプションの API 呼び出しに値を渡さない場合、以下の表のデフォルト値が使用されます。

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
        }
    }
```

| フィールド | 必須／オプション | 説明 | デフォルト値 |
|---|---|---|---|
| `compression.value` | オプション | データをファイルに保存する際に使用する圧縮コーデック。 サポートされている値： `none`, `bzip2`, `gzip`, `lz4`、および `snappy`. | `none` |
| `fileType.value` | オプション | 出力ファイル形式を指定します。 サポートされている値： `csv`, `parquet`、および `json`. | `csv` |
| `csvOptions.quote.value` | オプション | *のみ`"fileType.value": "csv"`*. 引用符で囲まれた値をエスケープするために使用する、区切り記号を値の一部にする 1 文字を設定します。 | `null` |
| `csvOptions.quoteAll.value` | オプション | *のみ`"fileType.value": "csv"`*. すべての値を常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含む値のみをエスケープします。 | `false` |
| `csvOptions.escape.value` | オプション | *のみ`"fileType.value": "csv"`*. 既に引用符で囲まれた値内の引用符のエスケープに使用する 1 文字を設定します。 | `\` |
| `csvOptions.escapeQuotes.value` | オプション | *のみ`"fileType.value": "csv"`*. 引用符を含む値を常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含むすべての値をエスケープします。 | `true` |
| `csvOptions.header.value` | オプション | *のみ`"fileType.value": "csv"`*. 列の名前を最初の行として書き込むかどうかを示します。 | `true` |
| `csvOptions.ignoreLeadingWhiteSpace.value` | オプション | *のみ`"fileType.value": "csv"`*. 値から先頭の空白をトリミングするかどうかを示します。 | `true` |
| `csvOptions.ignoreTrailingWhiteSpace.value` | オプション | *のみ`"fileType.value": "csv"`*. 値から末尾の空白をトリミングするかどうかを示します。 | `true` |
| `csvOptions.nullValue.value` | オプション | *のみ`"fileType.value": "csv"`*. null 値の文字列表現を設定します。 | `""` |
| `csvOptions.dateFormat.value` | オプション | *のみ`"fileType.value": "csv"`*. 日付の形式を示します。 | `yyyy-MM-dd` |
| `csvOptions.timestampFormat.value` | オプション | *のみ`"fileType.value": "csv"`*. タイムスタンプ形式を示す文字列を設定します。 | `yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX]` |
| `csvOptions.charToEscapeQuoteEscaping.value` | オプション | *のみ`"fileType.value": "csv"`*. 引用符文字のエスケープに使用する 1 文字を設定します。 | `\` エスケープ文字と引用符文字が異なる場合。 `\0` エスケープ文字と引用符文字が同じ場合。 |
| `csvOptions.emptyValue.value` | オプション | *のみ`"fileType.value": "csv"`*. 空の値の文字列表現を設定します。 | `""` |
| `csvOptions.lineSep.value` | オプション | *のみ`"fileType.value": "csv"`*. 書き込みに使用する行区切り記号を定義します。 最大長は 1 文字です。 | `\n` |
