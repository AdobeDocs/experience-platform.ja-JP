---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: scheduled queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 3%

---


# 予定クエリ

## サンプルAPI呼び出し

これで、使用するヘッダーが分かったので、クエリサービスAPIの呼び出しを開始する準備が整いました。 以下の節では、クエリサービスAPIを使用して実行できる様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

### スケジュールされたクエリのリストの取得

エンドポイントにGETリクエストを行うと、IMS組織でスケジュールされたすべてのクエリのリストを取得でき `/schedules` ます。

**API形式**

```http
GET /schedules
GET /schedules?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | (*オプション*)リクエストパスに追加されるパラメーター。応答で返される結果を設定します。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。 使用可能なパラメーターを以下に示します。 |

**クエリパラメーター**

次に、スケジュールされたクエリのリストに使用できるクエリパラメーターのリストを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュール済みクエリが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、 `orderby=created` は、作成された結果を昇順で並べ替えます。 作成 `-` 前(`orderby=-created`)を追加すると、作成されたアイテムを降順で並べ替えます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズ制限を指定します。 (*Default value: 20*) |
| `start` | 0から始まる番号を使用して、応答のリストをオフセットします。 例えば、3番目 `start=2` にリストされたクエリから開始するリストが返されます。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** 、HTMLエスケープする必要があります。 複数のフィルターセットを組み合わせる場合は、コンマを使用します。 サポートされているフィールドは、 `created`、 `templateId`および `userId`です。 サポートされる演算子は、 `>` （より大きい）、 `<` （より小さい）、 `==` （等しい）のリストです。 例えば、 `userId==6ebd9c2d-494d-425a-aa91-24033f3abeec` は、ユーザーIDが指定されたとおりである、スケジュールされたすべてのクエリを返します。 |

**リクエスト**

次のリクエストは、IMS組織用に作成された最新のスケジュール済みクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定したIMS組織に対してスケジュールされたクエリのリストを含むHTTPステータス200を返します。 次の応答は、IMS組織用に作成された最新のスケジュール済みクエリを返します。

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

### 新しい予定クエリの作成

エンドポイントにPOSTリクエストを作成して、新しいスケジュール済みクエリを作成でき `/schedules` ます。

**API形式**

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
| `query.dbName` | スケジュールクエリを作成するデータベースの名前。 |
| `query.sql` | 作成するSQLクエリ。 |
| `query.name` | スケジュールされたクエリの名前。 |
| `schedule.schedule` | クエリのcronスケジュール。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 この例で「30 * * * *」は、クエリが30分の時点で毎時実行されることを意味します。 |
| `schedule.startDate` | スケジュールされたクエリの開始日。UTCタイムスタンプで書き込まれます。 |

**応答**

正常に応答すると、HTTPステータス202（「受け入れ済み」）が返され、新たに作成されたスケジュール済みクエリの詳細が返されます。 スケジュールされたクエリのアクティブ化が完了する `state` と、の値がからに変更 `REGISTERING` され `ENABLED`ます。

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

>[!NOTE] の値を使用して、作成したスケジュール済みクエリ `_links.delete` を [削除できます](#delete-a-specified-scheduled-query)。

### 指定したスケジュール済みクエリの詳細の要求

エンドポイントにGETリクエストを行い、そのIDをリクエストパスに指定することで、特定のスケジュール済みクエリの情報を取得でき `/schedules` ます。

**API形式**

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

正常に応答すると、指定したスケジュール済みクエリの詳細と共にHTTPステータス200が返されます。

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

>[!NOTE] の値を使用して、作成したスケジュール済みクエリ `_links.delete` を [削除できます](#delete-a-specified-scheduled-query)。

### 指定したスケジュール済みクエリの詳細の更新

エンドポイントにPATCHリクエストを行い、そのIDをリクエストパスに指定することで、指定したスケジュール済みクエリの詳細を更新でき `/schedules` ます。

PATCHリクエストは、次の2つの異なるパスをサポートします。 `/state` と `/schedule/schedule`。

### スケジュールされたクエリ状態の更新

を使用して、選択したスケジュール済みクエリの状態(「有効」(ENABLED)または「無効」(DISABLED))を更新できます。 `/state` 状態を更新するには、値をまたはに設定する必要があり `enable` ま `disable`す。

**API形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |


**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの機能について詳しくは、APIの基本ドキュメントを参照してください。

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールされたクエリの状態を更新するので、の値をに設定する必要 `path` があり `/state`ます。 |
| `value` | の更新された値 `/state`。 この値は、スケジュールされたクエリを有効または無効にす `enable` るた `disable` めに、または有効に設定できます。 |

**応答**

応答が成功すると、次のメッセージと共にHTTPステータス202（受け入れ済み）が返されます。

```json
{
    "message": "Request to patch accepted",
    "statusCode": 202
}
```

### スケジュールされたクエリスケジュールの更新

を使用して、スケジュールさ `/schedule/schedule` れたクエリのCronスケジュールを更新できます。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。

**API形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |

**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの機能について詳しくは、APIの基本ドキュメントを参照してください。

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールされたクエリのスケジュールを更新するので、の値をに設定する必要 `path` があり `/schedule/schedule`ます。 |
| `value` | の更新された値 `/schedule`。 この値は、Cronスケジュールの形式にする必要があります。 この例では、スケジュールされたクエリは45分の時点で毎時実行されます。 |

**応答**

応答が成功すると、次のメッセージと共にHTTPステータス202（受け入れ済み）が返されます。

```json
{
    "message": "Request to patch accepted",
    "statusCode": 202
}
```

### 指定したスケジュール済みクエリの削除

エンドポイントにDELETEリクエストを送信し、削除する予定クエリのIDをリクエストパスに指定することで、指定した予定クエリを削除でき `/schedules` ます。

>[!NOTE] スケジュールを削除する前に **** 、無効にする必要があります。

**API形式**

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

応答が成功すると、次のメッセージと共にHTTPステータス202（受け入れ済み）が返されます。

```json
{
    "message": "Schedule deleted successfully",
    "statusCode": 202
}
```
