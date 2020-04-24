---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: query templates
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# クエリテンプレート

## サンプルAPI呼び出し

これで、使用するヘッダーを理解できたので、クエリサービスAPIの呼び出しを開始できます。 以下の節では、クエリサービスAPIを使用して行える様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプル応答が含まれます。

### テンプレートのリストの取得

エンドポイントにGET要求を行うことで、IMS組織のすべてのクエリテンプレートのリストを取得で `/query-templates` きます。

**API形式**

```http
GET /query-templates
GET /query-templates?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | (オ&#x200B;*プション*)応答に返される結果を設定する要求パスに追加されるパラメーター。 複数のパラメーターを含め、アンパサンド(`&`)で区切ることができます。 使用可能なパラメーターを以下に示します。 |

**クエリパラメータ**

次に、使用可能なクエリパラメータのリストを示します。クエリテンプレートの一覧を示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリテンプレートが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果を並べ替えるフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、は、 `orderby=created` 昇順で結果を並べ替えます。 「作成前( `-` )」を追加す`orderby=-created`ると、アイテムが作成された降順で並べ替えられます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。 (*Default value: 20*) |
| `start` | ゼロベースの番号付けを使用して、応答リストをオフセットします。 例えば、3番目 `start=2` のリストのリストからクエリを返します。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** HTMLエスケープする必要があります。 複数のコンマを使用して、複数のフィルターを組み合わせます。 サポートされているフィールドは `name` とで `userId`す。 サポートされている演算 `==` 子は（等しい）だけです。 例えば、は、名前を `name==my_template` 持つすべてのクエリテンプレートを返しま `my_template`す。 |

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

>[!NOTE] の値を使用して、テンプレ `_links.delete` ート [を削除できます](#delete-a-specified-query-template)。

### テンプレートのクエリ作成

エンドポイントにPOSTリクエストをクエリすることで、エンドポイントテンプレートを作成 `/query-templates` できます。

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
| `name` | テンプレートのクエリ名。 |

**応答**

応答が成功すると、新しく作成された応答テンプレートの詳細と共に、HTTPステータス202（受け入れ済み）がクエリされます。

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

>[!NOTE] の値を使用して、テンプレ `_links.delete` ート [を削除できます](#delete-a-specified-query-template)。

### 指定したテンプレートのクエリ

特定のクエリテンプレートを取得するには、エンドポイントにGET要求を行い、 `/query-templates/{TEMPLATE_ID}` 要求パスにクエリテンプレートのIDを指定します。

**API形式**

```http
GET /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `{TEMPLATE_ID}` | 取得 `id` するクエリテンプレートの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定した応答テンプレートの詳細と共にHTTPステータス200をクエリします。

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

>[!NOTE] の値を使用して、テンプレ `_links.delete` ート [を削除できます](#delete-a-specified-query-template)。

### 指定したテンプレートのクエリの更新

特定のクエリテンプレートを更新するには、エンドポイントにPUT要求を行い、 `/query-templates/{TEMPLATE_ID}` 要求パスにクエリテンプレートのIDを指定します。

**API形式**

```http
PUT /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TEMPLATE_ID}` | 取得 `id` するクエリテンプレートの値。 |

**リクエスト**

>[!NOTE] PUT要求では、sqlとnameの両方のフィールドに入力する必要があり、そのクエリ **テンプ** レートの現在の内容が上書きされます。

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

成功した応答は、指定した応答テンプレートの更新された情報と共に、HTTPステータス202（受け入れ済み）をクエリします。

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

>[!NOTE] の値を使用して、テンプレ `_links.delete` ート [を削除できます](#delete-a-specified-query-template)。

### 指定したテンプレートのクエリの削除

特定のクエリテンプレートを削除するには、にDELETEリクエストを行い、 `/query-templates/{TEMPLATE_ID}` リクエストパスにクエリテンプレートのIDを指定します。

**API形式**

```http
DELETE /query-templates/{TEMPLATE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{TEMPLATE_ID}` | 取得 `id` するクエリテンプレートの値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/query/query-templates/0094d000-9062-4e6a-8fdb-05606805f08f
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、次のメッセージと共にHTTPステータス202（受け入れ済み）を返します。

```json
{
    "message": "Deleted",
    "statusCode": 202
}
```
