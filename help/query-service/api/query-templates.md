---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: query templates
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 3%

---


# クエリテンプレート

## サンプルAPI呼び出し

これで、使用するヘッダーが分かったので、クエリサービスAPIの呼び出しを開始する準備が整いました。 以下の節では、クエリサービスAPIを使用して実行できる様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

### クエリテンプレートのリストの取得

エンドポイントにGETリクエストを行うと、IMS組織のすべてのクエリテンプレートのリストを取得でき `/query-templates` ます。

**API形式**

```http
GET /query-templates
GET /query-templates?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | (*オプション*)リクエストパスに追加されるパラメーター。応答で返される結果を設定します。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。 使用可能なパラメーターを以下に示します。 |

**クエリパラメーター**

次に、クエリテンプレートのリストを表示する際に使用できるクエリパラメータのリストを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリテンプレートが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、 `orderby=created` は、作成された結果を昇順で並べ替えます。 作成 `-` 前(`orderby=-created`)を追加すると、作成されたアイテムを降順で並べ替えます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズ制限を指定します。 (*Default value: 20*) |
| `start` | 0から始まる番号を使用して、応答のリストをオフセットします。 例えば、3番目 `start=2` にリストされたクエリから開始するリストが返されます。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** 、HTMLエスケープする必要があります。 複数のフィルターセットを組み合わせる場合は、コンマを使用します。 サポートされているフィールドは `name` とで `userId`す。 サポートされている演算子は `==` （等しい）だけです。 例えば、 `name==my_template` は、名前が付いたすべてのクエリテンプレートを返し `my_template`ます。 |

**リクエスト**

次のリクエストは、IMS組織用に作成された最新のクエリテンプレートを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したIMS組織のクエリテンプレートのリストを含むHTTPステータス200を返します。 次の応答は、IMS組織用に作成された最新のクエリテンプレートを返します。

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
                    "body": "{\"sql\" : \"new sql \", \"name\" : \"new name\"}"
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
>の値を使用して、クエリテンプレート `_links.delete` を [削除できます](#delete-a-specified-query-template)。

### クエリテンプレートの作成

エンドポイントにPOSTリクエストを作成して、クエリテンプレートを作成でき `/query-templates` ます。

**API形式**

```http
POST /query-templates
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/query-templates
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "sql": "SELECT * FROM accounts;",
        "name": "Sample query template"
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sql` | 作成するSQLクエリ。 |
| `name` | クエリテンプレートの名前。 |

**応答**

正常に応答すると、新しく作成したクエリテンプレートの詳細と共に、HTTPステータス202（受け入れ済み）が返されます。

```json
{
    "sql": "SELECT * FROM accounts;",
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
            "body": "{\"sql\" : \"new sql \", \"name\" : \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>の値を使用して、クエリテンプレート `_links.delete` を [削除できます](#delete-a-specified-query-template)。

### 指定したクエリテンプレートの取得

特定のクエリテンプレートを取得するには、エンドポイントにGETリクエストを送信し、リクエストパスにクエリテンプレートのIDを指定し `/query-templates/{TEMPLATE_ID}` ます。

**API形式**

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、指定したクエリテンプレートの詳細と共にHTTPステータス200が返されます。

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
            "body": "{\"sql\" : \"new sql \", \"name\" : \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>の値を使用して、クエリテンプレート `_links.delete` を [削除できます](#delete-a-specified-query-template)。

### 指定したクエリテンプレートの更新

エンドポイントにPUT要求を行い、要求パスにクエリテンプレートのIDを指定することで、特定のクエリテンプレートを更新でき `/query-templates/{TEMPLATE_ID}` ます。

**API形式**

```http
PUT /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TEMPLATE_ID}` | 取得するクエリテンプレートの `id` 値。 |

**リクエスト**

>[!NOTE]
>
>PUT要求では、sqlとnameの両方のフィールドに値を入力する必要があり、そのクエリテンプレートの現在の内容が **上書きされます** 。

```shell
curl -X PUT https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "sql": "SELECT * FROM accounts LIMIT 20;",
    "name": "Sample query template"
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `sql` | 更新するSQLクエリ。 |
| `name` | スケジュールされたクエリの名前。 |

**応答**

正常に応答すると、指定したクエリテンプレートに対して更新された情報と共に、HTTPステータス202（受け入れ済み）が返されます。

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
            "body": "{\"sql\" : \"new sql \", \"name\" : \"new name\"}"
        }
    }
}
```

>[!NOTE]
>
>の値を使用して、クエリテンプレート `_links.delete` を [削除できます](#delete-a-specified-query-template)。

### 指定したクエリテンプレートの削除

特定のクエリテンプレートを削除するには、にDELETEリクエストを送信し、そのクエリテンプレートのIDをリクエストパス `/query-templates/{TEMPLATE_ID}` に指定します。

**API形式**

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、次のメッセージと共にHTTPステータス202（受け入れ済み）が返されます。

```json
{
    "message": "Deleted",
    "statusCode": 202
}
```
