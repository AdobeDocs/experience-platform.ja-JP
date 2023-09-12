---
description: 「/destination-servers」エンドポイントを介して Adobe Experience Platform Destination SDK で作成されたファイルベースの宛先に対するファイル形式オプションの設定方法を説明します。
title: ファイル形式設定
source-git-commit: 4f4ffc7fc6a895e529193431aba77d6f3dcafb6f
workflow-type: tm+mt
source-wordcount: '1093'
ht-degree: 88%

---


# ファイル形式設定

Destination SDK は、統合のニーズに応じて設定できる、柔軟な機能のセットをサポートします。その中に、[!DNL CSV] ファイル形式をサポートする機能があります。

Destination SDK でファイルベースの宛先を作成する場合、書き出された CSV ファイルがどのように書式設定されるべきかを定義できます。以下のように、多くの形式オプションをカスタマイズできます（ただし、これに限定されません）。

* CSV ファイルにヘッダーが含まれる必要があるかどうか
* 値の引用にどの文字を使用するか
* 空の値をどのように表示すべきか。

宛先設定に応じて、ファイルベースの宛先に接続する際に、ユーザーには UI で特定のオプションが表示されます。これらのオプションがどのように見えるかを確認するには、[ファイルベースの宛先に対するファイル形式オプション](../../../ui/batch-destinations-file-formatting-options.md)ドキュメントを参照してください。


ファイル形式設定は、ファイルベースの宛先に対する宛先サーバー設定の一部です。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration).

`/authoring/destination-servers` エンドポイントを介してファイル形式オプションを設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先サーバー設定の作成](../../authoring-api/destination-server/create-destination-server.md)
* [宛先サーバー設定の更新](../../authoring-api/destination-server/update-destination-server.md)

このページでは、書き出された `CSV` ファイルでサポートされるすべてのファイル形式設定を説明します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | × |
| ファイルベースの（バッチ）統合 | ○ |

## サポートされるパラメーター {#supported-parameters}

Experience Platform から受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、宛先のファイル受け取りシステムの要件に合わせて変更できます。

>[!NOTE]
>
>CSV オプションは、CSV ファイルの書き出し時にのみサポートされます。新しい宛先サーバーを設定する際、`fileConfigurations` セクションは必須ではありません。CSV オプションの API 呼び出しで値を渡さない場合、[後述の参照テーブル](#file-formatting-reference-and-example)のデフォルト値が使用されます。


## ユーザーが設定オプションを選択できない CSV オプション {#file-configuration-templating-none}

以下の設定例では、すべての CSV オプションが事前定義済みです。各 `csvOptions` パラメーターで定義された書き出し設定は、最終的なもので、ユーザーが変更することはできません。

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
        "maxFileRowCount":5000000,
        "includeFileManifest": {
            "templatingStrategy":"PEBBLE_V1",
            "value":"{{ customerData.includeFileManifest }}"
      }
    }
```

## ユーザーが設定オプションを選択できる CSV オプション {#file-configuration-templating-pebble}

以下の設定例では、どの CSV オプションも事前定義されていません。各 `csvOptions` パラメーターの `value` は、`/destinations` エンドポイントを通じて、対応する顧客データフィールドで設定され（例：`quote` ファイル形式オプションの [`customerData.quote`](../../functionality/destination-configuration/customer-data-fields.md#conditional-options)）、ユーザーは、Experience Platform UI を使用して、対応する顧客データフィールドで設定する様々なオプションを選択できます。これらのオプションがどのように見えるかを確認するには、[ファイルベースの宛先に対するファイル形式オプション](../../../ui/batch-destinations-file-formatting-options.md)ドキュメントを参照してください。

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
      },
      "maxFileRowCount":5000000,
      "includeFileManifest": {
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{ customerData.includeFileManifest }}"
      }
   }
}
```

## サポートされるファイル形式オプションの完全なリファレンスおよび例 {#file-formatting-reference-and-example}

>[!TIP]
>
>後述の CSV ファイル形式オプションは、[CSV ファイル用 Apache Spark ガイド](https://spark.apache.org/docs/latest/sql-data-sources-csv.html)にもドキュメント化されています。以下で使用される説明は、Apache Spark ガイドから引用しています。

以下は、各オプションの出力例を含む、Destination SDK で使用できるすべてのファイル形式オプションの完全なリファレンスです。

| フィールド | 必須／オプション | 説明 | デフォルト値 | 出力例 1 | 出力例 2 |
|---|---|---|---|---|---|
| `templatingStrategy` | 必須 | 設定する各ファイル形式オプションについて、パラメーター `templatingStrategy` を追加する必要があります。このパラメーターは、以下の 2 つの値を持つことができます。<br><ul><li>`NONE`：ユーザーが設定で異なる値を選択できるようにする予定のない場合は、この値を使用します。ファイル形式オプションが固定される例については、[この設定](#file-configuration-templating-none)を参照してください。</li><li>`PEBBLE_V1`：ユーザーが設定で異なる値を選択できるようにする場合は、この値を使用します。この場合、UI でユーザーに様々なオプションを表示するために、`/destination` エンドポイント設定で対応する顧客データフィールドも設定する必要があります。ユーザーがファイル形式オプションで異なる値を選択できる例については、[この設定](#file-configuration-templating-pebble)を参照してください。</li></ul> | - | - | - |
| `compression.value` | オプション | データをファイルに保存する際に使用する圧縮コーデック。サポートされている値：`none`、`bzip2`、`gzip`、`lz4`、`snappy`。 | `none` | - | - |
| `fileType.value` | オプション | 出力ファイル形式を指定します。サポートされている値：`csv`、`parquet`、`json`。 | `csv` | - | - |
| `csvOptions.quote.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用された値をエスケープするために使用する 1 文字を設定します。区切り記号を値の一部として使用することもできます。 | `null` | デフォルト値の例： `quote.value: "u0000"` —> `male,NULJohn,LastNameNUL` | カスタムの例： `quote.value: "\""` —> `male,"John,LastName"` |
| `csvOptions.quoteAll.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。すべての値を常に引用符で囲む必要があるかどうかを示します。デフォルトでは、引用符文字を含む値のみをエスケープします。 | `false` | `quoteAll`:`false` --> `male,John,"TestLastName"` | `quoteAll`:`true` -->`"male","John","TestLastName"` |
| `csvOptions.delimiter.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。各フィールドと値の区切り文字を設定します。この区切り文字には、1 つまたは複数の文字を指定できます。 | `,` | `delimiter`:`,` --> `comma-separated values"` | `delimiter`:`\t` --> `tab-separated values` |
| `csvOptions.escape.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。 | `\` | `"escape"`:`"\\"` --> `male,John,"Test,\"LastName5"` | `"escape"`:`"'"` --> `male,John,"Test,'''"LastName5"` |
| `csvOptions.escapeQuotes.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符を含む値を、常に引用符で囲む必要があるかどうかを示します。デフォルトでは、引用符文字を含むすべての値をエスケープします。 | `true` | - | - |
| `csvOptions.header.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。列の名前を書き出されたファイルの最初の行として記述するかどうかを示します。 | `true` | - | - |
| `csvOptions.ignoreLeadingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から先頭の空白をトリミングするかどうかを示します。 | `true` | `ignoreLeadingWhiteSpace`:`true` --> `"male","John","TestLastName"` | `ignoreLeadingWhiteSpace`:`false`--> `"    male","John","TestLastName"` |
| `csvOptions.ignoreTrailingWhiteSpace.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。値から末尾の空白をトリミングするかどうかを示します。 | `true` | `ignoreTrailingWhiteSpace`:`true` --> `"male","John","TestLastName"` | `ignoreTrailingWhiteSpace`:`false`--> `"male    ","John","TestLastName"` |
| `csvOptions.nullValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。null 値の文字列表現を設定します。 | `""` | `nullvalue`:`""` --> `male,"",TestLastName` | `nullvalue`:`"NULL"` --> `male,NULL,TestLastName` |
| `csvOptions.dateFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。日付の形式を示します。 | `yyyy-MM-dd` | `dateFormat`:`yyyy-MM-dd` --> `male,TestLastName,John,2022-02-24` | `dateFormat`:`MM/dd/yyyy` --> `male,TestLastName,John,02/24/2022` |
| `csvOptions.timestampFormat.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。タイムスタンプ形式を示す文字列を設定します。 | `yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX]` | - | - |
| `csvOptions.charToEscapeQuoteEscaping.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。引用符文字のエスケープに使用する 1 文字を設定します。 | エスケープ文字と引用符文字が異なる場合は `\`。エスケープ文字と引用符文字が同じ場合は `\0` を使用します。 | - | - |
| `csvOptions.emptyValue.value` | オプション | *`"fileType.value": "csv"`* の場合のみ。空の値の文字列表現を設定します。 | `""` | `"emptyValue":""` --> `male,"",John` | `"emptyValue":"empty"` --> `male,empty,John` |
| `maxFileRowCount` | オプション | 書き出されるファイルごとの最大行数を 1,000,000～10,000,000 行の範囲で示します。 | 5,000,000 |
| `includeFileManifest` | オプション | ファイルの書き出しと共に、ファイルマニフェストの書き出しをサポートします。 マニフェスト JSON ファイルには、書き出し場所や書き出しサイズなどに関する情報が含まれています。 マニフェストの名前は、形式を使用して付けられます `manifest-<<destinationId>>-<<dataflowRunId>>.json`. | を表示します。 [サンプルマニフェストファイル](/help/destinations/assets/common/manifest-d0420d72-756c-4159-9e7f-7d3e2f8b501e-0ac8f3c0-29bd-40aa-82c1-f1b7e0657b19.json). マニフェストファイルには、次のフィールドが含まれます。 <ul><li>`flowRunId`: [データフローの実行](/help/dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) 書き出されたファイルを生成した</li><li>`scheduledTime`：ファイルが書き出されたときの UTC 時刻 (UTC)。 </li><li>`exportResults.sinkPath`：書き出されたファイルが格納されるストレージの場所のパス。 </li><li>`exportResults.name`：書き出されたファイルの名前。</li><li>`size`：書き出されるファイルのサイズ（バイト単位）。</li></ul> |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むことで、宛先サーバー設定でのファイル形式の仕組み、およびどのように設定できるかについて、理解を深めることができました。

その他の宛先サーバーコンポーネントについて詳しくは、以下の記事を参照してください。

* [Destination SDK で作成される宛先のサーバー仕様](server-specs.md)
* [テンプレートの仕様](templating-specs.md)
* [メッセージ形式](message-format.md)