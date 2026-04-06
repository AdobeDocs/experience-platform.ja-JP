---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: セルフサービスソースのソース仕様の設定（バッチSDK）
description: このドキュメントでは、セルフサービスソース（バッチSDK）を使用するために準備する必要がある設定の概要を説明します。
exl-id: f814c883-b529-4ecc-bedd-f638bf0014b5
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '2107'
ht-degree: 38%

---

# セルフサービスソースのソース仕様の設定（バッチSDK）

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
| `sourceSpec.attributes` | UI または API に固有のソースに関する情報が含まれます。 |  |
| `sourceSpec.attributes.uiAttributes` | UI に固有のソースに関する情報を表示します。 |  |
| `sourceSpec.attributes.uiAttributes.isPreview` | ソースがプレビューとして表示されるかどうかを示すブール値の属性（実稼動用/一般利用可能用ではありません）。 | <ul><li>`true`</li><li>`false`</li></ul> |
| `sourceSpec.attributes.uiAttributes.isBeta` | 機能に追加するために、顧客からのより多くのフィードバックがソースで必要かどうかを示すブール値の属性です。 | <ul><li>`true`</li><li>`false`</li></ul> |
| `sourceSpec.attributes.uiAttributes.category` | ソースのカテゴリを定義します。 | <ul><li>`advertising`</li><li>`crm`</li><li>`customer success`</li><li>`database`</li><li>`ecommerce`</li><li>`marketing automation`</li><li>`payments`</li><li>`protocols`</li></ul> |
| `sourceSpec.attributes.uiAttributes.icon` | Experience Platform UIでのソースのレンダリングに使用するアイコンを定義します。 | `mailchimp-icon.svg` |
| `sourceSpec.attributes.uiAttributes.description` | ソースの簡単な説明を表示します。 |  |
| `sourceSpec.attributes.uiAttributes.label` | Experience Platform UIでソースのレンダリングに使用するラベルを表示します。 |  |
| `sourceSpec.attributes.spec.properties.urlParams` | URL リソースのパス、メソッド、およびサポートされているクエリパラメーターに関する情報が含まれます。 |  |
| `sourceSpec.attributes.spec.properties.urlParams.properties.path` | データの取得元となるリソースパスを定義します。 | `/3.0/reports/${campaignId}/email-activity` |
| `sourceSpec.attributes.spec.properties.urlParams.properties.method` | データを取得するリクエストをリソースに送信する際に使用する HTTP メソッドを定義します。 | `GET`, `POST` |
| `sourceSpec.attributes.spec.properties.urlParams.properties.queryParams` | データの取得をリクエストする際にソース URL に追加できる、サポートされているクエリパラメーターを定義します。**メモ**：ユーザーが指定するパラメーター値は、プレースホルダーの形式にする必要があります。例：`${USER_PARAMETER}`。 | `"queryParams" : {"key" : "value", "key1" : "value1"}` はソース URL に `/?key=value&key1=value1` として追加されます。 |
| `sourceSpec.attributes.spec.properties.spec.properties.headerParams` | データの取得中にソース URL に対する HTTP リクエストで指定する必要があるヘッダーを定義します。 | `"headerParams" : {"Content-Type" : "application/json", "x-api-key" : "key"}` |
| `sourceSpec.attributes.spec.properties.bodyParams` | この属性は、POST リクエストを通じてHTTP bodyを送信するように設定できます。 |  |
| `sourceSpec.attributes.spec.properties.contentPath` | Experience Platformに取り込む必要のある項目のリストを含むノードを定義します。 この属性は、有効な JSON パス構文に従い、特定の配列を指す必要があります。 | コンテンツパス内に含まれるリソースの例については、[追加リソースセクション &#x200B;](#content-path)を参照してください。 |
| `sourceSpec.attributes.spec.properties.contentPath.path` | Experience Platformに取り込むコレクションレコードを指すパス。 | `$.emails` |
| `sourceSpec.attributes.spec.properties.contentPath.skipAttributes` | このプロパティを使用すると、コンテンツパスで識別されるリソースから、取り込みから除外する特定の項目を特定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.contentPath.keepAttributes` | このプロパティを使用すると、保持する個々の属性を明示的に指定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.contentPath.overrideWrapperAttribute` | このプロパティを使用すると、`contentPath` で指定した属性名の値を上書きできます。 | `email` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath` | このプロパティを使用すると、2つの配列を統合し、リソースデータをExperience Platform リソースに変換できます。 |  |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.path` | 統合するコレクションレコードを指すパス。 | `$.email.activity` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.skipAttributes` | このプロパティを使用すると、エンティティパスで識別されるリソースから、取り込みから除外する特定の項目を特定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.keepAttributes` | このプロパティを使用すると、保持する個々の属性を明示的に指定できます。 | `[total_items]` |
| `sourceSpec.attributes.spec.properties.explodeEntityPath.overrideWrapperAttribute` | このプロパティを使用すると、`explodeEntityPath` で指定した属性名の値を上書きできます。 | `activity` |
| `sourceSpec.attributes.spec.properties.paginationParams` | ユーザーの現在のページの応答から次のページへのリンクを取得するため、または次のページ URL を作成する際に指定する必要があるパラメーターまたはフィールドを定義します。 |  |
| `sourceSpec.attributes.spec.properties.paginationParams.type` | ソースでサポートされているページネーションタイプを表示します。 | <ul><li>`OFFSET`：このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の上限数を指定することで、結果を解析できます。</li><li>`POINTER`：このページネーションタイプを使用すると、リクエストと共に送信する必要がある特定の項目を、`pointer` 変数を使用して指定することができます。ポインタの種類のページ設定には、次のページを指すペイロードのパスが必要です。</li><li>`CONTINUATION_TOKEN`：このページ分割タイプを使用すると、クエリまたはヘッダーパラメーターを継続トークンで追加して、ソースから残りのリターンデータを取得できます。このリターンデータは、事前に決定された最大値のために最初に返されませんでした。</li><li>`PAGE`：このページ分割タイプを使用すると、ページごとに戻り値データをトラバースするページングパラメーターを含むクエリパラメーターをページの0から追加できます。</li><li>`NONE`：このページ分割タイプは、使用可能なページ分割タイプのいずれもサポートしていないソースに使用できます。 Pagination type `NONE`は、リクエストの後に応答データ全体を返します。</li></ul> |
| `sourceSpec.attributes.spec.properties.paginationParams.limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 | `limit` または `count` |
| `sourceSpec.attributes.spec.properties.paginationParams.limitValue` | 1 ページで取得するレコードの数。 | `limit=10` または `count=10` |
| `sourceSpec.attributes.spec.properties.paginationParams.offSetName` | オフセット属性名。 ページネーションタイプが `offset` に設定されている場合に必要です。 | `offset` |
| `sourceSpec.attributes.spec.properties.paginationParams.pointerPath` | ポインターの属性名。 これには、次のページを指す属性への JSON パスが必要です。 ページネーションタイプが `pointer` に設定されている場合に必要です。 | `pointer` |
| `sourceSpec.attributes.spec.properties.scheduleParams` | ソースでサポートされるスケジューリング形式を定義するパラメーターが含まれます。スケジュールのパラメーターには `startTime` および `endTime` が含まれます。いずれも使用することで、バッチ実行に特定の時間間隔を設定でき、これにより前のバッチ実行で取得されたレコードが再び取得されることがなくなります。 |  |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamName` | 開始時間のパラメーター名を定義します | `since_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamName` | 終了時間のパラメーター名を定義します | `before_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamFormat` | `scheduleStartParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamFormat` | `scheduleEndParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.spec.properties` | リソース値を取得するためのユーザー指定のパラメーターを定義します。 | [のユーザー入力パラメーターの例については、](#user-input)追加リソース `spec.properties`を参照してください。 |

{style="table-layout:auto"}

## その他のリソース {#appendix}

次の節では、高度なスケジュール設定やカスタムスキーマなど、`sourceSpec`に対して実行できる追加の設定について説明します。

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

### ソースに対して異なるページネーションの種類を設定します {#pagination}

次に、セルフサービスソース（バッチSDK）でサポートされているその他のページネーションタイプの例を示します。

>[!BEGINTABS]

>[!TAB  オフセット ]

このページネーションタイプでは、結果の配列の開始場所からインデックスを指定し、結果の返し数に制限を設けることで、結果を解析できます。 例：

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
| `endConditionName` | 次のHTTP リクエストでページネーション ループを終了する条件を示すユーザー定義の値。 終了条件を配置する属性名を指定する必要があります。 |
| `endConditionValue` | 終了条件を配置する属性値。 |

>[!TAB  ポインター]

このページ分割タイプを使用すると、`pointer`変数を使用して、リクエストで送信する必要がある特定の項目を指定できます。 ポインタの種類のページ設定には、次のページを指すペイロードのパスが必要です。 例：

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
| `pointerPath` | ポインターの属性名。 これには、次のページを指す属性へのjson パスが必要です。 |

>[!TAB 継続トークン ]

ページネーションの継続トークンタイプは、1回の応答で返すことができる項目の最大数があらかじめ決められているため、返すことができなかった項目の存在を示す文字列トークンを返します。

ページ分割の継続トークンタイプをサポートするソースには、次のようなページ分割パラメーターがあります。

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
| `continuationTokenPath` | 返される結果の次のページに移動するために、クエリパラメーターに追加する必要がある値。 |
| `parameterType` | `parameterType` プロパティは、`parameterName`を追加する必要がある場所を定義します。 `QUERYPARAM` タイプを使用すると、クエリを`parameterName`に追加できます。 `HEADERPARAM`を使用すると、ヘッダーリクエストに`parameterName`を追加できます。 |
| `parameterName` | 継続トークンの組み込みに使用されるパラメーターの名前。 形式は次のとおりです：`{PARAMETER_NAME}={CONTINUATION_TOKEN}`。 |
| `delayRequestMillis` | ページネーションの`delayRequestMillis` プロパティを使用すると、ソースに対して行われたリクエストの割合を制御できます。 一部のソースでは、1分に実行できるリクエストの数に制限があります。 例えば、[!DNL Zendesk]のリクエスト数は1分あたり100件に制限されており、`delayRequestMillis`から`850`を定義すると、1分あたり100件のリクエスト数のしきい値を大幅に下回る、1分あたり約80件のリクエストで呼び出しを行うようにソースを設定できます。 |

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

>[!TAB  ページ ]

`PAGE` タイプのページネーションでは、0から始まるページ数で返されるデータをトラバースできます。 `PAGE` タイプのページネーションを使用する場合は、1つのページで指定されたレコード数を指定する必要があります。

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
| `initialPageIndex` | （オプション）最初のページインデックスは、ページネーションを開始するページ番号を定義します。 このフィールドは、ページネーションが0から始まらないソースに使用できます。 指定しない場合、最初のページインデックスはデフォルトで0になります。 このフィールドには整数が必要です。 |
| `endPageIndex` | （オプション）終了ページインデックスを使用すると、終了条件を設定し、ページネーションを停止できます。 このフィールドは、ページ分割を停止するデフォルトの終了条件が使用できない場合に使用できます。 このフィールドは、取り込むページ数または最後のページ番号が応答ヘッダーを通じて提供される場合にも使用できます。これは、`PAGE`形式のページネーションを使用する場合に一般的です。 終了ページインデックスの値は、最後のページ番号または応答ヘッダーの文字列型式の値のいずれかになります。 例えば、`headers.x-pagecount`を使用して、応答ヘッダーから`x-pagecount`値に終了ページインデックスを割り当てることができます。 **注**: `x-pagecount`は、一部のソースに対する必須の応答ヘッダーであり、取り込むページの数の値を保持します。 |
| `pageParamName` | 戻り値データの異なるページをトラバースするために、クエリパラメーターに追加する必要があるパラメーターの名前。 例えば、`https://abc.com?pageIndex=1`は、APIのリターンペイロードの2番目のページを返します。 |
| `maximumRequest` | 特定の増分実行に対してソースが実行できるリクエストの最大数。 現在のデフォルト制限は10000です。 |

{style="table-layout:auto"}


>[!TAB なし]

`NONE` ページネーション タイプは、使用可能なページネーション タイプのいずれもサポートしていないソースに使用できます。 `NONE`のページネーション型を使用するソースは、GET リクエストが行われたときに、取得できるすべてのレコードを返すだけです。

```json
"paginationParams": {
  "type": "NONE"
}
```

>[!ENDTABS]

### セルフサービスソースの高度なスケジューリング（バッチSDK）

高度なスケジューリングを使用して、ソースの増分スケジュールとバックフィルのスケジュールを設定します。 `incremental` プロパティでは、ソースが新しいレコードまたは変更されたレコードのみを取り込むスケジュールを設定できます。一方、`backfill` プロパティでは、履歴データを取り込むスケジュールを作成できます。

高度なスケジュール機能を使用すると、ソースに固有の式や関数を使用して、増分スケジュールやバックフィル スケジュールを設定できます。 次の例では、[!DNL Zendesk] ソースでは、増分スケジュールを`type:user updated > {START_TIME} updated < {END_TIME}`形式で、バックフィルを`type:user updated < {END_TIME}`形式で設定する必要があります。

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
| `scheduleParams.type` | ソースで使用するスケジュールのタイプ。 高度なスケジュール タイプを使用するには、この値を`ADVANCE`に設定します。 |
| `scheduleParams.paramFormat` | スケジュール パラメータの定義済み形式。 この値は、ソースの`scheduleStartParamFormat`および`scheduleEndParamFormat`の値と同じにすることができます。 |
| `scheduleParams.incremental` | ソースの増分クエリ。 増分とは、新しいデータまたは変更されたデータのみが取り込まれる取り込み方法を指します。 |
| `scheduleParams.backfill` | ソースのバックフィルクエリ。 バックフィルとは、履歴データが取り込まれる取り込み方法を指します。 |

高度なスケジュールを設定したら、特定のソースがサポートしているものに応じて、URL、本文、またはヘッダーのパラメーターのセクションで`scheduleParams`を参照する必要があります。 次の例では、`{SCHEDULE_QUERY}`は、増分スケジュール式とバックフィル スケジュール式を使用する場所を指定するために使用されるプレースホルダーです。 [!DNL Zendesk] ソースの場合、`query`は`queryParams`で高度なスケジュール設定を指定するために使用されます。

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

### カスタムスキーマを追加して、ソースの動的属性を定義します

カスタムスキーマを`sourceSpec`に含めることで、必要な動的属性を含め、ソースに必要なすべての属性を定義できます。 接続仕様の`/connectionSpecs` セクションでカスタムスキーマを指定しながら、[!DNL Flow Service] APIの`sourceSpec` エンドポイントに対してPUT リクエストを行うことで、ソースの対応する接続仕様を更新できます。

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

ソースの仕様を入力したら、Experience Platformに統合するソースのエクスプローラーの仕様を設定します。 詳しくは、[探索仕様の設定](./explorespec.md)に関するドキュメントを参照してください。
