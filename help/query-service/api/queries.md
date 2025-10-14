---
keywords: Experience Platform；ホーム；人気のトピック；query service;api ガイド；クエリ；クエリ；Query service;
solution: Experience Platform
title: クエリ API エンドポイント
description: 次の節では、Query Service API の/queries エンドポイントを使用して実行できる呼び出しについて説明します。
role: Developer
exl-id: d6273e82-ce9d-4132-8f2b-f376c6712882
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '950'
ht-degree: 48%

---

# クエリエンドポイント

## サンプル API 呼び出し

次の節では、[!DNL Query Service] API の `/queries` エンドポイントを使用して作成できる呼び出しについて説明します。 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### クエリのリストの取得

`/queries` エンドポイントに対してGETリクエストを行うことで、組織のすべてのクエリのリストを取得できます。

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
| `start` | 結果を並べ替える ISO 形式タイムスタンプを指定します。 開始日を指定しない場合、API 呼び出しでは、最も古く作成されたクエリが最初に返され、次に、より新しい結果が引き続きリストされます。ISO タイムスタンプ <br> 使用すると、日付と時間に様々なレベルの精度を設定できます。 基本の ISO タイムスタンプは、2020 年 9 月 7 日を表すために `2020-09-07` の形式を取ります。 より複雑な例は、`2022-11-05T08:15:30-05:00` と記述され、2022 年 11 月 5 日午前 8:15:30 分（米国東部標準時）に対応します。 タイムゾーンには UTC オフセットを指定でき、サフィックス「Z」（`2020-01-01T01:01:01Z`）で示されます。 タイムゾーンが指定されない場合は、デフォルトで 0 に設定されます。 |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。サポートされているフィルターは `created`、`updated`、`state`、および `id` です。サポートされる演算子のリストは、`>`（次より大きい）、`<`（次より小さい）、`>=`（次よりも大きいか等しい）、`<=`（次よりも小さいか等しい）、`==`（等しい）、`!=`（次と等しくない）、`~`（次を含む）です。例えば、`id==6ebd9c2d-494d-425a-aa91-24033f3abeec` は指定した ID を持つすべてのクエリを返します。 |
| `excludeSoftDeleted` | ソフト削除されたクエリを含める必要があるかどうかを示します。例えば、`excludeSoftDeleted=false` はソフト削除クエリを&#x200B;**含みます**。（*ブール値、デフォルト値：true*） |
| `excludeHidden` | ユーザー主導でないクエリを表示するかどうかを示します。この値を false に設定すると、CURSOR 定義、FETCH、メタデータクエリなど、ユーザ主導でないクエリが&#x200B;**含まれます**。（*ブール値、デフォルト値：true*） |
| `isPrevLink` | `isPrevLink` クエリパラメーターは、ページネーションに使用されます。 API 呼び出しの結果は、`created` タイムスタンプと `orderby` プロパティを使用して並べ替えられます。 結果のページを移動する場合、後方にページングすると、`isPrevLink` は true に設定されます。 クエリの順序を逆にします。 例として、「次へ」リンクと「前へ」リンクを参照してください。 |

**リクエスト**

次のリクエストでは、組織に対して作成された最新のクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/queries?limit=1 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答に成功すると、HTTP ステータス 200 が、指定した組織のクエリのリストと共に JSON として返されます。 次の応答は、組織に対して作成された最新のクエリを返します。

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

次のリクエストは、ペイロードで提供された SQL 文を含む新しいクエリを作成します。

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/queries \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
        "dbName": "prod:all",
        "sql": "SELECT account_balance FROM user_data WHERE user_id='$user_id';",
        "queryParameters": {
            user_id : {USER_ID}
            }
        "name": "Sample Query",
        "description": "Sample Description"
    }  
```

以下のリクエストの例では、既存のクエリテンプレート ID を使用して新しいクエリを作成します。

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
| `queryParameters` | SQL 文内のパラメータ化された値を置き換えるためのキー値のペア。 指定する SQL 内でパラメーター置換を使用する場合 **のみ必須です** 使用しない場合）。 これらのキーと値のペアについては、値タイプのチェックは行われません。 |
| `templateId` | 既存のクエリの一意の ID。 SQL 文の代わりにこれを指定できます。 |
| `insertIntoParameters` | （オプション）このプロパティを定義すると、このクエリは INSERT INTO クエリに変換されます。 |
| `ctasParameters` | （オプション）このプロパティを定義すると、このクエリは CTAS クエリに変換されます。 |

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
>`_links.cancel` の値を使用して [&#x200B; 作成したクエリをキャンセル &#x200B;](#cancel-a-query) できます。

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
>`_links.cancel` の値を使用して [&#x200B; 作成したクエリをキャンセル &#x200B;](#cancel-a-query) できます。

### クエリのキャンセルまたはソフト削除

`/queries` エンドポイントに対してPATCHリクエストを実行し、リクエストパスにクエリの `id` 値を指定することで、指定したクエリのキャンセルまたはソフト削除をリクエストできます。

**API 形式**

```http
PATCH /queries/{QUERY_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | 操作を実行するクエリの `id` 値。 |


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
| `op` | リソースに対して実行する操作のタイプ。 指定できる値は、`cancel` および `soft_delete` です。クエリをキャンセルするには、op パラメーターに値 `cancel ` を設定する必要があります。 ソフト削除処理は、GETリクエストでクエリが返されるのを停止しますが、システムからクエリが削除されるわけではありません。 |

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Query cancel request received",
    "statusCode": 202
}
```
