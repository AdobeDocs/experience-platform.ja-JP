---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# クエリ

## サンプルAPI呼び出し

以降の節では、エンドポイントサービスAPIのエンドポイントを使用し `/queries` て行う呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプル応答が含まれます。

### リストの取得

IMS組織のすべてのクエリのリストを取得するには、エンドポイントにGETリクエストを作成 `/queries` します。

**API形式**

```http
GET /queries
GET /queries?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`:(オ&#x200B;*プション*)応答に返される結果を設定する要求パスに追加されるパラメーター。 複数のパラメーターを含め、アンパサンド(`&`)で区切ることができます。 使用可能なパラメーターを以下に示します。

**クエリパラメータ**

次に、使用可能なクエリパラメーターのリストを示します。クエリの一覧表示 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果を並べ替えるフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、は、 `orderby=created` 昇順で結果を並べ替えます。 「作成前( `-` )」を追加す`orderby=-created`ると、アイテムが作成された降順で並べ替えられます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。 (*Default value: 20*) |
| `start` | ゼロベースの番号付けを使用して、応答リストをオフセットします。 例えば、3番目 `start=2` のリストのリストからクエリを返します。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** HTMLエスケープする必要があります。 複数のコンマを使用して、複数のフィルターを組み合わせます。 サポートされているフ `created`ィール `updated`ドは、、、 `state`およびで `id`す。 サポートされる演算子のリストは、 `>` （より大きい）、 `<` （より小さい）、 `>=` （より大きいか等しい）、 `<=` （より小さいか等しい）、 `==` （より大きい）、（より大きい）、 `!=` （より大きい）、 `~` （含む）です。 例えば、は指定したID `id==6ebd9c2d-494d-425a-aa91-24033f3abeec` を持つすべてのクエリを返します。 |
| `excludeSoftDeleted` | ソフト削除されたクエリを含める必要があるかどうかを示します。 例えば、はソフト `excludeSoftDeleted=false` 削除 **** クエリを含む。 (ブ&#x200B;*ール値、デフォルト値：true*) |
| `excludeHidden` | 非ユーザー主導のクエリを表示するかどうかを示します。 この値をfalseに設定すると、CURSOR定 **義** 、FETCH、メタデータクエリなど、ユーザ主導以外のクエリが含まれます。 (ブ&#x200B;*ール値、デフォルト値：true*) |

**リクエスト**

次のリクエストで、IMS組織用に作成された最新のクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/queries?limit=1 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したIMS組織のクエリのリストをJSONとしてHTTPステータス200を返します。 次の応答は、IMS組織用に作成された最新のクエリを返します。

```json
{
    "queries": [
        {
            "isInsertInto": false,
            "request": {
                "dbName": "prod:all",
                "sql": "SELECT *\nFROM\n  accounts\nLIMIT 10\n"
            },
            "state": "SUCCESS",
            "rowCount": 0,
            "errors": [],
            "isCTAS": false,
            "version": 1,
            "id": "9957bd7f-2244-4fd5-91bc-077d7df1d8e5",
            "elapsedTime": 28,
            "updated": "2019-12-06T22:00:17.390Z",
            "client": "Adobe Query Service UI",
            "userId": "{USER_ID}",
            "created": "2019-12-06T22:00:17.362Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/queries/9957bd7f-2244-4fd5-91bc-077d7df1d8e5",
                    "method": "GET"
                },
                "soft_delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/queries/9957bd7f-2244-4fd5-91bc-077d7df1d8e5",
                    "method": "PATCH",
                    "body": "{ \"op\": \"soft_delete\"}"
                },
                "referenced_datasets": [
                    {
                        "id": "5b2bdd32230d4401de87397c",
                        "href": "https://platform.adobe.io/data/foundation/catalog/dataSets/5b2bdd32230d4401de87397c"
                    }
                ]
            }
        }
    ],
    "_page": {
        "orderby": "-created",
        "start": "2019-12-06T22:00:17.362Z",
        "next": "2019-08-01T00:14:21.748Z",
        "count": 1
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io/data/foundation/query/queries?orderby=-created&start=2019-08-01T00:14:21.748Z"
        },
        "prev": {
            "href": "https://platform.adobe.io/data/foundation/query/queries?orderby=-created&start=2019-12-06T22:00:17.362Z&isPrevLink=true"
        }
    },
    "version": 1
}
```

### クエリ

エンドポイントにPOSTリクエストをクエリすることで、新しいエンドポイントを作成で `/queries` きます。

**API形式**

```http
POST /queries
```

**リクエスト**

次のリクエストは、ペイロードで指定された値によって設定された新しいクエリを作成します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/queries \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
        "dbName": "prod:all",
        "sql": "SELECT * FROM accounts;",
        "name": "Sample Query"
        "description": "Sample Description"
    }  
```

| プロパティ | 説明 |
| -------- | ----------- |
| `dbName` | SQLデータベースを作成する対象のクエリの名前。 |
| `sql` | 作成するSQLクエリ。 |
| `name` | SQLクエリの名前。 |
| `description` | SQLクエリの説明。 |

**応答**

応答が成功すると、新しく作成された応答の詳細と共にHTTPステータス202 （受け入れ済み）が返されます。クエリ クエリのアクティブ化が完了し、正常に実行されたら、 `state` がからに変更さ `SUBMITTED` れま `SUCCESS`す。

```json
{
    "isInsertInto": false,
    "request": {
        "dbName": "prod:all",
        "sql": "SELECT * FROM accounts;",
        "name": "Sample Query",
        "description": "Sample Description"
    },
    "state": "SUBMITTED",
    "rowCount": 0,
    "errors": [],
    "isCTAS": false,
    "version": 1,
    "id": "4d64cd49-cf8f-463a-a182-54bccb9954fc",
    "elapsedTime": 0,
    "updated": "2020-01-08T21:47:46.865Z",
    "client": "API",
    "userId": "{USER_ID}",
    "created": "2020-01-08T21:47:46.865Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc",
            "method": "GET"
        },
        "soft_delete": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc",
            "method": "PATCH",
            "body": "{ \"op\": \"soft_delete\"}"
        },
        "cancel": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc",
            "method": "PATCH",
            "body": "{ \"op\": \"cancel\"}"
        }
    }
}
```

>[!NOTE] の値を使用して、作成した `_links.cancel` クエリ [をキャンセルできます](#cancel-a-query)。

### IDによるクエリの取得

特定のクエリに関する詳細な情報を取得するには、エンドポイントにGETリクエストを送信し、 `/queries` リクエストパスにクエリの値を `id` 指定します。

**API形式**

```http
GET /queries/{QUERY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | 取得 `id` するクエリの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200を返し、指定された応答に関する詳細な情報がクエリされます。

```json
{
    "isInsertInto": false,
    "request": {
        "dbName": "prod:all",
        "sql": "SELECT * FROM accounts;",
        "name": "Sample Query",
        "description": "Sample Description"
    },
    "state": "SUBMITTED",
    "rowCount": 0,
    "errors": [],
    "isCTAS": false,
    "version": 1,
    "id": "4d64cd49-cf8f-463a-a182-54bccb9954fc",
    "elapsedTime": 0,
    "updated": "2020-01-08T21:47:46.865Z",
    "client": "API",
    "userId": "{USER_ID}",
    "created": "2020-01-08T21:47:46.865Z",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc",
            "method": "GET"
        },
        "soft_delete": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc",
            "method": "PATCH",
            "body": "{ \"op\": \"soft_delete\"}"
        },
        "cancel": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc",
            "method": "PATCH",
            "body": "{ \"op\": \"cancel\"}"
        }
    }
}
```

>[!NOTE] の値を使用して、作成した `_links.cancel` クエリ [をキャンセルできます](#cancel-a-query)。

### クエリ

エンドポイントにPATCHリクエストを送信し、リクエストパスにクエリの値を指定するこ `/queries` とで、指定したクエリを削 `id` 除するようにリクエストできます。

**API形式**

```http
PATCH /queries/{QUERY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | キャン `id` セルするクエリの値。 |


**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの仕組みについて詳しくは、APIの基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json',
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
   "op": "cancel"  
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `op` | クエリをキャンセルするには、opパラメーターに値を設定する必要がありま `cancel `す。 |

**応答**

応答が成功すると、HTTPステータス202（受け入れ済み）が次のメッセージと共に返されます。

```json
{
    "message": "Query cancel request received",
    "statusCode": 202
}
```
