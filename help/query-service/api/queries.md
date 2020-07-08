---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: queries
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 2%

---


# クエリ

## サンプルAPI呼び出し

以下の節では、クエリサービスAPIの `/queries` エンドポイントを使用した呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

### クエリのリストの取得

エンドポイントにGETリクエストを行うと、IMS組織のすべてのクエリのリストを取得でき `/queries` ます。

**API形式**

```http
GET /queries
GET /queries?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`: (*オプション*)リクエストパスに追加されるパラメーター。応答で返される結果を設定します。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。 使用可能なパラメーターを以下に示します。

**クエリパラメーター**

クエリを一覧表示する際に使用できるクエリパラメーターのリストを次に示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、 `orderby=created` は、作成された結果を昇順で並べ替えます。 作成 `-` 前(`orderby=-created`)を追加すると、作成されたアイテムを降順で並べ替えます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズ制限を指定します。 (*Default value: 20*) |
| `start` | 0から始まる番号を使用して、応答のリストをオフセットします。 例えば、3番目 `start=2` にリストされたクエリから開始するリストが返されます。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** 、HTMLエスケープする必要があります。 複数のフィルターセットを組み合わせる場合は、コンマを使用します。 サポートされているフィールドは、、、、 `created`および `updated``state``id`です。 サポートされる演算子は、 `>` （より大きい）、 `<` （より小さい）、 `>=` （より大きいか等しい）、 `<=` （より小さいか等しい）、（等しい）、 `==` （等しくない）、 `!=``~` （より小さい）の各リストです。 例えば、 `id==6ebd9c2d-494d-425a-aa91-24033f3abeec` は指定したIDを持つすべてのクエリを返します。 |
| `excludeSoftDeleted` | ソフト削除されたクエリを含める必要があるかどうかを示します。 例えば、 `excludeSoftDeleted=false` は、ソフト削除されたクエリを **含めます** 。 (*ブール値、デフォルト値： true*) |
| `excludeHidden` | ユーザー主導以外のクエリを表示する必要があるかどうかを示します。 この値をfalseに設定すると **、CURSOR定義、FETCH、メタデータクエリなど** 、ユーザ主導以外のクエリが含まれます。 (*ブール値、デフォルト値： true*) |

**リクエスト**

次のリクエストは、IMS組織用に作成された最新のクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/queries?limit=1 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したIMS組織のクエリのリストを持つHTTPステータス200をJSONとして返します。 次の応答は、IMS組織用に作成された最新のクエリを返します。

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

### クエリの作成

エンドポイントにPOSTリクエストを作成して、新しいクエリを作成でき `/queries` ます。

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
| `dbName` | SQLクエリを作成する対象のデータベースの名前。 |
| `sql` | 作成するSQLクエリ。 |
| `name` | SQLクエリの名前。 |
| `description` | SQLクエリの説明。 |

**応答**

正常に応答すると、新しく作成したクエリの詳細と共に、HTTPステータス202（受け入れ済み）が返されます。 クエリのアクティブ化が完了し、正常に実行されると、がからに変更 `state` さ `SUBMITTED` れ `SUCCESS`ます。

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

>[!NOTE]
>
>の値を使用して、作成したクエリ `_links.cancel` を [キャンセルできます](#cancel-a-query)。

### IDによるクエリの取得

エンドポイントにGETリクエストを送信し、リクエストパスにクエリの `/queries``id` 値を指定することで、特定のクエリに関する詳細な情報を取得できます。

**API形式**

```http
GET /queries/{QUERY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | 取得するクエリの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定されたクエリに関する詳細情報と共にHTTPステータス200を返します。

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

>[!NOTE]
>
>の値を使用して、作成したクエリ `_links.cancel` を [キャンセルできます](#cancel-a-query)。

### クエリのキャンセル

エンドポイントにPATCHリクエストを送信し、要求パスにクエリの `/queries``id` 値を指定することで、指定したクエリを削除するようにリクエストできます。

**API形式**

```http
PATCH /queries/{QUERY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | キャンセルするクエリの `id` 値。 |


**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの機能について詳しくは、APIの基本ドキュメントを参照してください。

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
| `op` | クエリをキャンセルするには、opパラメーターに値を設定する必要があり `cancel `ます。 |

**応答**

応答が成功すると、HTTPステータス202（受け入れ済み）が次のメッセージと共に返されます。

```json
{
    "message": "Query cancel request received",
    "statusCode": 202
}
```
