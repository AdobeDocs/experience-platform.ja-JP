---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: scheduled queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 96%

---


# スケジュール済みクエリ

## サンプル API 呼び出し

Now that you understand what headers to use, you are ready to begin making calls to the [!DNL Query Service] API. The following sections walk through the various API calls you can make using the [!DNL Query Service] API. 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### スケジュールされたクエリのリストの取得

IMS 組織に対してスケジュールされたすべてのクエリのリストを取得するには、`/schedules` エンドポイントへの GET リクエストをおこないます。

**API 形式**

```http
GET /schedules
GET /schedules?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | （*オプション*）リクエストパスに追加されるパラメーター。このリクエストパスは応答で返される結果を設定します。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。使用できるパラメーターは以下のとおりです。 |

**クエリパラメータ**

次に、スケジュールされたクエリの一覧表示に使用可能なクエリパラメーターのリストを示します。これらのパラメーターはすべてオプションです。パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用できる、スケジュールされたクエリがすべて取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。サポートされているフィールドは `created` と `updated` です。例えば、`orderby=created` は、昇順で結果を並べ替えます。作成前に `-` を追加する（`orderby=-created`）と、項目が作成日の降順で並べ替えられます。 |
| `limit` | ページサイズの制限を指定して、ページに含める結果の数を制御します。（*デフォルト値：20*） |
| `start` | ゼロベースのナンバリングを使用して、応答リストをオフセットします。例えば、`start=2` は、3 番目のクエリから開始するリストを返します。（*デフォルト値：0*） |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。サポートされるフィールドは `created`、`templateId`、および `userId` です。サポートされる演算子のリストは `>`（より大きい）、`<`（より小さい）、および `==`（等しい）です。例えば、`userId==6ebd9c2d-494d-425a-aa91-24033f3abeec` は、指定されたものと同じたユーザー ID を持つスケジュール済みクエリをすべて返します。 |

**リクエスト**

次のリクエストは、IMS 組織用に作成され、最新のスケジュール済みクエリを返します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、HTTP ステータス 200 とともに、指定した IMS 組織に対する、スケジュール済みクエリのリストを返します。次の応答は、IMS 組織用に作成され、最新のスケジュール済みクエリを返します。

```json
{
    "schedules": [
        {
            "state": "ENABLED",
            "query": {
                "dbName": "prod:all",
                "sql": "SELECT * FROM accounts;",
                "name": "Sample Scheduled Query",
                "description": "A sample of a scheduled query."
            },
            "updatedUserId": "{USER_ID}",
            "version": 2,
            "id": "e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "updated": "1578523458919",
            "schedule": {
                "schedule": "30 * * * *",
                "startDate": "2020-01-08T12:30:00.000Z",
                "maxActiveRuns": 1
            },
            "userId": "{USER_ID}",
            "created": "1578523458919",
            "_links": {
                "enable": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
                    "method": "PATCH",
                    "body": "{ \"op\": \"enable\" }"
                },
                "runs": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs",
                    "method": "GET"
                },
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
                    "method": "DELETE"
                },
                "disable": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
                    "method": "PATCH",
                    "body": "{ \"op\": \"disable\" }"
                },
                "trigger": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs",
                    "method": "POST"
                }
            }
        }
    ],
    "_page": {
        "orderby": "+created",
        "start": "2020-01-08T22:44:18.919Z",
        "count": 1
    },
    "_links": {},
    "version": 2
}
```

### 新しいスケジュール済みクエリの作成

`/schedules` エンドポイントに POST リクエストを送信することで、新しいスケジュール済みクエリを作成できます。

**API 形式**

```http
POST /schedules
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/schedules
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
 {
     "query": {
         "dbName": "prod:all",
         "sql": "SELECT * FROM accounts;",
         "name": "Sample Scheduled Query",
         "description": "A sample of a scheduled query."
     }, 
     "schedule": {
         "schedule": "30 * * * *",
         "startDate": "2020-01-08T12:30:00.000Z"
     }
 }
 '
```

| プロパティ | 説明 |
| -------- | ----------- |
| `query.dbName` | スケジュール済みクエリを作成するデータベースの名前。 |
| `query.sql` | 作成する SQL クエリ。 |
| `query.name` | スケジュール済みクエリの名前。 |
| `schedule.schedule` | クエリの cron スケジュール。Cron スケジュールの詳細については、[cron 式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「30 * * * *」は、クエリが毎時 30 分に実行されることを意味します。 |
| `schedule.startDate` | スケジュール済みクエリの開始日（UTC タイムスタンプ形式）です。 |

**応答**

成功応答は HTTP ステータス（許可済み）とともに、作成したスケジュール済みクエリの詳細を返します。スケジュール済みクエリのアクティブ化が完了すると、`state` は `REGISTERING` から `ENABLED` に変更されます。

```json
{
    "state": "REGISTERING",
    "query": {
        "dbName": "prod:all",
        "sql": "SELECT * FROM accounts;",
        "name": "Sample Scheduled Query",
        "description": "A sample of a scheduled query."
    },
    "updatedUserId": "{USER_ID}",
    "version": 2,
    "id": "e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
    "schedule": {
        "schedule": "30 * * * *",
        "startDate": "2020-01-08T12:30:00.000Z",
        "maxActiveRuns": 1
    },
    "userId": "{USER_ID}",
    "_links": {
        "enable": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "PATCH",
            "body": "{ \"op\": \"enable\" }"
        },
        "runs": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs",
            "method": "GET"
        },
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "DELETE"
        },
        "disable": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "PATCH",
            "body": "{ \"op\": \"disable\" }"
        },
        "trigger": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs",
            "method": "POST"
        }
    }
}
```

>[!NOTE]
>
> `_links.delete` の値を使用して、[作成したスケジュール済みクエリを削除](#delete-a-specified-scheduled-query)できます。

### 指定したスケジュール済みクエリの詳細のリクエスト

特定のスケジュール済みクエリの情報を取得するには、`/schedules` エンドポイントに GET リクエストを送信し、リクエストパスでその ID を指定します。

**API 形式**

```http
GET /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、HTTP ステータス 200 とともに、指定されたスケジュール済みクエリの詳細を返します。

```json
{
    "state": "ENABLED",
    "query": {
        "dbName": "prod:all",
        "sql": "SELECT * FROM accounts;",
        "name": "Sample Scheduled Query",
        "description": "A sample of a scheduled query."
    },
    "updatedUserId": "{USER_ID}",
    "version": 2,
    "id": "e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
    "updated": "1578523458919",
    "schedule": {
        "schedule": "30 * * * *",
        "startDate": "2020-01-08T12:30:00.000Z",
        "maxActiveRuns": 1
    },
    "userId": "{USER_ID}",
    "created": "1578523458919",
    "_links": {
        "enable": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "PATCH",
            "body": "{ \"op\": \"enable\" }"
        },
        "runs": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs",
            "method": "GET"
        },
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "DELETE"
        },
        "disable": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
            "method": "PATCH",
            "body": "{ \"op\": \"disable\" }"
        },
        "trigger": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs",
            "method": "POST"
        }
    }
}
```

>[!NOTE]
>
> `_links.delete` の値を使用して、[作成したスケジュール済みクエリを削除](#delete-a-specified-scheduled-query)できます。

### 指定したスケジュール済みクエリの詳細の更新

指定したスケジュール済みクエリの詳細を更新するには、`/schedules` エンドポイントに PATCH リクエストを送信し、リクエストパスでその ID を指定します。

PATCH リクエストは、`/state` と `/schedule/schedule` の 2 つのパスをサポートします。

### スケジュール済みクエリの状態の更新

`/state` を使用して、選択したスケジュール済みクエリの状態（ENABLED または DISABLED）を更新できます。状態を更新するには、値を `enable` または `disable` に設定する必要があります。

**API 形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |


**リクエスト**

この API リクエストは、ペイロードに JSON パッチ構文を使用します。JSON パッチの仕組みについて詳しくは、API の基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
     "body": [
         {
             "op": "replace",
             "path": "/state",
             "value": "disable"
         }
     ]
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `path` | パッチを適用する値のパス。この場合、スケジュール済みクエリの状態を更新することになるので、`path` の値を `/state` に設定する必要があります。 |
| `value` | `/state` の更新された値。この値は、`enable` または `disable` に設定して、スケジュール済みクエリを有効または無効にすることができます。 |

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Request to patch accepted",
    "statusCode": 202
}
```

### スケジュール済みクエリスケジュールの更新

`/schedule/schedule`を使用して、スケジュールされたクエリの cron スケジュールを更新できます。cron スケジュールの詳細については、[cron 式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。

**API 形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |

**リクエスト**

この API リクエストは、ペイロードに JSON パッチ構文を使用します。JSON パッチの仕組みについて詳しくは、API の基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
     "body": [
         {
             "op": "replace",
             "path": "/schedule/schedule",
             "value": "45 * * * *"
         }
     ]
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `path` | パッチを適用する値のパス。この場合、スケジュール済みクエリのスケジュールを更新するので、`path` の値を `/schedule/schedule` に設定する必要があります。 |
| `value` | `/schedule` の更新された値。この値は、cron スケジュールの形式で指定する必要があります。この例では、スケジュールされたクエリは毎時 45 分に実行されます。 |

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Request to patch accepted",
    "statusCode": 202
}
```

### 指定したスケジュール済みクエリの削除

`/schedules` エンドポイントに DELETE リクエストを送信し、リクエストパスで削除するスケジュール済みクエリの ID を指定することで、指定したスケジュール済みクエリを削除できます。

>[!NOTE]
>
> スケジュールを削除する前に、無効にする&#x200B;**必要があります**。

**API 形式**

```http
DELETE /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Schedule deleted successfully",
    "statusCode": 202
}
```
