---
description: ファイルベースの宛先のファイル形式オプションの設定
title: Destination SDKを使用して、ファイルベースの宛先のファイル形式オプションを設定する方法を説明します。
exl-id: e61c7989-1123-4b3b-9781-a6097cd0e2b4
source-git-commit: d47c82339afa602a9d6914c1dd36a4fc9528ea32
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 22%

---

# ファイルベースの宛先のファイル形式オプションの設定

## 概要 {#overview}

Destination SDKを使用すると、書き出したファイルの書式設定と圧縮のオプションを、ストレージの場所のダウンストリーム要件に合わせて大幅に調整できます。

ここでは、Destination SDKを使用して、ファイルベースの宛先のファイル形式オプションを設定する方法について説明します。

## 前提条件 {#prerequisites}

以下に説明する手順に進む前に、[Destination SDKの概要 &#x200B;](../../getting-started.md) ページを参照して、Adobe I/ODestination SDK資格情報および認証 API を使用するために必要なその他の前提条件について確認してください。

Adobeでは、先に進む前に次のドキュメントを読み、内容を理解しておくこともお勧めします。

* 使用可能なすべてのファイル形式オプションについては、[&#x200B; ファイル形式設定 &#x200B;](../../functionality/destination-server/file-formatting.md) セクションで詳しく説明しています。
* Destination SDKを使用して [&#x200B; ファイルベースの宛先を設定 &#x200B;](../../guides/configure-file-based-destination-instructions.md) する手順を実行します。

## サーバーとファイル設定の作成 {#create-server-file-configuration}

まず `/destination-server` エンドポイントを使用して、書き出したファイルに設定するファイル形式設定オプションを決定します。

以下は、複数のファイル形式オプションが選択された、[!DNL Amazon S3] 宛先の宛先サーバー設定の例です。

**API 形式**

```http
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-server \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
{
  "name": "Amazon S3 Server with several CSV Options",
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
}
}'
```

## 宛先設定へのファイル形式オプションの追加 {#create-destination-configuration}

>[!TIP]
>
>**Experience PlatformUI を確認します**。 以下の節で説明する設定でファイル形式オプションを設定する場合は、Experience Platform UI でこれらのオプションのレンダリング方法を確認する必要があります。

前の手順で、目的のファイル形式オプションを宛先サーバーに追加し、ファイル形式設定を追加したら、`/destinations` API エンドポイントを使用して、目的のフィールドを顧客データフィールドとして宛先設定に追加できるようになりました。

>[!IMPORTANT]
>
>この手順はオプションで、Experience PlatformUI でユーザーに表示するファイル形式オプションを指定するだけです。 顧客データフィールドとしてファイル形式オプションを設定しない場合、ファイルの書き出しは、[&#x200B; サーバーおよびファイル設定 &#x200B;](#create-server-file-configuration) で設定されたデフォルト値で続行されます。

この手順では、表示されるオプションを任意の順序でグループ化し、選択したファイルタイプに基づいて、カスタムグループ化、ドロップダウンフィールド、条件付きグループを作成できます。 これらの設定はすべて、録画と後述の節で説明します。

![&#x200B; バッチファイルの様々なファイル形式オプションを示す画面録画。](../../assets/guides/batch/file-formatting-options.gif)

### ファイル形式オプションの並べ替え {#ordering}

宛先設定でファイル形式オプションを顧客データフィールドとして追加した順序は、UI に反映されます。 例えば、以下の設定は、UI にそのまま反映され、オプションは **[!UICONTROL 区切り文字]**、**[!UICONTROL 引用符文字]**、**[!UICONTROL エスケープ文字]**、**[!UICONTROL 空の値]**、**[!UICONTROL Null 値]** の順序で表示されます。

![Experience Platform UI でのファイル形式オプションの順序を示す画像。](../../assets/guides/batch/file-formatting-order.png)

```json
        {
            "name": "csvOptions",
            "title": "CSV Options",
            "description": "Select your CSV options",
            "type": "object",
            "properties": [
                {
                    "name": "delimiter",
                    "title": "Delimiter",
                    "description": "Select your Delimiter",
                    "type": "string",
                    "isRequired": false,
                    "default": ",",
                    "namedEnum": [
                        {
                            "name": "Comma (,)",
                            "value": ","
                        },
                        {
                            "name": "Tab (\\t)",
                            "value": "\t"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "quote",
                    "title": "Quote Character",
                    "description": "Select your Quote character",
                    "type": "string",
                    "isRequired": false,
                    "default": "\u0000",
                    "namedEnum": [
                        {
                            "name": "Double Quotes (\")",
                            "value": "\""
                        },
                        {
                            "name":"Null Character (\u0000)",
                            "value": ""
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "escape",
                    "title": "Escape Character",
                    "description": "Select your Escape character",
                    "type": "string",
                    "isRequired": false,
                    "default": "\\",
                    "namedEnum": [
                        {
                            "name": "Back Slash (\\)",
                            "value": "\\"
                        },
                        {
                            "name": "Single Quote (')",
                            "value": "'"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "emptyValue",
                    "title": "Empty Value",
                    "description": "Select the output value of blank fields",
                    "type": "string",
                    "isRequired": false,
                    "default": "",
                    "namedEnum": [
                        {
                            "name": "Empty String",
                            "value": ""
                        },
                        {
                            "name": "\"\"",
                            "value": "\"\""
                        },
                        {
                            "name": "null",
                            "value": "null"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "nullValue",
                    "title": "Null Value",
                    "description": "Select the output value of 'null' fields",
                    "type": "string",
                    "isRequired": false,
                    "default": "null",
                    "namedEnum": [
                        {
                            "name": "Empty String",
                            "value": ""
                        },
                        {
                            "name": "\"\"",
                            "value": "\"\""
                        },
                        {
                            "name": "null",
                            "value": "null"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                }
```

### ファイル形式オプションのグループ化 {#grouping}

複数のファイル形式オプションを 1 つのセクション内にグループ化できます。 UI で宛先への接続を設定する際に、ユーザーは、類似したフィールドを視覚的にグループ化することで、メリットが得られます。

これを行うには、`"type": "object"` を使用してグループを作成し、以下の例に示すように、`properties` パラメーター内に目的のファイル形式オプションを収集します **[!UICONTROL CSV オプション]** のグループ化がハイライト表示されています。

```json {line-numbers="true" start-number="100" highlight="106-128"}
"customerDataFields":[
[...]
{
   "name":"csvOptions",
   "title":"CSV Options",
   "description":"Select your CSV options",
   "type":"object",
   "properties":[
      {
         "name":"delimiter",
         "title":"Delimiter",
         "description":"Select your Delimiter",
         "type":"string",
         "isRequired":false,
         "default":",",
         "namedEnum":[
            {
               "name":"Comma (,)",
               "value":","
            },
            {
               "name":"Tab (\\t)",
               "value":"\t"
            }
         ],
         "readOnly":false,
         "hidden":false
      },
      [...]
   ]
}
[...]
]
```

![UI での CSV オプションのグループ化を示す画像。](../../assets/guides/batch/file-formatting-grouping.png)

### ファイル形式オプション用のドロップダウンセレクターの作成 {#dropdown-selectors}

ユーザーがいくつかのオプションから選択できるようにしたい状況（例えば、CSV ファイルのフィールドを区切るためにどの文字を使用するか）では、UI にドロップダウンフィールドを追加できます。

これを行うには、以下に示すように、`namedEnum` オブジェクトを使用して、ユーザーが選択できるオプションの `default` 値を設定します。

```json {line-numbers="true" start-number="100" highlight="114-124"}
[...]
"customerDataFields":[
[...]
{
   "name":"csvOptions",
   "title":"CSV Options",
   "description":"Select your CSV options",
   "type":"object",
   "properties":[
      {
         "name":"delimiter",
         "title":"Delimiter",
         "description":"Select your Delimiter",
         "type":"string",
         "isRequired":false,
         "default":",",
         "namedEnum":[
            {
               "name":"Comma (,)",
               "value":","
            },
            {
               "name":"Tab (\\t)",
               "value":"\t"
            }
         ],
         "readOnly":false,
         "hidden":false
      },
      [...]
   ]
}
[...]
]
```

![上記の設定で作成されたドロップダウンセレクターの例を示す画面録画。](../../assets/guides/batch/dropdown-options-file-formatting.gif)

### 条件付きファイル形式オプションの作成 {#conditional-options}

条件付きファイル形式オプションを作成できます。このオプションは、ユーザーが書き出し用に特定のファイルタイプを選択する場合にのみ、アクティベーションワークフローに表示されます。 例えば、以下の設定は、CSV ファイルオプション用の条件グループを作成します。 CSV ファイルオプションは、ユーザーが CSV を書き出し用のファイルタイプとして選択する場合にのみ表示されます。

フィールドを条件付きとして設定するには、以下に示すように、`conditional` パラメーターを使用します。

```json
            "conditional": {
                "field": "fileType",
                "operator": "EQUALS",
                "value": "CSV"
            }
```

より広いコンテキストでは、以下の宛先設定で、`fileType` 文字列とそれが定義されている `csvOptions` オブジェクトと共に `conditional` フィールドが使用されているのを確認できます。

```json
        {
            "name": "fileType",
            "title": "File Type",
            "description": "Select your file type",
            "type": "string",
            "isRequired": true,
            "enum": [
                "PARQUET",
                "CSV",
                "JSON"
            ],
            "readOnly": false,
            "hidden": false
        },
        {
            "name": "csvOptions",
            "title": "CSV Options",
            "description": "Select your CSV options",
            "type": "object",
            "conditional": {
                "field": "fileType",
                "operator": "EQUALS",
                "value": "CSV"
            },            
            "properties": [
                {
                    "name": "delimiter",
                    "title": "Delimiter",
                    "description": "Select your Delimiter",
                    "type": "string",
                    "isRequired": false,
                    "default": ",",
                    "namedEnum": [
                        {
                            "name": "Comma (,)",
                            "value": ","
                        },
                        {
                            "name": "Tab (\\t)",
                            "value": "\t"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "quote",
                    "title": "Quote Character",
                    "description": "Select your Quote character",
                    "type": "string",
                    "isRequired": false,
                    "default": "",
                    "namedEnum": [
                        {
                            "name": "Double Quotes (\")",
                            "value": "\""
                        },
                        {
                            "name":"Null Character (\u0000)",
                            "value": "\u0000"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "escape",
                    "title": "Escape Character",
                    "description": "Select your Escape character",
                    "type": "string",
                    "isRequired": false,
                    "default": "\\",
                    "namedEnum": [
                        {
                            "name": "Back Slash (\\)",
                            "value": "\\"
                        },
                        {
                            "name": "Single Quote (')",
                            "value": "'"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "emptyValue",
                    "title": "Empty Value",
                    "description": "Select the output value of blank fields",
                    "type": "string",
                    "isRequired": false,
                    "default": "",
                    "namedEnum": [
                        {
                            "name": "Empty String",
                            "value": ""
                        },
                        {
                            "name": "\"\"",
                            "value": "\"\""
                        },
                        {
                            "name": "null",
                            "value": "null"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                },
                {
                    "name": "nullValue",
                    "title": "Null Value",
                    "description": "Select the output value of 'null' fields",
                    "type": "string",
                    "isRequired": false,
                    "default": "null",
                    "namedEnum": [
                        {
                            "name": "Empty String",
                            "value": ""
                        },
                        {
                            "name": "\"\"",
                            "value": "\"\""
                        },
                        {
                            "name": "null",
                            "value": "null"
                        }
                    ],
                    "readOnly": false,
                    "hidden": false
                }
            ],
            "isRequired": false,
            "readOnly": false,
            "hidden": false
        }
```

以下に、上記の設定に基づいた結果の UI 画面を確認できます。ユーザーがファイルタイプで CSV を選択すると、CSV ファイルタイプを参照する追加のファイル形式オプションが UI に表示されます。

![CSV ファイルの条件付きファイル形式オプションを示す画面録画。](../../assets/guides/batch/conditional-file-formatting.gif)

### 上記のすべてのオプションを含む完全な API リクエスト

以下の API リクエストは、上記の節で説明するすべてのオプションを 1 つの設定にまとめたものです。

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
  "name": "My S3 Destination",
  "description": "Test destination",
  "status": "TEST",
  "sources": [
    "UNIFIED_PROFILE"
  ],
  "customerAuthenticationConfigurations": [
    {
      "authType": "S3"
    }
  ],
  "customerDataFields": [
    {
      "name": "bucket",
      "type": "string",
      "title": "Bucket",
      "description": "Enter your S3 Bucket",
      "isRequired": true
    },
    {
      "name": "path",
      "type": "string",
      "title": "Path",
      "description": "Enter your S3 Path",
      "isRequired": true
    },
    {
      "name": "fileType",
      "type": "string",
      "enum": [
        "CSV",
        "JSON",
        "PARQUET"
      ],
      "title": "File Type",
      "description": "Select your file type",
      "isRequired": true
    },
    {
      "name": "csvOptions",
      "type": "object",
      "title": "CSV Options",
      "description": "Select your CSV options",
      "conditional": {
        "field": "fileType",
        "operator": "EQUALS",
        "value": "CSV"
      },
      "properties": [
        {
          "name": "delimiter",
          "type": "string",
          "title": "Delimiter",
          "description": "Select your Delimiter",
          "namedEnum": [
            {
              "name": "Comma (,)",
              "value": ","
            },
            {
              "name": "Tab (\\t)",
              "value": "\t"
            }
          ],
          "default": ","
        },
        {
          "name": "quote",
          "type": "string",
          "title": "Quote Character",
          "description": "Select your Quote character",
          "namedEnum": [
            {
              "name": "Double Quotes (\")",
              "value": "\""
            },
            {
              "name": "Null Character (\\u0000)",
              "value": "\u0000"
            }
          ],
          "default": "\u0000"
        },
        {
          "name": "escape",
          "type": "string",
          "title": "Escape Character",
          "description": "Select your Escape character",
          "namedEnum": [
            {
              "name": "Back Slash (\\)",
              "value": "\\"
            },
            {
              "name": "Single Quote (')",
              "value": "'"
            }
          ],
          "default": "\\"
        },
        {
          "name": "emptyValue",
          "type": "string",
          "title": "Empty Value",
          "description": "Select the output value of blank fields",
          "namedEnum": [
            {
              "name": "null",
              "value": "null"
            },
            {
              "name": "Empty String",
              "value": ""
            },
            {
              "name": "\"\"",
              "value": "\"\""
            }
          ],
          "default": ""
        },
        {
          "name": "nullValue",
          "type": "string",
          "title": "Null Value",
          "description": "Select the output value of 'null' fields",
          "namedEnum": [
            {
              "name": "null",
              "value": "null"
            },
            {
              "name": "Empty String",
              "value": ""
            },
            {
              "name": "\"\"",
              "value": "\"\""
            }
          ],
          "default": "null"
        }
      ]
    }
  ],
  "uiAttributes": {
    "documentationLink": "https://www.adobe.com/go/aep",
    "category": "cloudStorage",
    "connectionType": "Server-to-server",
    "frequency": "Batch",
    "monitoringSupported": true,
    "flowRunsSupported": true
  },
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
    "defaultStartTime": "00:00",
    "filenameConfig": {
      "allowedFilenameAppendOptions": [
        "SEGMENT_NAME",
        "DESTINATION_INSTANCE_ID",
        "DESTINATION_INSTANCE_NAME",
        "ORGANIZATION_NAME",
        "SANDBOX_NAME",
        "DATETIME",
        "CUSTOM_TEXT"
      ],
      "defaultFilenameAppendOptions": [
        "DATETIME"
      ],
      "defaultFilename": "%DESTINATION%_%SEGMENT_ID%"
    }
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
      "destinationServerId": "<server-id>"
    }
  ]
}'
```

応答が成功すると、設定の一意の ID （`instanceId`）を含む宛先設定が返されます。

## 既知の制限事項 {#known-limitations}

ファイル形式オプションの特定の組み合わせが、望ましくないファイル書き出しの結果につながる可能性があります。
Adobeでは、次の CSV オプションの組み合わせを選択しないことをお勧めします。

```
nullValue -> ""
quote -> "
emptyValue -> ""
```

制限の例として、次の値を持つファイルのエクスポートを考えてみましょう。

| 名 | 姓 | 国 | state |
|---------|----------|---------|--------|
| マイケル | Rose | 米国 | NY |
| ジェームズ | Smith |  | null |

{style="table-layout:auto"}

その結果、次のような出力が得られます。 テーブルの null 値が誤ってエスケープされた引用符として書き出されていることに注意してください。

```csv
Michael,Rose,USA,NY 
James,Smith,"","\"\""
```

## 次の手順 {#next-steps}

この記事では、Destination SDKを使用して、書き出したファイルにカスタムのファイル形式オプションを設定する方法を確認しました。 次に、チームは [&#x200B; ファイルベース宛先のアクティベーションワークフロー &#x200B;](../../../ui/activate-batch-profile-destinations.md) を使用して、宛先にデータを書き出すことができます。
