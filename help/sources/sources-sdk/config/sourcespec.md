---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: Sources SDK の認証仕様の設定
topic-legacy: overview
description: このドキュメントでは、Sources SDK を使用するために準備が必要な設定の概要を説明します。
hide: true
hidefromtoc: true
exl-id: f814c883-b529-4ecc-bedd-f638bf0014b5
source-git-commit: 4c4c89ab7db7d3546163d707ac80210561c2fa02
workflow-type: ht
source-wordcount: '861'
ht-degree: 100%

---

# Sources SDK のソース仕様の設定

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
| `sourceSpec.attributes.spec.properties.contentPath` | Platform に取り込む必要がある項目のリストを含むノードを定義します。 この属性は、有効な JSON パス構文に従い、特定の配列を指す必要があります。 | コンテンツパス内に含まれるリソースの例については、[付録](#content-path)を参照してください。 |
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
| `sourceSpec.attributes.spec.properties.paginationParams.type` | ソースでサポートされているページネーションタイプを表示します。 | <ul><li>`offset`：このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の上限数を指定することで、結果を解析できます。</li><li>`pointer`：このページネーションタイプを使用すると、リクエストと共に送信する必要がある特定の項目を、`pointer` 変数を使用して指定することができます。ポインタータイプのページネーションでは、次のページを指すパスがペイロード内に必要です</li></ul> |
| `sourceSpec.attributes.spec.properties.paginationParams.limitName` | API が 1 ページで取得するレコードの数を指定できる制限の名前。 | `limit` または `count` |
| `sourceSpec.attributes.spec.properties.paginationParams.limitValue` | 1 ページで取得するレコードの数。 | `limit=10` または `count=10` |
| `sourceSpec.attributes.spec.properties.paginationParams.offSetName` | オフセット属性名。 ページネーションタイプが `offset` に設定されている場合に必要です。 | `offset` |
| `sourceSpec.attributes.spec.properties.paginationParams.pointerPath` | ポインターの属性名。 これには、次のページを指す属性への JSON パスが必要です。 ページネーションタイプが `pointer` に設定されている場合に必要です。 | `pointer` |
| `sourceSpec.attributes.spec.properties.scheduleParams` | ソースでサポートされるスケジューリング形式を定義するパラメーターが含まれます。スケジュールのパラメーターには `startTime` および `endTime` が含まれます。いずれも使用することで、バッチ実行に特定の時間間隔を設定でき、これにより前のバッチ実行で取得されたレコードが再び取得されることがなくなります。 |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamName` | 開始時間のパラメーター名を定義します | `since_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamName` | 終了時間のパラメーター名を定義します | `before_last_changed` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleStartParamFormat` | `scheduleStartParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.attributes.spec.properties.scheduleParams.scheduleEndParamFormat` | `scheduleEndParamName` でサポートされる形式を定義します。 | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.spec.properties` | リソース値を取得するためのユーザー指定のパラメーターを定義します。 | `spec.properties` でユーザーが入力したパラメーターの例については、[付録](#user-input)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## 付録 {#appendix}

### コンテンツパスの例 {#content-path}

[!DNL MailChimp Campaigns] 接続の仕様における `contentPath` プロパティのコンテンツの例を次に示します。

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

## 次の手順

ソースの仕様を入力したので、次は Platform に統合するソースの探索仕様を設定できます。詳しくは、[探索仕様の設定](./explorespec.md)に関するドキュメントを参照してください。
