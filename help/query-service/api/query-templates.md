---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリテンプレート；api ガイド；テンプレート；クエリサービス；
solution: Experience Platform
title: クエリテンプレート API エンドポイント
description: このガイドでは、クエリサービス API を使用して実行できる様々なクエリテンプレート API 呼び出しの詳細を説明します。
exl-id: 14cd7907-73d2-478f-8992-da3bdf08eacc
source-git-commit: 958d5c322ff26f7372f8ab694a70ac491cbff56c
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 54%

---

# クエリテンプレートエンドポイント

## サンプル API 呼び出し

以下の節では、 [!DNL Query Service] API. 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

詳しくは、 [UI クエリテンプレートドキュメント](../ui/query-templates.md) テンプレート UI を使用したExperience Platform作成の詳細。

### クエリテンプレートのリストの取得

組織のすべてのクエリテンプレートのリストを取得するには、 `/query-templates` endpoint.

**API 形式**

```http
GET /query-templates
GET /query-templates?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | （*オプション*）リクエストパスに追加されるパラメーター。このリクエストパスは応答で返される結果を設定します。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。使用できるパラメーターは以下のとおりです。 |

**クエリパラメーター**

次に、クエリテンプレートを一覧表示するために使用可能なクエリパラメータ－のリストを示します。これらのパラメーターはすべてオプションです。パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリテンプレートが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。サポートされているフィールドは `created` と `updated` です。例えば、`orderby=created` は、昇順で結果を並べ替えます。作成前に `-` を追加する（`orderby=-created`）と、項目が作成日の降順で並べ替えられます。 |
| `limit` | ページサイズの制限を指定して、ページに含める結果の数を制御します。（*デフォルト値：20*） |
| `start` | ISO 形式のタイムスタンプを指定して、結果を並べ替えます。 開始日が指定されていない場合、API 呼び出しは最も古く作成されたテンプレートを最初に返し、続けて最新の結果のリストを表示します。<br> ISO タイムスタンプは、日時の様々な精度レベルを許可します。 基本的な ISO タイムスタンプの形式は次のとおりです。 `2020-09-07` 日付を表すには、2020 年 9 月 7 日。 より複雑な例は次のように記述されます。 `2022-11-05T08:15:30-05:00` 2022 年 11 月 5 日 (PT)、8 日 (PT) に相当:15:午前 30 時、米国東部標準時。 タイムゾーンは UTC オフセットで指定でき、サフィックス「Z」(`2020-01-01T01:01:01Z`) をクリックします。 タイムゾーンを指定しない場合、デフォルトは 0 に設定されます。 |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のコンマを使用して、複数のフィルターを組み合わせます。サポートされているフィールドは `name` と `userId` です。サポートされている演算子は `==`（等しい）だけです。例えば、`name==my_template` は、`my_template` という名前を持つすべてのクエリテンプレートを返します。 |

**リクエスト**

次のリクエストは、 組織用に作成された最新のクエリテンプレートを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定した組織のクエリテンプレートのリストを返します。 次の応答は、 組織用に作成された最新のクエリテンプレートを返します。

```json
{
    "templates": [
        {
            "sql": "SELECT *\nFROM\n  accounts\nLIMIT 10\n",
            "name": "Test",
            "id": "f7cb5155-29da-4b95-8131-8c5deadfbe7f",
            "updated": "2019-11-21T21:50:01.469Z",
            "userId": "{USER_ID}",
            "created": "2019-11-21T21:50:01.469Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/query-templates/f7cb5155-29da-4b95-8131-8c5deadfbe7f",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/query-templates/f7cb5155-29da-4b95-8131-8c5deadfbe7f",
                    "method": "DELETE"
                },
                "update": {
                    "href": "https://platform.adobe.io/data/foundation/query/query-templates/f7cb5155-29da-4b95-8131-8c5deadfbe7f",
                    "method": "PUT",
                    "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
                }
            }
        }
    ],
    "_page": {
        "orderby": "-created",
        "start": "2019-11-21T21:50:01.469Z",
        "next": "2019-11-21T21:50:01.469Z",
        "count": 1
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates?orderby=-created&start=2019-11-21T21:50:01.469Z"
        },
        "prev": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates?orderby=-created&start=2019-11-21T21:50:01.469Z&isPrevLink=true"
        }
    },
    "version": 1
}
```

>[!NOTE]
>
>`_links.delete` の値を使用して、[テンプレートを削除](#delete-a-specified-query-template)できます。

### クエリテンプレートの作成

`/query-templates` エンドポイントに POST リクエストを実行することで、クエリテンプレートを作成できます。

**API 形式**

```http
POST /query-templates
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/query-templates
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "sql": "SELECT account_balance FROM user_data WHERE user_id='$user_id';",
        "name": "Sample query template",
        "queryParameters": {
            user_id : {USER_ID}
            }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sql` | 作成する SQL クエリ。 標準の SQL またはパラメータの置き換えを使用できます。 SQL でパラメータの置換を使用するには、パラメータキーの前に `$`. 例： `$key`を使用し、SQL で使用されるパラメーターを JSON キーと値のペアとして `queryParameters` フィールドに入力します。 ここで渡される値は、テンプレートで使用されるデフォルトのパラメーターです。 これらのパラメーターを上書きする場合は、POSTリクエストで上書きする必要があります。 |
| `name` | テンプレートのクエリ名。 |
| `queryParameters` | SQL 文内のパラメーター化された値を置き換えるキー値の組み合わせ。 必要なのは **if** 指定した SQL 内でパラメータ置換を使用しています。 これらのキー値ペアに対しては、値タイプのチェックはおこなわれません。 |

**応答**

正常な応答は、HTTP ステータス 202（受け入れ済み）と、新しく作成されたクエリテンプレートの詳細を返します。

```json
{
    "sql": "SELECT account_balance FROM user_data WHERE user_id='$user_id';",
    "name": "Sample query template",
    "id": "0094d000-9062-4e6a-8fdb-05606805f08f",
    "updated": "2020-01-09T00:20:09.670Z",
    "userId": "{USER_ID}",
    "created": "2020-01-09T00:20:09.670Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "DELETE"
        },
        "update": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "PUT",
            "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>`_links.delete` の値を使用して、[テンプレートを削除](#delete-a-specified-query-template)できます。

### 指定されたクエリテンプレートの取得

`/query-templates/{TEMPLATE_ID}` エンドポイントに GET リクエストを実行して、リクエストパスにクエリテンプレートの ID を指定することで、特定のクエリテンプレートを取得できます。

**API 形式**

```http
GET /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `{TEMPLATE_ID}` | 取得するクエリテンプレートの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と指定されたクエリテンプレートの詳細を返します。

```json
{
    "sql": "SELECT * FROM accounts;",
    "name": "Sample query template",
    "id": "0094d000-9062-4e6a-8fdb-05606805f08f",
    "updated": "2020-01-09T00:20:09.670Z",
    "userId": "A5A562D15E1645480A495CE1@techacct.adobe.com",
    "created": "2020-01-09T00:20:09.670Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "DELETE"
        },
        "update": {
            "href": "https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "PUT",
            "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>`_links.delete` の値を使用して、[テンプレートを削除](#delete-a-specified-query-template)できます。

### 指定されたクエリテンプレートの更新

`/query-templates/{TEMPLATE_ID}` エンドポイントに PUT リクエストを実行して、リクエストパスにクエリテンプレートの ID を指定することで、特定のクエリテンプレートを更新できます。

**API 形式**

```http
PUT /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TEMPLATE_ID}` | 取得するクエリテンプレートの `id` 値。 |

**リクエスト**

>[!NOTE]
>
> PUT リクエストでは、sql と name の両方のフィールドに入力する必要があり、そのクエリテンプレートの現在の内容が&#x200B;**上書き**&#x200B;されます。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "sql": "SELECT account_balance FROM user_data WHERE user_id='$user_id';",
    "name": "Sample query template",
    "queryParameters": {
            user_id : {USER_ID}
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sql` | 作成する SQL クエリ。 標準の SQL またはパラメータの置き換えを使用できます。 SQL でパラメータの置換を使用するには、パラメータキーの前に `$`. 例： `$key`を使用し、SQL で使用されるパラメーターを JSON キーと値のペアとして `queryParameters` フィールドに入力します。 ここで渡される値は、テンプレートで使用されるデフォルトのパラメーターです。 これらのパラメーターを上書きする場合は、POSTリクエストで上書きする必要があります。 |
| `name` | テンプレートのクエリ名。 |
| `queryParameters` | SQL 文内のパラメーター化された値を置き換えるキー値の組み合わせ。 必要なのは **if** 指定した SQL 内でパラメータ置換を使用しています。 これらのキー値ペアに対しては、値タイプのチェックはおこなわれません。 |

**応答**

正常な応答は、HTTP ステータス 202（受け入れ済み）と、指定されたクエリテンプレートの更新された情報を返します。

```json
{
    "sql": "SELECT * FROM accounts LIMIT 20;",
    "name": "Sample query template",
    "id": "0094d000-9062-4e6a-8fdb-05606805f08f",
    "updated": "2020-01-09T00:29:20.028Z",
    "lastUpdatedBy": "{USER_ID}",
    "userId": "{USER_ID}",
    "created": "2020-01-09T00:20:09.670Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/query_templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/query_templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "DELETE"
        },
        "update": {
            "href": "https://platform.adobe.io/data/foundation/query/query_templates/0094d000-9062-4e6a-8fdb-05606805f08f",
            "method": "PUT",
            "body": "{\"sql\": \"new sql \", \"name\": \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>`_links.delete` の値を使用して、[テンプレートを削除](#delete-a-specified-query-template)できます。

### 指定されたクエリテンプレートの削除

`/query-templates/{TEMPLATE_ID}` に DELETE リクエストを実行して、リクエストパスにクエリテンプレートの ID を指定することで、特定のクエリテンプレートを削除できます。

**API 形式**

```http
DELETE /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TEMPLATE_ID}` | 取得するクエリテンプレートの `id` 値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Deleted",
    "statusCode": 202
}
```
