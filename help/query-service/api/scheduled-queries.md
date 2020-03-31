---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: scheduled queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# 予定クエリ

## サンプルAPI呼び出し

これで、使用するヘッダーを理解できたので、クエリサービスAPIの呼び出しを開始できます。 以下の節では、クエリサービスAPIを使用して行える様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプル応答が含まれます。

### スケジュールされたリストの取得クエリ

エンドポイントにGET要求を行うことで、IMS組織のすべてのスケジュール済みクエリのリストを取得で `/schedules` きます。

**API形式**

```http
GET /schedules
GET /schedules?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{QUERY_PARAMETERS}` | (オ&#x200B;*プション*)応答に返される結果を設定する要求パスに追加されるパラメーター。 複数のパラメーターを含め、アンパサンド(`&`)で区切ることができます。 使用可能なパラメーターを以下に示します。 |

**クエリパラメータ**

次に、スケジュールされたリストを一覧表示するために使用できるクエリパラメータのクエリを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュール済みクエリが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果を並べ替えるフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、は、 `orderby=created` 昇順で結果を並べ替えます。 「作成前( `-` )」を追加す`orderby=-created`ると、アイテムが作成された降順で並べ替えられます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。 (*Default value: 20*) |
| `start` | ゼロベースの番号付けを使用して、応答リストをオフセットします。 例えば、3番目 `start=2` のリストのリストからクエリを返します。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** HTMLエスケープする必要があります。 複数のコンマを使用して、複数のフィルターを組み合わせます。 サポートされているフィ `created`ールドは、、 `templateId`およびで `userId`す。 サポートされる演 `>` 算子のリストは、 `<` （より大きい）、（より小さい）お `==` よび（等しい）です。 例えば、は、指定さ `userId==6ebd9c2d-494d-425a-aa91-24033f3abeec` れたユーザーIDを持つスケジュール済みクエリをすべて返します。 |

**リクエスト**

次のリクエストでは、IMS組織用に作成された最新のスケジュール済みクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したIMS組織に対してスケジュールされたクエリのリストを含むHTTPステータス200を返します。 次の応答は、IMS組織用に作成された最新のスケジュールクエリを返します。

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

### 新しいスケジュール済みクエリ

エンドポイントにPOSTリクエストを行うことで、新しいスケジュール済みクエリを作成することが `/schedules` できます。

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
| `query.dbName` | スケジュールされたデータベースを作成するクエリの名前。 |
| `query.sql` | 作成するSQLクエリ。 |
| `query.name` | スケジュールされたクエリの名前。 |
| `schedule.schedule` | クエリのCronスケジュール。 cronスケジュールの詳細については、Cron式形式のドキュメントを [参照し](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 、 この例では、「30 * * * *」は、クエリが毎時30分の時点で実行されることを意味します。 |
| `schedule.startDate` | スケジュールされた開始のクエリ日で、UTCタイムスタンプで書き込まれます。 |

**応答**

正常に応答すると、新しく作成されたスケジュール済みクエリの詳細と共に、HTTPステータス202（受け入れ済み）が返されます。 スケジュールされたクエリのアクティブ化が完了す `state` ると、がからに変 `REGISTERING` 更されま `ENABLED`す。

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

>[!NOTE] の値を使用して、作成したスケジ `_links.delete` ュー [ル済みクエリを削除できま](#delete-a-specified-scheduled-query)す。

### 指定したスケジュール済みクエリの詳細

特定のスケジュールされたクエリの情報を取得するには、エンドポイントにGETリクエストを送信し、そのIDを `/schedules` リクエストパスに指定します。

**API形式**

```http
GET /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得す `id` るスケジュール済みクエリの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定されたスケジュール済みの応答の詳細を含むHTTPステータス200を返します。クエリ

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

>[!NOTE] の値を使用して、作成したスケジ `_links.delete` ュー [ル済みクエリを削除できま](#delete-a-specified-scheduled-query)す。

### 指定したスケジュール済みクエリの詳細

エンドポイントにPATCHリクエストを行い、リクエストパスにIDを指定することで、指定したスケジュール済み `/schedules` クエリの詳細を更新することができます。

PATCHリクエストは、次の2つの異なるパスをサポートします。 `/state` と `/schedule/schedule`

### スケジュールされたクエリ状態の更新

を使用して、選択し `/state` たスケジュール済みクエリの状態（ENABLEDまたはDISABLED）を更新できます。 状態を更新するには、値をまたはに設定する必要があ `enable` ります `disable`。

**API形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得す `id` るスケジュール済みクエリの値。 |


**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの仕組みについて詳しくは、APIの基本ドキュメントを参照してください。

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールされたクエリの状態を更新するので、の値をに設定する必要があ `path` ります `/state`。 |
| `value` | の更新された値 `/state`。 この値は、スケジュールされたクエリを有効 `enable` または `disable` 無効にするために設定できます。 |

**応答**

成功した応答は、次のメッセージと共にHTTPステータス202（受け入れ済み）を返します。

```json
{
    "message": "Request to patch accepted",
    "statusCode": 202
}
```

### スケジュールされたクエリスケジュールの更新

を使用して、スケジ `/schedule/schedule` ュールされたクエリのCronスケジュールを更新できます。 cronスケジュールの詳細については、Cron式形式のドキュメントを [参照し](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 、

**API形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得す `id` るスケジュール済みクエリの値。 |

**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの仕組みについて詳しくは、APIの基本ドキュメントを参照してください。

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールされたクエリのスケジュールを更新するので、の値をに設定する必要があ `path` りま `/schedule/schedule`す。 |
| `value` | の更新された値 `/schedule`。 この値は、Cronスケジュールの形式で指定する必要があります。 この例では、スケジュールされたクエリは毎時45分に実行されます。 |

**応答**

成功した応答は、次のメッセージと共にHTTPステータス202（受け入れ済み）を返します。

```json
{
    "message": "Request to patch accepted",
    "statusCode": 202
}
```

### 指定したスケジュール済みクエリの削除

エンドポイントにDELETEリクエストを行い、削除するスケジュール済みクエリのIDをリクエストパス `/schedules` に指定することで、指定したスケジュール済みクエリを削除できます。

>[!NOTE] スケジュールを **削除する前に** 、スケジュールを無効にする必要があります。

**API形式**

```http
DELETE /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得す `id` るスケジュール済みクエリの値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、次のメッセージと共にHTTPステータス202（受け入れ済み）を返します。

```json
{
    "message": "Schedule deleted successfully",
    "statusCode": 202
}
```
