---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
title: ソース SDK の認証仕様の設定
topic-legacy: overview
description: このドキュメントでは、ソース SDK を使用するために準備が必要な設定の概要を説明します。
hide: true
hidefromtoc: true
source-git-commit: d4b5b54be9fa2b430a3b45eded94a523b6bd4ef8
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 1%

---


# ソース SDK のソース指定を設定する

ソースの仕様には、ソースのカテゴリ、ベータステータス、カタログアイコンに関する属性など、ソースに固有の情報が含まれます。 また、URL パラメーター、コンテンツ、ヘッダー、スケジュールなどの便利な情報も含まれています。 また、ソース仕様は、ベース接続からソース接続を作成するために必要なパラメータのスキーマも記述します。 ソース接続を作成するには、スキーマが必要です。

詳しくは、 [付録](#source-spec) ：完全に移入されたソース仕様の例。


```json
"sourceSpec": {
  "attributes": {
    "uiAttributes": {
      "isSource": true,
      "isPreview": true,
      "isBeta": true,
      "category": {
        "key": "{CATEGORY}"
      },
      "icon": {
        "key": "{ICON}"
      },
      "description": {
        "key": "{DESCRIPTION}"
      },
      "label": {
        "key": "{LABEL}"
      }
    },
    "urlParams": {
      "path": "{RESOURCE_PATH}",
      "method": "{GET_or_POST}",
      "queryParams": "{QUERY_PARAMS}"
    },
    "headerParams": "{HEADER_VALUES}",
    "bodyParams": "{BODY_PARAMS_USED_IF_METHOD_IS_POST}",
    "contentPath": {
      "path": "{PATH_SHOULD_POINT_TO_COLLECTION_OF_RECORDS}",
      "skipAttributes": [],
      "overrideWrapperAttribute": "{OVERRIDE_ATTRIBUTES}",
      "keepAttributes": ["action", "type", "timestamp"]
    },
    "explodeEntityPath": {
      "path": "{PATH_SHOULD_POINT_TO_COLLECTION_OF_RECORDS}",
      "skipAttributes": [],
      "overrideWrapperAttribute": "{OVERRIDE_ATTRIBUTES}",
      "keepAttributes": ["action", "type", "timestamp"]
    },
    "paginationParams": {
      "type": "{OFFSET_OR_POINTER}",
      "limitName": "{NUMBER_OF_RECORDS_ATTRIBUTE_NAME}",
      "limitValue": "{NUMBER_OF_RECORDS_PER_PAGE}",
      "offSetName": "{OFFSET_ATTRIBUTE_NAME_REQUIRED_IN_CASE_OF_OFFSET BASED_PAGINATION}",
      "pointerName": "{POINTER_PATH_REQUIRED_IN__CASE_OF_POINTER BASED_PAGINATION}"
    },
    "scheduleParams": {
      "scheduleStartParamName": "{START_TIME_PARAMETER_NAME}",
      "scheduleEndParamName": "{END_TIME_PARAMETER_NAME}",
      "scheduleStartParamFormat": "{DATE_TIME_FORMAT_FOR_START_TIME}",
      "scheduleEndParamFormat": "{END_TIME_FORMAT_FOR_START_TIME}"
    }
  },
  "spec": {
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "description": "Define user input parameters to fetch resource values.",
    "properties": "{USER_INPUT}"
  }
}
```

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `sourceSpec.attributes` | UI または API に固有のソースに関する情報が含まれます。 |
| `sourceSpec.attributes.uiAttributes` | UI に固有のソースに関する情報を表示します。 |
| `sourceSpec.attributes.uiAttributes.isBeta` | 機能に追加するために、ソースで顧客からのより多くのフィードバックが必要かどうかを示す boolean 属性です。 | <ul><li>`true`</li><li>`false`</li></ul> |
| `sourceSpec.attributes.uiAttributes.category` | ソースのカテゴリを定義します。 | <ul><li>`advertising`</li><li>`cloud storage`</li><li>`crm`</li><li>`customer success`</li><li>`database`</li><li>`ecommerce`</li><li>`marketing automation`</li><li>`payments`</li><li>`protocols`</li><li>`streaming`</li></ul> |
| `sourceSpec.attributes.uiAttributes.icon` | Platform UI でソースのレンダリングに使用するアイコンを定義します。 | `mailchimp-icon.svg` |
| `sourceSpec.attributes.uiAttributes.description` | ソースの簡単な説明を表示します。 |
| `sourceSpec.attributes.uiAttributes.label` | Platform UI でソースのレンダリングに使用するラベルを表示します。 |
| `sourceSpec.attributes.urlParams` | URL リソースのパス、メソッド、およびサポートされているクエリパラメーターに関する情報が含まれます。 |
| `sourceSpec.attributes.urlParams.path` | データの取得元となるリソースパスを定義します。 | `/3.0/reports/${campaignId}/email-activity` |
| `sourceSpec.attributes.urlParams.method` | データを取得するためにリソースにリクエストを送信する際に使用する HTTP メソッドを定義します。 | `GET`、`POST` |
| `sourceSpec.attributes.urlParams.queryParams` | データを取得するリクエストを実行する際にソース URL を追加するために使用できる、サポートされているクエリパラメーターを定義します。 クエリパラメーターはコンマ (`,`) 区切りのキーと値のペアを使用することをお勧めします。 **注意**:ユーザーが指定したパラメータ値は、プレースホルダーとして書式設定する必要があります。 例：`${USER_PARAMETER}`。 | `exclude_fields=emails._links,id=${id}` |
| `sourceSpec.attributes.headerParams` | コンマ (`,`) で区切られたヘッダーで、データの取得時にソース URL への HTTP リクエストで指定する必要があります。 | `Content-Type=application/json,foo=bar&userHeader={{USER_HEADER_VALUE}}` |
| `sourceSpec.attributes.bodyParams` | 必須の本文パラメーターを定義します。 このプロパティは、 `urlParams.method` が `POST`. |
| `sourceSpec.attributes.contentPath` | Platform に取り込む必要がある項目のリストを含むノードを定義します。 この属性は、有効な JSON パス構文に従い、特定の配列を指す必要があります。 | 詳しくは、 [付録](#content-path) 例えば、コンテンツパス内に含まれるリソースの例です。 |
| `sourceSpec.attributes.contentPath.path` | Platform に取り込まれるコレクションレコードを指すパス。 | `$.emails` |
| `sourceSpec.attributes.contentPath.skipAttributes` | このプロパティを使用すると、取り込みから除外するコンテンツパスで識別されるリソースの特定の項目を識別できます。 |
| `sourceSpec.attributes.contentPath.overrideWrapperAttribute` | このプロパティを使用すると、 `contentPath`. |
| `sourceSpec.attributes.contentPath.keepAttributes` | このプロパティを使用すると、マッピングする個々の属性を明示的に指定できます。 |
| `sourceSpec.attributes.explodeEntityPath` | このプロパティを使用すると、2 つの配列を統合し、リソースデータを Platform リソースに変換できます。 |
| `sourceSpec.attributes.explodeEntityPath.path` | 統合するコレクションレコードを指すパスです。 | `$.email.activity` |
| `sourceSpec.attributes.explodeEntityPath.skipAttributes` | このプロパティを使用すると、取り込みから除外するエンティティパスで識別されるリソースから特定の項目を識別できます。 |
| `sourceSpec.attributes.explodeEntityPath.overrideWrapperAttribute` | このプロパティを使用すると、 `explodeEntityPath`. |
| `sourceSpec.attributes.explodeEntityPath.keepAttributes` | このプロパティを使用すると、マッピングする個々の属性を明示的に指定できます。 |
| `sourceSpec.attributes.paginationParams` | ユーザーの現在のページ応答から次のページへのリンクを取得するため、または次のページ URL を作成する際に指定する必要があるパラメーターまたはフィールドを定義します。 |
| `sourceSpec.attributes.paginationParams.type` | ソースでサポートされているページネーションタイプを表示します。 | <ul><li>`offset`:このページネーションタイプを使用すると、結果の配列を開始する場所のインデックスと、返される結果の数の制限を指定することで、結果を解析できます。</li><li>`pointer`:このページネーションタイプでは、 `pointer` 変数を使用して、リクエストと共に送信する必要がある特定の項目を指定します。 ポインタータイプのページネーションには、次のページを指すペイロード内のパスが必要です</li></ul> |
| `sourceSpec.attributes.paginationParams.limitName` | API がページで取得するレコード数を指定できる制限の名前。 | `count` |
| `sourceSpec.attributes.paginationParams.limitValue` | ページで取得するレコードの数。 | `100` |
| `sourceSpec.attributes.paginationParams.offSetName` | オフセット属性名。 これは、ページネーションタイプが `offset`. | `offset` |
| `sourceSpec.attributes.paginationParams.pointerName` | pointer 属性名。 次のページを指す属性への json パスが必要です。 これは、ページネーションタイプが `pointer`. | `pointer` |
| `sourceSpec.attributes.scheduleParams` | ソースでサポートされるスケジューリング形式を定義するパラメーターが含まれます。 スケジュールのパラメーターは次のとおりです `startTime` および `endTime`を使用すると、両方とも、バッチ実行に特定の時間間隔を設定でき、前のバッチ実行で取得されたレコードが再び取得されなくなります。 |
| `sourceSpec.attributes.scheduleParams.scheduleStartParamName` | 開始時間のパラメーター名を定義します | `since_last_changed` |
| `sourceSpec.attributes.scheduleParams.scheduleEndParamName` | 終了時間のパラメーター名を定義します | `before_last_changed` |
| `sourceSpec.attributes.scheduleParams.scheduleStartParamFormat` | でサポートされるの形式を定義します。 `scheduleStartParamName`. | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.attributes.scheduleParams.scheduleEndParamFormat` | でサポートされるの形式を定義します。 `scheduleEndParamName`. | `yyyy-MM-ddTHH:mm:ssZ` |
| `sourceSpec.spec.properties` | リソース値を取得するためのユーザー指定のパラメーターを定義します。 | 詳しくは、 [付録](#user-input) 例： `spec.properties`. |

{style=&quot;table-layout:auto&quot;}

## 付録 {#appendix}

### コンテンツパスの例 {#content-path}

次に、 `contentPath` プロパティ [!DNL MailChimp Campaigns] 接続の仕様。

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

次に、ユーザーが指定した例を示します `spec.properties` の使用 [!DNL MailChimp Members] 接続の仕様。

この例では、 `listId` は、 `urlParams.path`. を `listId` 顧客から、それを `spec.properties`.


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

以下は、 [!DNL MailChimp Members]:

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

ソースの仕様を入力したら、Platform に統合するソースの探索仕様を構成できます。 ドキュメントを [仕様の構成](./explorespec.md) を参照してください。