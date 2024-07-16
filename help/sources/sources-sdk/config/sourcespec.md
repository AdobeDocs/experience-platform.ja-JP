---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: セルフサービスソースのソース仕様の設定（Batch SDK）
description: このドキュメントでは、セルフサービスソース（Batch SDK）を使用するために準備が必要な設定の概要を説明します。
exl-id: f814c883-b529-4ecc-bedd-f638bf0014b5
source-git-commit: 1fdce7c798d8aff49ab4953298ad7aa8dddb16bd
workflow-type: tm+mt
source-wordcount: '2084'
ht-degree: 43%

---

# セルフサービスソースのソース仕様の設定（Batch SDK）

ソース仕様には、ソースのカテゴリ、ベータステータス、カタログアイコンに関する属性など、ソースに固有の情報が含まれます。また、URL パラメーター、コンテンツ、ヘッダー、スケジュールなどの便利な情報も含まれています。ソース仕様は、ベース接続からのソース接続を作成するために必要なパラメーターのスキーマも記述します。ソース接続を作成するには、スキーマが必要です。

完全に入力されたソース仕様の例については、[付録](#source-spec)を参照してください。


```json
"sourceSpec": {
  "attributes": {
    "uiAttributes": {
      "isSource": true,
      "isPreview": true,
      "isBeta": true,
      "category": {
        "key": "protocols"
      },
      "icon": {
        "key": "genericRestIcon"
      },
      "description": {
        "key": "genericRestDescription"
      },
      "label": {
        "key": "genericRestLabel"
      }
    },
    "spec": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "description": "Defines static and user input parameters to fetch resource values.",
      "properties": {
        "urlParams": {
          "type": "object",
          "properties": {
            "host": {
              "type": "string",
              "description": "Enter resource url host path.",
              "example": "https://{domain}.api.mailchimp.com"
            },
            "path": {
              "type": "string",
              "description": "Enter resource path",
              "example": "/3.0/reports/campaignId/email-activity"
            },
            "method": {
              "type": "string",
              "description": "HTTP method type.",
              "enum": [
                "GET",
                "POST"
              ]
            },
            "queryParams": {
              "type": "object",
              "description": "The query parameters in json format",
            }
          },
          "required": [
            "host",
            "path",
            "method"
          ]
        },
        "headerParams": {
          "type": "object",
          "description": "The header parameters in json format",
        },
        "contentPath": {
          "type": "object",
          "description": "The parameters required for main collection content.",
          "properties": {
            "path": {
              "description": "The path to the main content.",
              "type": "string",
              "example": "$.emails"
            },
            "skipAttributes": {
              "type": "array",
              "description": "The list of attributes that needs to be skipped while fattening the array.",
              "example": "[total_items]",
              "items": {
                "type": "string"
              }
            },
            "keepAttributes": {
              "type": "array",
              "description": "The list of attributes that needs to be kept while fattening the array.",
              "example": "[total_items]",
              "items": {
                "type": "string"
              }
            },
            "overrideWrapperAttribute": {
              "type": "string",
              "description": "The new name to be used for the root content path node.",
              "example": "email"
            }
          },
          "required": [
            "path"
          ]
        },
        "explodeEntityPath": {
          "type": "object",
          "description": "The parameters required for the sub-array content.",
          "properties": {
            "path": {
              "description": "The path to the sub-array content.",
              "type": "string",
              "example": "$.email.activity"
            },
            "skipAttributes": {
              "type": "array",
              "description": "The list of attributes that needs to be skipped while fattening sub-array.",
              "example": "[total_items]",
              "items": {
                "type": "string"
              }
            },
            "keepAttributes": {
              "type": "array",
              "description": "The list of attributes that needs to be kept while fattening the sub-array.",
              "example": "[total_items]",
              "items": {
                "type": "string"
              }
            },
            "overrideWrapperAttribute": {
              "type": "string",
              "description": "The new name to be used for the  root content path node.",
              "example": "activity"
            }
          },
          "required": [
            "path"
          ]
        },
        "paginationParams": {
          "type": "object",
          "description": "The parameters required to fetch data using pagination.",
          "properties": {
            "type": {
              "description": "The pagination fetch type.",
              "type": "string",
              "enum": [
                "OFFSET",
                "POINTER"
              ]
            },
            "limitName": {
              "type": "string",
              "description": "The limit property name",
              "example": "limit or count"
            },
            "limitValue": {
              "type": "integer",
              "description": "The number of records to fetch per page.",
              "example": "limit=10 or count=10"
            },
            "offsetName": {
              "type": "string",
              "description": "The offset property name",
              "example": "offset"
            },
            "pointerPath": {
              "type": "string",
              "description": "The path to pointer property",
              "example": "$.paging.next"
            }
          },
          "required": [
            "type",
            "limitName",
            "limitValue"
          ]
        },
        "scheduleParams": {
          "type": "object",
          "description": "The parameters required to fetch data for batch schedule.",
          "properties": {
            "scheduleStartParamName": {
              "type": "string",
              "description": "The order property name to get the order by date."
            },
            "scheduleEndParamName": {
              "type": "string",
              "description": "The order property name to get the order by date."
            },
            "scheduleStartParamFormat": {
              "type": "string",
              "description": "The order property name to get the order by date.",
              "example": "yyyy-MM-ddTHH:mm:ssZ"
            },
            "scheduleEndParamFormat": {
              "type": "string",
              "description": "The order property name to get the order by date.",
              "example": "yyyy-MM-ddTHH:mm:ssZ"
            }
          },
          "required": [
            "scheduleStartParamName",
            "scheduleEndParamName"
          ]
        }
      },
      "required": [
        "urlParams",
        "contentPath",
        "paginationParams",
        "scheduleParams"
      ]
    }
  },
}
```

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `sourceSpec.attributes` | UI または API に固有のソースに関する情報が含まれます。 |
| `sourceSpec.attributes.uiAttributes` | UI に固有のソースに関する情報を表示します。 |
| `sourceSpec.attributes.uiAttributes.isBeta` | 機能に追加するために、顧客からのより多くのフィードバックがソースで必要かどうかを示すブール値の属性です。 | <ul><li>`true`</li><li>`false`</li></ul> |
| `sourceSpec.attributes.uiAttributes.category` | ソースのカテゴリを定義します。 | <ul><li>`advertising`</li><li>`crm`</li><li>`customer success`</li><li>`database`</li><li>`ecommerce`</li><li>`marketing automation`</li><li>`payments`</li><li>`protocols`</li></ul> |
| `sourceSpec.attributes.uiAttributes.icon` | Platform の UI でソースのレンダリングに使用するアイコンを定義します。 | `mailchimp-icon.svg` |
| `sourceSpec.attributes.uiAttributes.description` | ソースの簡単な説明を表示します。 |
| `sourceSpec.attributes.uiAttributes.label` | Platform の UI でソースのレンダリングに使用するラベルを表示します。 |
| `sourceSpec.attributes.spec.properties.urlParams` | URL リソースのパス、メソッド、およびサポートされているクエリパラメーターに関する情報が含まれます。 |
| `sourceSpec.attributes.spec.properties.urlParams.properties.path` | データの取得元となるリソースパスを定義します。 | `/3.0/reports/${campaignId}/email-activity` |
| `sourceSpec.attributes.spec.properties.urlParams.properties.method` | データを取得するリクエストをリソースに送信する際に使用する HTTP メソッドを定義します。 | `GET`、`POST` |
| `sourceSpec.attributes.spec.properties.urlParams.properties.queryParams` | データの取得をリクエストする際にソース URL に追加できる、サポートされているクエリパラメーターを定義します。**メモ**：ユーザーが指定するパラメーター値は、プレースホルダーの形式にする必要があります。例：`${USER_PARAMETER}`。 | `"queryParams" : {"key" : "value", "key1" : "value1"}` はソース URL に `/?key=value&key1=value1` として追加されます。 |
| `sourceSpec.attributes.spec.properties.spec.properties.headerParams` | データの取得中にソース URL に対する HTTP リクエストで指定する必要があるヘッダーを定義します。 | `"headerParams" : {"Content-Type" : "application/json", "x-api-key" : "key"}` |
| `sourceSpec.attributes.spec.properties.bodyParams` | この属性は、POSTリクエストを通じて HTTP 本文を送信するように設定できます。 |
| `sourceSpec.attributes.spec.properties.contentPath` | Platform に取り込む必要がある項目のリストを含むノードを定義します。 この属性は、有効な JSON パス構文に従い、特定の配列を指す必要があります。 | コンテンツパス内に含まれるリソースの例については、[ 追加のリソース ](#content-path) の節を参照してください。 |
| `sourceSpec.attributes.spec.properties.contentPath.path` | Platform に取り込まれるコレクションレコードを指すパス。 | `$.emails` |
| `sourceSpec.attributes.spec.properties.contentPath.skipAttributes` | このプロパティを使用すると、コンテンツパスで識別されるリソースから、取り込みから除外する特定の項目を特定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.contentPath.keepAttributes` | このプロパティを使用すると、保持する個々の属性を明示的に指定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.contentPath.overrideWrapperAttribute` | このプロパティを使用すると、`contentPath` で指定した属性名の値を上書きできます。 | `email` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath` | このプロパティを使用すると、2 つの配列を統合し、リソースデータを Platform リソースに変換できます。 |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.path` | 統合するコレクションレコードを指すパス。 | `$.email.activity` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.skipAttributes` | このプロパティを使用すると、エンティティパスで識別されるリソースから、取り込みから除外する特定の項目を特定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.keepAttributes` | このプロパティを使用すると、保持する個々の属性を明示的に指定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.overrideWrapperAttribute` | このプロパティを使用すると、`explodeEntityPath` で指定した属性名の値を上書きできます。 | `activity` |
| `sourceSpec.attributes.spec.properties.paginationParams` | ユーザーの現在のページの応答から次のページへのリンクを取得するため、または次のページ URL を作成する際に指定する必要があるパラメーターまたはフィールドを定義します。 |
| `sourceSpec.attributes.spec.properties.paginationParams.type` | ソースでサポートされているページネーションタイプを表示します。 | <ul><li>`OFFSET`：このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の上限数を指定することで、結果を解析できます。</li><li>`POINTER`：このページネーションタイプを使用すると、リクエストと共に送信する必要がある特定の項目を、`pointer` 変数を使用して指定することができます。ポインタータイプのページネーションでは、次のページを指すパスがペイロード内に必要です。</li><li>`CONTINUATION_TOKEN`：このページネーションタイプを使用すると、クエリまたはヘッダーパラメーターに継続トークンを追加して、事前に決定された最大値によって最初に返されなかった残りのリターンデータをソースから取得できます。</li><li>`PAGE`：このページネーションタイプを使用すると、クエリパラメーターにページングパラメーターを追加して、page 0 から始まり、ページごとに返されるデータをトラバースできます。</li><li>`NONE`：このページネーションタイプは、使用可能なページネーションタイプのいずれもサポートしていないソースに使用できます。 ページネーションタイプ `NONE` は、リクエスト後の応答データ全体を返します。</li></ul> |
| `sourceSpec.attributes.spec.properties.paginationParams.limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 | `limit` または `count` |
| `sourceSpec.attributes.spec.properties.paginationParams.limitValue` | 1 ページで取得するレコードの数。 | `limit=10` または `count=10` |
| `sourceSpec.attributes.spec.properties.paginationParams.offSetName` | オフセット属性名。 ページネーションタイプが `offset` に設定されている場合に必要です。 | `offset` |
| `sourceSpec.attributes.spec.properties.paginationParams.pointerPath` | ポインターの属性名。 これには、次のページを指す属性への JSON パスが必要です。 ページネーションタイプが `pointer` に設定されている場合に必要です。 | `pointer` |
| `sourceSpec.attributes.spec.properties.scheduleParams` | ソースでサポートされるスケジューリング形式を定義するパラメーターが含まれます。スケジュールのパラメーターには `startTime` および `endTime` が含まれます。いずれも使用することで、バッチ実行に特定の時間間隔を設定でき、これにより前のバッチ実行で取得されたレコードが再び取得されることがなくなります。 |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamName` | 開始時間のパラメーター名を定義します | `since_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamName` | 終了時間のパラメーター名を定義します | `before_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamFormat` | `scheduleStartParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamFormat` | `scheduleEndParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.spec.properties` | リソース値を取得するためのユーザー指定のパラメーターを定義します。 | `spec.properties` ーザーが入力したパラメーターの例については、[ 追加のリソース ](#user-input) を参照してください。 |

{style="table-layout:auto"}

## その他のリソース {#appendix}

次の節では、高度なスケジュール設定やカスタムスキーマなど、`sourceSpec` ークフローに対して実行できる追加設定について説明します。

### コンテンツパスの例 {#content-path}

[!DNL MailChimp Members] 接続の仕様における `contentPath` プロパティのコンテンツの例を次に示します。

```json
"contentPath": {
  "path": "$.members",
  "skipAttributes": [
    "_links",
    "total_items",
    "list_id"
  ],
  "overrideWrapperAttribute": "member"
}
```

### `spec.properties` ユーザー入力の例 {#user-input}

[!DNL MailChimp Members] 接続の仕様を使用したユーザー指定 `spec.properties` の例を次に示します。

この例では、`listId` は `urlParams.path` の一部として指定されています。`listId` を顧客から取得する必要がある場合は、それを `spec.properties` の一部として定義する必要があります。


```json
"urlParams": {
        "path": "/3.0/lists/${listId}/members",
        "method": "GET"
      }
"spec": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "description": "Define user input parameters to fetch resource values.",
      "properties": {
        "listId": {
          "type": "string",
          "description": "listId for which members need to fetch."
        }
      }
    }
```

### ソースの仕様の例 {#source-spec}

[!DNL MailChimp Members] を使用した完全なソース仕様を次に示します。

```json
  "sourceSpec": {
    "attributes": {
      "uiAttributes": {
        "isSource": true,
        "isPreview": true,
        "isBeta": true,
        "category": {
          "key": "marketingAutomation"
        },
        "icon": {
          "key": "mailchimpMembersIcon"
        },
        "description": {
          "key": "mailchimpMembersDescription"
        },
        "label": {
          "key": "mailchimpMembersLabel"
        }
      },
      "urlParams": {
        "host": "https://{domain}.api.mailchimp.com",
        "path": "/3.0/lists/${listId}/members",
        "method": "GET"
      },
      "contentPath": {
        "path": "$.members",
        "skipAttributes": [
          "_links",
          "total_items",
          "list_id"
        ],
        "overrideWrapperAttribute": "member"
      },
      "paginationParams": {
        "type": "OFFSET",
        "limitName": "count",
        "limitValue": "100",
        "offSetName": "offset"
      },
      "scheduleParams": {
        "scheduleStartParamName": "since_last_changed",
        "scheduleEndParamName": "before_last_changed",
        "scheduleStartParamFormat": "yyyy-MM-ddTHH:mm:ss:fffffffK",
        "scheduleEndParamFormat": "yyyy-MM-ddTHH:mm:ss:fffffffK"
      }
    },
    "spec": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "description": "Define user input parameters to fetch resource values.",
      "properties": {
        "listId": {
          "type": "string",
          "description": "listId for which members need to fetch."
        }
      }
    }
  }
```

### ソースに対して様々なページネーションタイプを設定 {#pagination}

セルフサービスソース（Batch SDK）でサポートされる他のページネーションタイプの例を以下に示します。

>[!BEGINTABS]

>[!TAB  オフセット ]

このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の上限数を指定することで、結果を解析できます。 例：

```json
"paginationParams": {
        "type": "OFFSET",
        "limitName": "limit",
        "limitValue": "4",
        "offSetName": "offset",
        "endConditionName": "$.hasMore",
        "endConditionValue": "Const:false"
}
```

| プロパティ | 説明 |
| --- | --- |
| `type` | データを返すために使用されるページネーションのタイプ。 |
| `limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 |
| `limitValue` | 1 ページで取得するレコードの数。 |
| `offSetName` | オフセット属性名。 ページネーションタイプが `offset` に設定されている場合に必要です。 |
| `endConditionName` | 次の HTTP リクエストでページネーションループを終了させる条件を示すユーザー定義の値。 終了条件を設定する属性名を指定する必要があります。 |
| `endConditionValue` | 終了条件を設定する属性値。 |

>[!TAB  ポインター ]

このページネーションタイプを使用すると、リクエストと共に送信する必要がある特定の項目を、`pointer` 変数を使用して指定することができます。 ポインタータイプのページネーションでは、次のページを指すパスがペイロード内に必要です。 例：

```json
{
 "type": "POINTER",
 "limitName": "limit",
 "limitValue": 1,
 "pointerPath": "paging.next"
}
```

| プロパティ | 説明 |
| --- | --- |
| `type` | データを返すために使用されるページネーションのタイプ。 |
| `limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 |
| `limitValue` | 1 ページで取得するレコードの数。 |
| `pointerPath` | ポインターの属性名。 これには、次のページを指す属性への JSON パスが必要です。 |

>[!TAB  継続トークン ]

ページネーションの継続トークンタイプは、1 回の応答で返される可能性のある項目の最大数があらかじめ決定されていることから、返すことができなかった項目の数がさらに存在することを示す文字列トークンを返します。

継続トークンタイプのページネーションをサポートするソースは、次のようなページネーションパラメーターを持つ場合があります。

```json
"paginationParams": {
  "type": "CONTINUATION_TOKEN",
  "continuationTokenPath": "$.meta.after_cursor",
  "parameterType": "QUERYPARAM",
  "parameterName": "page[after]",
  "delayRequestMillis": "850"
}
```

| プロパティ | 説明 |
| --- | --- |
| `type` | データを返すために使用されるページネーションのタイプ。 |
| `continuationTokenPath` | 返された結果の次のページに移動するために、クエリパラメーターに追加する必要がある値。 |
| `parameterType` | `parameterType` プロパティは、`parameterName` を追加する場所を定義します。 `QUERYPARAM` タイプを使用すると、クエリに `parameterName` を追加できます。 `HEADERPARAM` を使用すると、ヘッダーリクエストに `parameterName` を追加できます。 |
| `parameterName` | 継続トークンの組み込みに使用されるパラメーターの名前。 形式は次のとおりです。`{PARAMETER_NAME}={CONTINUATION_TOKEN}` |
| `delayRequestMillis` | ページネーションの `delayRequestMillis` プロパティを使用すると、ソースに対して行われるリクエストの割合を制御できます。 一部のソースでは、1 分あたりに実行できるリクエスト数に制限があります。 例えば、[!DNL Zendesk] には 1 分あたりのリクエストが 100 件という制限があり、`delayRequestMillis` を `850` に定義すると、1 分あたりのリクエスト 100 件のしきい値の下で、1 分あたりのリクエスト数がちょうど約 80 件になるようにソースを設定できます。 |

次に、継続トークンタイプのページネーションを使用して返される応答の例を示します。

```json
{
  "results": [
    {
      "id": 5624716025745,
      "url": "https://dev.zendesk.com/api/v2/users/5624716025745.json",
      "name": "newinctest@zenaep.com",
      "email": "newinctest@zenaep.com",
      "created_at": "2022-04-22T10:27:30Z",
      
    }
  ],
  "facets": null,
  "meta": {
    "has_more": false,
    "after_cursor": "eyJmaWVsZCI6ImNyZWF0ZWRfYXQiLCJk",
    "before_cursor": null
  },
  "links": {
    "prev": null,
    "next": "https://dev.zendesk.com/api/v2/search/export.json?filter%5Btype%5D=user&page%5Bafter%5D=eyJmaWVsZCI6"
  }
}
```

>[!TAB  ページ ]

`PAGE` タイプのページネーションを使用すると、0 から始まるページ数でリターンデータをトラバースできます。 `PAGE` タイプのページネーションを使用する場合、1 つのページで指定するレコード数を指定する必要があります。

```json
"paginationParams": {
  "type": "PAGE",
  "limitName": "pageSize",
  "limitValue": 100,
  "initialPageIndex": 1,
  "endPageIndex": "headers.x-pagecount",
  "pageParamName": "pageNumber",
  "maximumRequest": 10000
}
```

| プロパティ | 説明 |
| --- | --- |
| `type` | データを返すために使用されるページネーションのタイプ。 |
| `limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 |
| `limitValue` | 1 ページで取得するレコードの数。 |
| `initialPageIndex` | （オプション）初期ページインデックスは、ページネーションを開始するページ番号を定義します。 このフィールドは、ページネーションが 0 から始まらないソースに使用できます。 指定しない場合、初期ページインデックスはデフォルトで 0 になります。 このフィールドには整数を指定してください。 |
| `endPageIndex` | （オプション）終了ページインデックスを使用すると、終了条件を確立し、ページネーションを停止できます。 このフィールドは、ページネーションを停止するデフォルトの終了条件が使用できない場合に使用できます。 このフィールドは、取り込まれるページ数または最後のページ番号が応答ヘッダーを通じて提供された場合にも使用できます。これは、`PAGE` タイプのページネーションを使用する場合に一般的です。 終了ページインデックスの値は、最後のページ番号または応答ヘッダーから得られる文字列型の式値です。 例えば、`headers.x-pagecount` を使用して、応答ヘッダーの `x-pagecount` 値に終了ページインデックスを割り当てることができます。 **注意**:`x-pagecount` は一部のソースにとって必須の応答ヘッダーであり、取り込まれるページの値を保持します。 |
| `pageParamName` | 返されるデータの様々なページをトラバースするために、クエリパラメーターに追加する必要があるパラメーターの名前。 例：`https://abc.com?pageIndex=1` は、API のリターンペイロードの 2 ページ目を返します。 |
| `maximumRequest` | 特定の増分実行に対してソースがおこなえるリクエストの最大数。 現在のデフォルトの制限は 10000 です。 |

{style="table-layout:auto"}


>[!TAB  なし ]

使用可能なページネーションタイプのいずれもサポートしていないソースには、`NONE` のページネーションタイプを使用できます。 `NONE` のページネーションタイプを使用するソースは、GETリクエストが行われた場合、取得可能なすべてのレコードを返します。

```json
"paginationParams": {
  "type": "NONE"
}
```

>[!ENDTABS]

### セルフサービスソースの高度なスケジュール設定（Batch SDK）

詳細スケジュールを使用して、ソースの増分スケジュールとバックフィルスケジュールを設定します。 `incremental` プロパティを使用すると、ソースが新規または変更済みのレコードのみを取り込むスケジュールを構成でき、`backfill` プロパティを使用すると、履歴データを取り込むスケジュールを作成できます。

高度なスケジュール設定では、ソースに固有の式や関数を使用して、増分スケジュールとバックフィルスケジュールを設定できます。 次の例では、増分スケジュールを `type:user updated > {START_TIME} updated < {END_TIME}` 形式に、バックフィルを `type:user updated < {END_TIME}` 形式にフォーマットして、[!DNL Zendesk] ソースに必要なデータを示します。

```json
"scheduleParams": {
        "type": "ADVANCE",
        "paramFormat": "yyyy-MM-ddTHH:mm:ssK",
        "incremental": "type:user updated > {START_TIME} updated < {END_TIME}",
        "backfill": "type:user updated < {END_TIME}"
      }
```

| プロパティ | 説明 |
| --- | --- |
| `scheduleParams.type` | ソースが使用するスケジュールのタイプ。 この値を `ADVANCE` に設定して、詳細スケジュール タイプを使用します。 |
| `scheduleParams.paramFormat` | スケジュールパラメーターの定義済み形式。 この値は、ソースの `scheduleStartParamFormat` 値および `scheduleEndParamFormat` 値と同じにすることができます。 |
| `scheduleParams.incremental` | ソースの増分処理クエリ。 増分とは、新しいデータまたは変更されたデータのみを取り込む取り込み方法を指します。 |
| `scheduleParams.backfill` | ソースのバックフィルクエリ。 バックフィルとは、履歴データが取り込まれる取り込み方法を指します。 |

高度なスケジュールを設定したら、特定のソースがサポートする内容に応じて、URL、本文、ヘッダーパラメーターのセクションで `scheduleParams` を参照する必要があります。 次の例では、増分スケジュール式 `{SCHEDULE_QUERY}` バックフィルスケジュール式が使用される場所を指定するためのプレースホルダーを使用しています。 [!DNL Zendesk] ソースの場合、`query` は `queryParams` で高度なスケジュール設定を指定するために使用されます。

```json
"urlParams": {
        "path": "/api/v2/search/export@{if(empty(coalesce(pipeline()?.parameters?.ingestionStart,'')),'?query=type:user&filter[type]=user&','')}",
        "method": "GET",
        "queryParams": {
          "query": "{SCHEDULE_QUERY}",
          "filter[type]": "user"
        }
      }
```

### カスタムスキーマを追加してソースの動的属性を定義する

ソースにカスタムスキーマを含めて、`sourceSpec` ースに必要なすべての属性（必要になる可能性のある動的属性を含む）を定義できます。 接続仕様の `sourceSpec` セクションでカスタムスキーマを指定すると同時に、[!DNL Flow Service] API の `/connectionSpecs` エンドポイントに対してPUTリクエストを行うことで、ソースの対応する接続仕様を更新できます。

ソースの接続仕様に追加できるカスタムスキーマの例を次に示します。

```json
      "schema": {
        "type": "object",
        "properties": {
          "results": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "organization_id": {
                  "type": "integer",
                  "minimum": -9007199254740992,
                  "maximum": 9007199254740991
                }
                "active": {
                  "type": "boolean"
                },
                "created_at": {
                  "type": "string"
                },
                "email": {
                  "type": "string"
                },
                "iana_time_zone": {
                  "type": "string"
                },
                "id": {
                  "type": "integer"
                },
                "locale": {
                  "type": "string"
                },
                "locale_id": {
                  "type": "integer"
                },
                "moderator": {
                  "type": "boolean"
                },
                "name": {
                  "type": "string"
                },
                "only_private_comments": {
                  "type": "boolean"
                },
                "report_csv": {
                  "type": "boolean"
                },
                "restricted_agent": {
                  "type": "boolean"
                },
                "result_type": {
                  "type": "string"
                },
                "role": {
                  "type": "integer"
                },
                "shared": {
                  "type": "boolean"
                },
                "shared_agent": {
                  "type": "boolean"
                },
                "suspended": {
                  "type": "boolean"
                },
                "ticket_restriction": {
                  "type": "string"
                },
                "time_zone": {
                  "type": "string"
                },
                "two_factor_auth_enabled": {
                  "type": "boolean"
                },
                "updated_at": {
                  "type": "string"
                },
                "url": {
                  "type": "string"
                },
                "verified": {
                  "type": "boolean"
                },
                "tags": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  }
                }
              }
            }
          }
        }
      }
```


## 次の手順

ソースの仕様を入力したので、次は Platform に統合するソースの探索仕様を設定できます。詳しくは、[ 探索仕様の設定 ](./explorespec.md) に関するドキュメントを参照してください。
