---
description: ファイルベースの宛先のファイル形式オプションの設定
title: Destination SDKを使用して、ファイルベースの宛先のファイル形式設定オプションを設定する方法について説明します。
source-git-commit: 9b4c7da5aa02ae27608c2841b1d825445ac3015e
workflow-type: tm+mt
source-wordcount: '932'
ht-degree: 2%

---

# ファイルベースの宛先のファイル形式オプションの設定

## 概要 {#overview}

Destination SDKを使用すると、ストレージの場所でのダウンストリーム要件に合わせて、書き出したファイルの形式と圧縮のオプションを大幅に調整できます。

このページでは、Destination SDKを使用して、ファイルベースの宛先のファイル形式設定オプションを設定する方法について説明します。

## 前提条件 {#prerequisites}

以下の手順に進む前に、 [Destination SDKの概要](../../getting-started.md) ページを参照してください。Adobe I/O認証に必要な資格情報や、Destination SDKAPI を使用するためのその他の前提条件が取得されます。

Adobeでは、先に進む前に、次のドキュメントを読み、よく理解しておくことをお勧めします。

* 使用可能なすべてのファイルフォーマットオプションについては、 [ファイルフォーマット設定](../../server-and-file-configuration.md#file-configuration) 」セクションに入力します。
* 次の手順を実行します。 [ファイルベースの宛先の設定](/help/destinations/destination-sdk/configure-file-based-destination-instructions.md) Destination SDKを使用。

## サーバーとファイル設定の作成 {#create-server-file-configuration}

まず、 `/destination-server` endpoint ：書き出されたファイルに対して設定するファイル形式設定オプションを決定します。

次に、 [!DNL Amazon S3] 宛先に書き出します。複数のファイル形式設定オプションが選択されています。

>[!TIP]
>
>利用可能なすべてのファイル形式設定オプションについては、 [ファイルフォーマット設定](../../server-and-file-configuration.md#file-configuration) 」セクションに入力します。

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

## 宛先設定にファイル形式設定オプションを追加する {#create-destination-configuration}

>[!TIP]
>
>**Experience PlatformUI の確認**. 以下の節で説明する設定を使用してファイル形式オプションを設定する場合は、Experience PlatformUI で、これらのオプションのレンダリング方法を確認する必要があります。

前の手順で、目的のファイルフォーマットオプションを宛先サーバーに追加し、ファイルフォーマット設定を設定した後、 `/destinations` 目的のフィールドを顧客データフィールドとして宛先設定に追加する API エンドポイント。

>[!IMPORTANT]
>
>この手順はオプションで、ユーザーに対してExperience PlatformUI で表示するファイル形式設定オプションを指定するだけです。 顧客データフィールドとしてファイルフォーマットオプションを設定しない場合、ファイルエクスポートは、 [サーバーとファイルの設定](#create-server-file-configuration).

この手順では、表示されたオプションを任意の順序でグループ化し、選択したファイルタイプに基づいてカスタムグループ化、ドロップダウンフィールド、条件付きグループを作成できます。 これらの設定はすべて、記録および後述のセクションに表示されます。

![バッチファイルの様々なファイル形式オプションを示す画面記録。](/help/destinations/destination-sdk/assets/guides/batch/file-formatting-options.gif)

### ファイル形式設定オプションの並べ替え {#ordering}

宛先設定で顧客データフィールドとしてファイル形式オプションを追加する順序は、UI に反映されます。 例えば、以下の設定は UI に応じて反映され、オプションは順番に表示されます **[!UICONTROL 区切り]**, **[!UICONTROL 引用符文字]**, **[!UICONTROL エスケープ文字]**, **[!UICONTROL 空の値]**, **[!UICONTROL Null 値]**.

![画像 UI のファイル形式設定オプションの順序を示すExperience Platform。](/help/destinations/destination-sdk/assets/guides/batch/file-formatting-order.png)

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

### ファイル形式設定オプションをグループ化します {#grouping}

複数のファイル形式設定オプションを 1 つのセクション内にグループ化できます。 UI で宛先への接続を設定すると、類似したフィールドを視覚的にグループ化して、その利点を活用することができます。

これをおこなうには、 `"type": "object"` グループを作成し、必要なファイルフォーマットオプションを `properties` 次の例に示すように、グループ化の場所のパラメーター **[!UICONTROL CSV オプション]** がハイライト表示されます。

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

[...]
```

![UI での CSV オプションのグループ化を示す画像。](/help/destinations/destination-sdk/assets/guides/batch/file-formatting-grouping.png)

### ファイル形式設定オプション用のドロップダウンセレクターを作成します {#dropdown-selectors}

CSV ファイルのフィールドを区切る文字など、複数のオプションから選択できるようにする場合は、UI にドロップダウンフィールドを追加できます。

これをおこなうには、 `namedEnum` 以下に示すようにオブジェクトを選択し、 `default` ユーザーが選択できるオプションの値。

```json
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
```

![上記の設定で作成されたドロップダウンセレクターの例を示す画面記録。](/help/destinations/destination-sdk/assets/guides/batch/dropdown-options-file-formatting.gif)

### 条件付きファイル書式設定オプションの作成 {#conditional-options}

条件付きファイル書式設定オプションを作成できます。このオプションは、ユーザーが書き出し用に特定のファイルタイプを選択した場合にのみアクティベーションワークフローに表示されます。 例えば、以下の設定では、CSV ファイルオプションの条件付きグループが作成されます。 CSV ファイルオプションは、書き出し対象のファイルタイプとして CSV を選択した場合にのみ表示されます。

フィールドを条件付きとして設定するには、 `conditional` パラメーターの値を次に示します。

```json
            "conditional": {
                "field": "fileType",
                "operator": "EQUALS",
                "value": "CSV"
            }
```

より広いコンテキストでは、 `conditional` 以下の宛先設定で、 `fileType` 文字列と `csvOptions` オブジェクトを定義します。

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

以下に、上記の設定に基づいて、結果の UI 画面を示します。 ユーザーがファイルタイプ CSV を選択すると、CSV ファイルタイプを参照する追加のファイル形式設定オプションが UI に表示されます。

![CSV ファイルの条件付きファイル形式オプションを示す画面記録。](/help/destinations/destination-sdk/assets/guides/batch/conditional-file-formatting.gif)

### 上記のすべてのオプションを含む完全な API リクエスト

以下の API リクエストは、上記の節で説明したすべてのオプションを 1 つの設定に組み合わせたものです。

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
  "releaseNotes": "Test destination",
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

正常な応答は、一意の識別子 (`instanceId`) が含まれています。

## 既知の制限事項 {#known-limitations}

ファイルフォーマットオプションを特定の組み合わせにすると、ファイルの書き出し結果が望ましくなくなる場合があります。
Adobeでは、次の CSV オプションの組み合わせを選択しないことをお勧めします。

```
nullValue -> ""
quote -> "
emptyValue -> ""
```

制限の例を示すために、次の値を持つファイルのエクスポートを検討します。

| 名 | 姓 | 国 | state |
|---------|----------|---------|--------|
| マイケル | ローズ | USA | NY |
| James | Smith |  | null |

{style=&quot;table-layout:auto&quot;}

これにより、次のような出力が生成されます。 テーブルからの null 値が、誤ってエスケープされた引用符として書き出されることに注意してください。

```csv
Michael,Rose,USA,NY 
James,Smith,"","\"\""
```

## 次の手順 {#next-steps}

この記事では、書き出したファイルのカスタムファイル形式設定オプションを、Destination SDKを使用して設定する方法を説明します。 次に、チームが [ファイルベースの宛先のアクティベーションワークフロー](../../../ui/activate-batch-profile-destinations.md) をクリックして、宛先にデータを書き出します。
