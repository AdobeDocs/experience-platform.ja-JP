---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: セルフサービスソース（バッチ SDK）のソース仕様の設定
description: このドキュメントでは、セルフサービスソース（バッチ SDK）を使用するために準備する必要がある設定の概要を説明します。
exl-id: f814c883-b529-4ecc-bedd-f638bf0014b5
source-git-commit: 1fdce7c798d8aff49ab4953298ad7aa8dddb16bd
workflow-type: tm+mt
source-wordcount: '2078'
ht-degree: 45%

---

# セルフサービスソース（バッチ SDK）のソース仕様の設定

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
| `sourceSpec.attributes.spec.properties.contentPath` | Platform に取り込む必要がある項目のリストを含むノードを定義します。 この属性は、有効な JSON パス構文に従い、特定の配列を指す必要があります。 | 次を表示： [その他のリソースセクション](#content-path) 例えば、コンテンツパス内に含まれるリソースの例です。 |
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
| `sourceSpec.attributes.spec.properties.paginationParams.type` | ソースでサポートされているページネーションタイプを表示します。 | <ul><li>`OFFSET`：このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の上限数を指定することで、結果を解析できます。</li><li>`POINTER`：このページネーションタイプを使用すると、リクエストと共に送信する必要がある特定の項目を、`pointer` 変数を使用して指定することができます。ポインタータイプのページネーションでは、次のページを指すパスがペイロード内に必要です。</li><li>`CONTINUATION_TOKEN`：このページネーションタイプを使用すると、クエリまたはヘッダーパラメーターに継続トークンを追加して、事前に決定された最大値により最初に返されなかった残りの戻りデータをソースから取得できます。</li><li>`PAGE`：このページネーションタイプを使用すると、0 ページから始まり、ページごとに戻りデータをトラバースするページングパラメーターと共にクエリパラメーターを追加できます。</li><li>`NONE`：このページネーションタイプは、使用可能なページネーションタイプをサポートしないソースに使用できます。 ページネーションタイプ `NONE` は、リクエストの後に応答データ全体を返します。</li></ul> |
| `sourceSpec.attributes.spec.properties.paginationParams.limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 | `limit` または `count` |
| `sourceSpec.attributes.spec.properties.paginationParams.limitValue` | 1 ページで取得するレコードの数。 | `limit=10` または `count=10` |
| `sourceSpec.attributes.spec.properties.paginationParams.offSetName` | オフセット属性名。 ページネーションタイプが `offset` に設定されている場合に必要です。 | `offset` |
| `sourceSpec.attributes.spec.properties.paginationParams.pointerPath` | ポインターの属性名。 これには、次のページを指す属性への JSON パスが必要です。 ページネーションタイプが `pointer` に設定されている場合に必要です。 | `pointer` |
| `sourceSpec.attributes.spec.properties.scheduleParams` | ソースでサポートされるスケジューリング形式を定義するパラメーターが含まれます。スケジュールのパラメーターには `startTime` および `endTime` が含まれます。いずれも使用することで、バッチ実行に特定の時間間隔を設定でき、これにより前のバッチ実行で取得されたレコードが再び取得されることがなくなります。 |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamName` | 開始時間のパラメーター名を定義します | `since_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamName` | 終了時間のパラメーター名を定義します | `before_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamFormat` | `scheduleStartParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamFormat` | `scheduleEndParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.spec.properties` | リソース値を取得するためのユーザー指定のパラメーターを定義します。 | 詳しくは、 [その他のリソース](#user-input) 例えば、 `spec.properties`. |

{style="table-layout:auto"}

## その他のリソース {#appendix}

以下の節では、 `sourceSpec`（高度なスケジュール設定やカスタムスキーマを含む）を含む ) を含みます。

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

### ソースに対する様々なページネーションタイプの設定 {#pagination}

セルフサービスソース（バッチ SDK）でサポートされるその他のページネーションタイプの例を次に示します。

>[!BEGINTABS]

>[!TAB オフセット]

このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の数の制限を指定することで、結果を解析できます。 以下に例を示します。

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
| `endConditionName` | 次の HTTP 要求でページネーションループを終了する条件を示すユーザー定義値。 終了条件を設定する属性名を指定する必要があります。 |
| `endConditionValue` | 終了条件を設定する属性値です。 |

>[!TAB ポインター]

このページネーションタイプでは、 `pointer` 変数を使用して、リクエストと共に送信する必要がある特定の項目を指定します。 ポインタータイプのページネーションでは、次のページを指すパスがペイロード内に必要です. 以下に例を示します。

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
| `pointerPath` | ポインターの属性名。 これには、次のページを指す属性への JSON パスが必要です。  |

>[!TAB 継続トークン]

ページネーションの継続トークンタイプは、1 回の応答で返すことができるアイテムの最大数が事前に決定されているので、返せないアイテムの数を増やしたことを示す文字列トークンを返します。

ページネーションの継続トークンタイプをサポートするソースには、次のようなページネーションパラメーターがあります。

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
| `continuationTokenPath` | 返された結果の次のページに移動するためにクエリパラメーターに追加する必要がある値。 |
| `parameterType` | The `parameterType` プロパティは、 `parameterName` を追加する必要があります。 The `QUERYPARAM` タイプを使用すると、クエリを `parameterName`. The `HEADERPARAM` を使用すると、 `parameterName` をヘッダーリクエストに追加します。 |
| `parameterName` | 継続トークンの組み込みに使用するパラメーターの名前。 形式は次のとおりです。 `{PARAMETER_NAME}={CONTINUATION_TOKEN}`. |
| `delayRequestMillis` | The `delayRequestMillis` プロパティをページネーションで使用すると、ソースに対するリクエストの割合を制御できます。 一部のソースでは、1 分あたりに実行できるリクエスト数に制限がある場合があります。 例： [!DNL Zendesk] には、1 分あたり 100 件のリクエストと定義の制限があります。  `delayRequestMillis` から `850` では、1 分あたり 100 件のリクエストしきい値の下で、1 分あたり約 80 件のリクエストで呼び出しをおこなうようにソースを設定できます。 |

次に、ページネーションの継続トークンタイプを使用して返される応答の例を示します。

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

>[!TAB ページ]

The `PAGE` ページネーションのタイプを使用すると、0 から始まるページ数で戻りデータをトラバースできます。 を使用する場合 `PAGE` 「ページネーション」と入力する場合、単一のページで指定するレコード数を指定する必要があります。

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
| `initialPageIndex` | （オプション）最初のページインデックスは、ページネーションを開始するページ番号を定義します。 このフィールドは、ページネーションが 0 から開始しないソースに使用できます。 指定しない場合、最初のページインデックスはデフォルトで 0 に設定されます。 このフィールドには整数が必要です。 |
| `endPageIndex` | （オプション）終了ページのインデックスを使用すると、終了条件を確立し、ページネーションを停止できます。 このフィールドは、ページネーションを停止するデフォルトの終了条件が使用できない場合に使用できます。 このフィールドは、取り込むページの数または最後のページ番号が応答ヘッダーを通じて指定される場合にも使用できます。このヘッダーは、 `PAGE` ページネーションを入力します。 終了ページのインデックスの値は、最後のページ番号か、応答ヘッダーからの文字列型式の値にすることができます。 例えば、 `headers.x-pagecount` 終了ページのインデックスを `x-pagecount` の値を返します。 **注意**: `x-pagecount` は、一部のソースに対する必須の応答ヘッダーで、取り込むページ数の値を保持しています。 |
| `pageParamName` | 戻りデータの様々なページをトラバースするためにクエリパラメーターに追加する必要があるパラメーターの名前。 例： `https://abc.com?pageIndex=1` は API の戻りペイロードの 2 番目のページを返します。 |
| `maximumRequest` | 特定の増分実行に対してソースが実行できるリクエストの最大数。 現在のデフォルトの制限は10000です。 |

{style="table-layout:auto"}


>[!TAB なし]

The `NONE` ページネーションタイプは、使用可能なページネーションタイプをサポートしないソースに使用できます。 ページネーションタイプを使用するソース `NONE` 単に、取得可能なすべてのレコードを、GETリクエストがおこなわれたときに返します。

```json
"paginationParams": {
  "type": "NONE"
}
```

>[!ENDTABS]

### セルフサービスソース（バッチ SDK）の高度なスケジュール設定

高度なスケジュールを使用して、ソースの増分スケジュールとバックフィルスケジュールを設定します。 The `incremental` プロパティを使用すると、新しいレコードや変更されたレコードのみを取り込むスケジュールを設定できます。 `backfill` プロパティを使用すると、履歴データを取り込むスケジュールを作成できます。

高度なスケジュール設定では、ソースに固有の式や関数を使用して、増分スケジュールとバックフィルスケジュールを設定できます。 次の例では、 [!DNL Zendesk] ソースでは、増分スケジュールの形式を設定する必要があります `type:user updated > {START_TIME} updated < {END_TIME}` およびバックフィル： `type:user updated < {END_TIME}`.

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
| `scheduleParams.type` | ソースで使用するスケジュールのタイプ。 この値をに設定します。 `ADVANCE` 詳細スケジュールタイプを使用するには、次の手順に従います。 |
| `scheduleParams.paramFormat` | スケジュールパラメーターの定義された形式。 この値は、ソースの `scheduleStartParamFormat` および `scheduleEndParamFormat` 値。 |
| `scheduleParams.incremental` | ソースの増分処理クエリ。 増分とは、新しいデータまたは変更されたデータのみを取り込む取り込み方法を指します。 |
| `scheduleParams.backfill` | ソースのバックフィルクエリ。 バックフィルとは、履歴データを取り込む取り込み方法を指します。 |

高度なスケジュールを設定したら、 `scheduleParams` 「 URL 」、「 body 」または「 header params 」セクションの値を指定します。 次の例では、 `{SCHEDULE_QUERY}` は、増分およびバックフィルのスケジューリング式を使用する場所を指定するために使用されるプレースホルダーです。 の場合、 [!DNL Zendesk] ソース、 `query` が `queryParams` をクリックして、アドバンススケジュールを指定します。

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

### カスタムスキーマを追加して、ソースの動的属性を定義する

カスタムスキーマを `sourceSpec` ：ソースに必要なすべての属性（必要な動的属性を含む）を定義します。 ソースの対応する接続仕様を更新するには、 `/connectionSpecs` エンドポイント [!DNL Flow Service] API ( `sourceSpec` 」のセクションを参照してください。

次に、ソースの接続仕様に追加できるカスタムスキーマの例を示します。

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

ソースの仕様を入力したので、次は Platform に統合するソースの探索仕様を設定できます。次のドキュメントを参照してください： [仕様の構成](./explorespec.md) を参照してください。
