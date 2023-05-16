---
description: 「/destination-servers」エンドポイントを介して、Adobe Experience Platform Destination SDKを使用して構築したファイルベースの宛先のファイル形式オプションを設定する方法について説明します。
title: ファイル形式設定
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 25%

---


# ファイル形式設定

Destination SDKは、統合のニーズに応じて設定できる、柔軟な機能セットをサポートします。 これらの機能の中で、は [!DNL CSV] ファイルのフォーマット。

Destination SDKを使用してファイルベースの宛先を作成する場合、書き出した CSV ファイルの書式設定方法を定義できます。 次のような様々な書式設定オプションをカスタマイズできます（ただし、これらに限定されません）。

* CSV ファイルにヘッダーを含めるかどうか。
* 値の引用符に使用する文字
* 空の値はどのように表示されます。

宛先の設定に応じて、ファイルベースの宛先に接続する際に、UI に特定のオプションが表示されます。 これらのオプションは、 [ファイルベースの宛先のファイル形式オプション](../../../ui/batch-destinations-file-formatting-options.md) ドキュメント。


ファイル形式設定は、ファイルベースの宛先の宛先サーバー設定の一部です。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration).

ファイルフォーマットオプションは、 `/authoring/destination-servers` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先サーバー設定の作成](../../authoring-api/destination-server/create-destination-server.md)
* [宛先サーバー設定の更新](../../authoring-api/destination-server/update-destination-server.md)

このページでは、書き出しでサポートされるすべてのファイル形式設定について説明します `CSV` ファイル。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | いいえ |
| ファイルベース（バッチ）の統合 | ○ |

## サポートされているパラメーター {#supported-parameters}

Experience Platformから受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、宛先のファイル受け取りシステムの要件に合わせて変更できます。

>[!NOTE]
>
>CSV オプションは、CSV ファイルの書き出し時にのみサポートされます。 新しい宛先サーバーを設定する際、`fileConfigurations` セクションは必須ではありません。CSV オプションの API 呼び出しに値を渡さない場合、 [下の参照表](#file-formatting-reference-and-example) が使用されます。


## ユーザーが設定オプションを選択できない CSV オプション {#file-configuration-templating-none}

以下の設定例では、すべての CSV オプションが事前に定義されています。 各 `csvOptions` パラメータは最終的なもので、ユーザーは変更できません。

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

## ユーザーが設定オプションを選択できる CSV オプション {#file-configuration-templating-pebble}

以下の設定例では、事前に定義された CSV オプションはありません。 この `value` それぞれの `csvOptions` パラメーターは、 `/destinations` エンドポイント ( 例： [`customerData.quote`](../../functionality/destination-configuration/customer-data-fields.md#conditional-options) の `quote` ファイルフォーマットオプションを参照 ) およびユーザーは、Experience PlatformUI を使用して、対応する顧客データフィールドで設定した様々なオプションの中から選択できます。 これらのオプションは、 [ファイルベースの宛先のファイル形式オプション](../../../ui/batch-destinations-file-formatting-options.md) ドキュメント。

```json
{
   "fileConfigurations":{
      "compression":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{% if customerData contains 'compression' and customerData.compression is not empty %}{{customerData.compression}}{% else %}NONE{% endif %}"
      },
      "fileType":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.fileType}}"
      },
      "csvOptions":{
         "sep":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'delimiter' %}{{customerData.csvOptions.delimiter}}{% else %},{% endif %}"
         },
         "quote":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'quote' %}{{customerData.csvOptions.quote}}{% else %}\"{% endif %}"
         },
         "escape":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'escape' %}{{customerData.csvOptions.escape}}{% else %}\\{% endif %}"
         },
         "nullValue":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'nullValue' %}{{customerData.csvOptions.nullValue}}{% else %}null{% endif %}"
         },
         "emptyValue":{
            "templatingStrategy":"PEBBLE_V1",
            "value":"{% if customerData contains 'csvOptions' and customerData.csvOptions contains 'emptyValue' %}{{customerData.csvOptions.emptyValue}}{% else %}{% endif %}"
         }
      }
   }
}
```

## サポートされているファイル形式のオプションに関する完全なリファレンスと例 {#file-formatting-reference-and-example}

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
| `csvOptions.quoteAll.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。すべての値を常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含む値のみをエスケープします。 | `false` | `quoteAll`:`false` —> `male,John,"TestLastName"` | `quoteAll`:`true` -->`"male","John","TestLastName"` |
| `csvOptions.delimiter.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。各フィールドと値の区切り文字を設定します。 この区切り文字には、1 つ以上の文字を使用できます。 | `,` | `delimiter`:`,` --> `comma-separated values"` | `delimiter`:`\t` --> `tab-separated values` |
| `csvOptions.escape.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。 既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。 | `\` | `"escape"`:`"\\"` --> `male,John,"Test,\"LastName5"` | `"escape"`:`"'"` --> `male,John,"Test,'''"LastName5"` |
| `csvOptions.escapeQuotes.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符を含む値を、常に引用符で囲む必要があるかどうかを示します。 デフォルトでは、引用符文字を含むすべての値をエスケープします。 | `true` | - | - |
| `csvOptions.header.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。エクスポートされたファイルの最初の行として列の名前を書き込むかどうかを示します。 | `true` | - | - |
| `csvOptions.ignoreLeadingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から先頭の空白をトリミングするかどうかを示します。 | `true` | `ignoreLeadingWhiteSpace`:`true` --> `"male","John","TestLastName"` | `ignoreLeadingWhiteSpace`:`false`--> `"    male","John","TestLastName"` |
| `csvOptions.ignoreTrailingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から末尾の空白をトリミングするかどうかを示します。 | `true` | `ignoreTrailingWhiteSpace`:`true` --> `"male","John","TestLastName"` | `ignoreTrailingWhiteSpace`:`false`--> `"male    ","John","TestLastName"` |
| `csvOptions.nullValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。null 値の文字列表現を設定します。 | `""` | `nullvalue`:`""` --> `male,"",TestLastName` | `nullvalue`:`"NULL"` --> `male,NULL,TestLastName` |
| `csvOptions.dateFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。日付の形式を示します。 | `yyyy-MM-dd` | `dateFormat`:`yyyy-MM-dd` --> `male,TestLastName,John,2022-02-24` | `dateFormat`:`MM/dd/yyyy` --> `male,TestLastName,John,02/24/2022` |
| `csvOptions.timestampFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。タイムスタンプ形式を示す文字列を設定します。 | `yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX]` | - | - |
| `csvOptions.charToEscapeQuoteEscaping.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符文字のエスケープに使用する 1 文字を設定します。 | エスケープ文字と引用符文字が異なる場合は `\`。 エスケープ文字と引用符文字が同じ場合は `\0` を使用します。 | - | - |
| `csvOptions.emptyValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。空の値の文字列表現を設定します。 | `""` | `"emptyValue":""` --> `male,"",John` | `"emptyValue":"empty"` --> `male,empty,John` |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読んだ後、宛先サーバーの構成でのファイルの形式設定の仕組みと設定方法をより深く理解する必要があります。

その他の宛先サーバーコンポーネントの詳細については、次の記事を参照してください。

* [Destination SDK](server-specs.md)
* [テンプレート仕様](templating-specs.md)
* [メッセージの形式](message-format.md)