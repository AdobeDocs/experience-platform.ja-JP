---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；クエリテンプレート；api ガイド；テンプレート；クエリサービス；
solution: Experience Platform
title: クエリテンプレート API エンドポイント
description: このガイドでは、クエリサービス APIを使用して実行できるさまざまなクエリテンプレート API呼び出しについて詳しく説明します。
role: Developer
exl-id: 14cd7907-73d2-478f-8992-da3bdf08eacc
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 44%

---

# クエリテンプレートエンドポイント

## サンプル API 呼び出し

次の節では、[!DNL Query Service] APIを使用して実行できるさまざまなAPI呼び出しについて説明します。 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

Experience Platform UIを使用してテンプレートを作成する方法については、[UI クエリテンプレートのドキュメント &#x200B;](../ui/query-templates.md)を参照してください。

### クエリテンプレートのリストの取得

`/query-templates` エンドポイントに対してGET リクエストを行うことで、組織のすべてのクエリテンプレートのリストを取得できます。

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
| `start` | 結果を並べ替えるには、ISO形式のタイムスタンプを指定します。 開始日が指定されていない場合、API呼び出しは、最も古い作成されたテンプレートを最初に返し、その後、より最近の結果を引き続きリストします。<br>個のISO タイムスタンプを使用すると、日付と時刻に異なるレベルの詳細を設定できます。 基本的なISO タイムスタンプは、2020年9月7日の日付を表すために`2020-09-07`の形式になります。 より複雑な例では、`2022-11-05T08:15:30-05:00`と書かれ、2022年11月5日（PT）午前8:15:30、米国東部標準時に対応します。 タイムゾーンはUTC オフセットで指定でき、サフィックス「Z」（`2020-01-01T01:01:01Z`）で示されます。 タイムゾーンが指定されていない場合、デフォルトは0になります。 |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のコンマを使用して、複数のフィルターを組み合わせます。サポートされているフィールドは `name` と `userId` です。サポートされている演算子は `==`（等しい）だけです。例えば、`name==my_template` は、`my_template` という名前を持つすべてのクエリテンプレートを返します。 |

**リクエスト**

次のリクエストは、組織に対して作成された最新のクエリテンプレートを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、指定した組織のクエリテンプレートのリストを含むHTTP ステータス 200が返されます。 次の応答は、組織に対して作成された最新のクエリテンプレートを返します。

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
>`_links.delete`の値を使用して、クエリテンプレート [を](#delete-a-specified-query-template)削除できます。

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
| `sql` | 作成するSQL クエリ。 標準のSQLまたはパラメーターの置換を使用できます。 SQLでパラメーター置換を使用するには、パラメーターのキーの先頭に`$`を付ける必要があります。 例：`$key`。SQLで使用されるパラメーターを`queryParameters` フィールドのJSON キー値のペアとして指定します。 ここで渡される値は、テンプレートで使用されるデフォルトのパラメーターになります。 これらのパラメーターを上書きする場合は、POST リクエストでそれらを上書きする必要があります。 |
| `name` | テンプレートのクエリ名。 |
| `queryParameters` | SQL ステートメント内のパラメーター化された値を置き換えるキー値のペア。 指定したSQL内でパラメーターの置換を使用している場合&#x200B;**のみ必須です。**&#x200B;これらのキー値のペアに対して、値タイプのチェックは行われません。 |

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
>`_links.delete`の値を使用して、クエリテンプレート [を](#delete-a-specified-query-template)削除できます。

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
>`_links.delete`の値を使用して、クエリテンプレート [を](#delete-a-specified-query-template)削除できます。

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
>PUT リクエストでは、sql フィールドとname フィールドの両方を入力する必要があり、そのクエリテンプレートの現在の内容を&#x200B;**上書き**&#x200B;します。

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
| `sql` | 作成するSQL クエリ。 標準のSQLまたはパラメーターの置換を使用できます。 SQLでパラメーター置換を使用するには、パラメーターのキーの先頭に`$`を付ける必要があります。 例：`$key`。SQLで使用されるパラメーターを`queryParameters` フィールドのJSON キー値のペアとして指定します。 ここで渡される値は、テンプレートで使用されるデフォルトのパラメーターになります。 これらのパラメーターを上書きする場合は、POST リクエストでそれらを上書きする必要があります。 |
| `name` | テンプレートのクエリ名。 |
| `queryParameters` | SQL ステートメント内のパラメーター化された値を置き換えるキー値のペア。 指定したSQL内でパラメーターの置換を使用している場合&#x200B;**のみ必須です。**&#x200B;これらのキー値のペアに対して、値タイプのチェックは行われません。 |

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
>`_links.delete`の値を使用して、クエリテンプレート [を](#delete-a-specified-query-template)削除できます。

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
