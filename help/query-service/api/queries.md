---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；API ガイド；クエリ；クエリ；クエリサービス；
solution: Experience Platform
title: クエリ API エンドポイント
description: 以降の節では、クエリサービス API の/querys エンドポイントを使用しておこなう呼び出しについて説明します。
exl-id: d6273e82-ce9d-4132-8f2b-f376c6712882
source-git-commit: e0287076cc9f1a843d6e3f107359263cd98651e6
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 73%

---

# クエリエンドポイント

## サンプル API 呼び出し

以降の節では、 `/queries` エンドポイント [!DNL Query Service] API 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### クエリのリストの取得

IMS 組織に対するすべてのクエリのリストを取得するには、`/queries` エンドポイントへの GET リクエストをおこないます。

**API 形式**

```http
GET /queries
GET /queries?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`：（*オプション*）応答に返される結果を設定するリクエストパスに追加されるパラメーター。複数のパラメーターを含め、アンパサンド（`&`）で区切ることができます。使用できるパラメーターは以下のとおりです。

**クエリパラメータ**

次に、クエリの一覧表示に使用可能なクエリパラメーターのリストを示します。これらのパラメーターはすべてオプションです。パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。サポートされているフィールドは `created` と `updated` です。例えば、`orderby=created` は、昇順で結果を並べ替えます。作成前に `-` を追加する（`orderby=-created`）と、項目が作成日の降順で並べ替えられます。 |
| `limit` | ページサイズの制限を指定して、ページに含める結果の数を制御します。（*デフォルト値：20*） |
| `start` | ゼロベースのナンバリングを使用して、応答リストをオフセットします。例えば、`start=2` は、3 番目のクエリから開始するリストを返します。（*デフォルト値：0*） |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。サポートされているフィルターは `created`、`updated`、`state`、および `id` です。サポートされる演算子のリストは、`>`（次より大きい）、`<`（次より小さい）、`>=`（次よりも大きいか等しい）、`<=`（次よりも小さいか等しい）、`==`（等しい）、`!=`（次と等しくない）、`~`（次を含む）です。例えば、`id==6ebd9c2d-494d-425a-aa91-24033f3abeec` は指定した ID を持つすべてのクエリを返します。 |
| `excludeSoftDeleted` | ソフト削除されたクエリを含める必要があるかどうかを示します。例えば、`excludeSoftDeleted=false` はソフト削除クエリを&#x200B;**含みます**。（*ブール値、デフォルト値：true*） |
| `excludeHidden` | ユーザー主導でないクエリを表示するかどうかを示します。この値を false に設定すると、CURSOR 定義、FETCH、メタデータクエリなど、ユーザ主導でないクエリが&#x200B;**含まれます**。（*ブール値、デフォルト値：true*） |
| `isPrevLink` | この `isPrevLink` ページネーションにはクエリパラメーターが使用されます。 API 呼び出しの結果は、 `created` タイムスタンプと `orderby` プロパティ。 結果のページを移動する場合、 `isPrevLink` が true に設定されている場合、逆方向にページングします。 クエリの順序を逆にします。 例として、「次へ」および「前へ」リンクを参照してください。 |

**リクエスト**

次のリクエストで、IMS 組織用に作成された最新のクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/queries?limit=1 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、HTTP ステータス 200 とともに、JSON として指定した IMS 組織のクエリのリストを返します。次の応答は、IMS 組織用に作成された最新のクエリを返します。

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

`/queries` エンドポイントに POST リクエストをおこなうことで、新しいクエリを作成できます。

**API 形式**

```http
POST /queries
```

**リクエスト**

次のリクエストは、ペイロードで指定された SQL ステートメントを使用して、新しいクエリを作成します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/queries \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
        "dbName": "prod:all",
        "sql": "SELECT account_balance FROM user_data WHERE $user_id;",
        "queryParameters": {
            $user_id : {USER_ID}
            }
        "name": "Sample Query",
        "description": "Sample Description"
    }  
```

次のリクエストの例では、既存のクエリテンプレート ID を使用して新しいクエリを作成します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/queries \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
        "dbName": "prod:all",
        "templateID": "f7cb5155-29da-4b95-8131-8c5deadfbe7f",
        "name": "Sample Query",
        "description": "Sample Description"
    }  
```

| プロパティ | 説明 |
| -------- | ----------- |
| `dbName` | SQL データベースを作成する対象のクエリの名前。 |
| `sql` | 作成する SQL クエリ。 |
| `name` | SQL クエリの名前。 |
| `description` | SQL クエリの説明。 |
| `queryParameters` | SQL 文内のパラメーター化された値を置き換えるキー値の組み合わせ。 必要なのは **if** 指定した SQL 内でパラメータ置換を使用しています。 これらのキー値ペアに対しては、値タイプのチェックはおこなわれません。 |
| `templateId` | 既存のクエリの一意の ID。 SQL 文の代わりにこの文を指定できます。 |
| `insertIntoParameters` | （オプション）このプロパティが定義されている場合、このクエリは INSERT INTO クエリに変換されます。 |
| `ctasParameters` | （オプション）このプロパティが定義されている場合、このクエリは CTAS クエリに変換されます。 |

**応答**

成功応答は HTTP ステータス（許可済み）とともに、作成したクエリの詳細を返します。クエリのアクティブ化が完了し、正常に実行されたら、`state` が `SUBMITTED` から `SUCCESS` に変更されます。

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
> `_links.cancel` の値を使用して、[作成したクエリをキャンセル](#cancel-a-query)できます。

### ID によるクエリの取得

特定のクエリに関する詳細な情報を取得するには、`/queries` エンドポイントに GET リクエストを送信し、リクエストパスにクエリの `id` の値を指定します。

**API 形式**

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
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、HTTP ステータス 200 とともに、指定された応答に関する詳細な情報を返します。

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
> `_links.cancel` の値を使用して、[作成したクエリをキャンセル](#cancel-a-query)できます。

### クエリのキャンセル

`/queries` エンドポイントに PATCH リクエストを送信し、リクエストパスでクエリの `id` 値を指定することで、指定したクエリを削除するようリクエストできます。

**API 形式**

```http
PATCH /queries/{QUERY_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | キャンセルするクエリの `id` 値。 |


**リクエスト**

この API リクエストは、ペイロードに JSON パッチ構文を使用します。JSON パッチの仕組みについて詳しくは、API の基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/queries/4d64cd49-cf8f-463a-a182-54bccb9954fc \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json',
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
   "op": "cancel"  
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `op` | クエリをキャンセルするには、op パラメーターに値 `cancel ` を設定する必要があります。 |

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Query cancel request received",
    "statusCode": 202
}
```
