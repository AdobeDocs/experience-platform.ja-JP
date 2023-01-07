---
keywords: Experience Platform、ホーム、人気の高いトピック、クエリサービス、スケジュール済みクエリの実行、スケジュール済みクエリの実行、クエリサービス、スケジュール済みクエリ、スケジュール済みクエリ、
solution: Experience Platform
title: スケジュール済みクエリ実行 API エンドポイント
description: 以下の節では、クエリサービス API を使用してスケジュールされたクエリを実行するために実行できる様々な API 呼び出しについて説明します。
exl-id: 1e69b467-460a-41ea-900c-00348c3c923c
source-git-commit: 58eadaaf461ecd9598f3f508fab0c192cf058916
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 90%

---

# スケジュール済みクエリ実行エンドポイント

## サンプル API 呼び出し

使用するヘッダーを理解できたので、[!DNL Query Service] API への呼び出しを開始できます。以下の節では、 [!DNL Query Service] API 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### 指定されたスケジュール済みクエリのすべての実行のリストを取得

特定のスケジュール済みクエリのすべての実行のリストは、それらが現在実行されているか、すでに完了しているかに関係なく取得できます。これは、`/schedules/{SCHEDULE_ID}/runs` エンドポイントに対する GET リクエストを介して実行されます。ここで、`{SCHEDULE_ID}` は、実行を取得するスケジュール済みクエリの `id` 値です。

**API 形式**

```http
GET /schedules/{SCHEDULE_ID}/runs
GET /schedules/{SCHEDULE_ID}/runs?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |
| `{QUERY_PARAMETERS}` | （*オプション*）応答に返される結果を設定するリクエストパスに追加されるパラメーター。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。使用できるパラメーターは以下のとおりです。 |

**クエリパラメーター**

次に、指定したスケジュール済みクエリの実行を一覧表示するために使用できるクエリパラメーターのリストを示します。これらのパラメーターはすべてオプションです。パラメーターなしでこのエンドポイントを呼び出すと、指定されたスケジュール済みクエリで使用可能なすべての実行が取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。サポートされているフィールドは `created` と `updated` です。例えば、`orderby=created` は、昇順で結果を並べ替えます。作成前に `-` を追加する（`orderby=-created`）と、項目が作成日の降順で並べ替えられます。 |
| `limit` | ページサイズの制限を指定して、ページに含める結果の数を制御します。（*デフォルト値：20*） |
| `start` | ゼロベースのナンバリングを使用して、応答リストをオフセットします。例えば、`start=2` は、3 番目のクエリから開始するリストを返します。（*デフォルト値：0*） |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。サポートされているフィールドは、`created`、`state`、`externalTrigger` です。サポートされている演算子のリストは、`>`（より大きい）、`<`（より小さい）、`==`（次に等しい）、`!=`（次に等しくない）です。例えば、`externalTrigger==true,state==SUCCESS,created>2019-04-20T13:37:00Z` では、2019 年 4 月 20 日以降に手動で正常に作成されたすべての実行が返されます。 |

**リクエスト**

次のリクエストでは、指定したスケジュール済みクエリの最後の 4 回の実行を取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs?limit=4
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、JSON として指定されたスケジュール済みクエリの実行のリストと共に HTTP ステータス 200 を返します。次の応答では、指定されたスケジュール済みクエリの最後の 4 回の実行を返します。

```json
{
    "runsSchedules": [
        {
            "state": "SUCCESS",
            "version": 1,
            "id": "c2NoZWR1bGVkX18yMDIwLTAxLTA4VDEyOjMwOjAwKzAwOjAw",
            "externalTrigger": "false",
            "created": "2020-01-08T12:30:00Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDEyOjMwOjAwKzAwOjAw",
                    "method": "GET"
                },
                "cancel": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDEyOjMwOjAwKzAwOjAw",
                    "method": "PATCH",
                    "body": "{ \"op\": \"cancel\" }"
                }
            }
        },
        {
            "state": "SUCCESS",
            "version": 1,
            "id": "c2NoZWR1bGVkX18yMDIwLTAxLTA4VDEzOjMwOjAwKzAwOjAw",
            "externalTrigger": "false",
            "created": "2020-01-08T13:30:00Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDEzOjMwOjAwKzAwOjAw",
                    "method": "GET"
                },
                "cancel": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDEzOjMwOjAwKzAwOjAw",
                    "method": "PATCH",
                    "body": "{ \"op\": \"cancel\" }"
                }
            }
        },
        {
            "state": "SUCCESS",
            "version": 1,
            "id": "c2NoZWR1bGVkX18yMDIwLTAxLTA4VDE0OjMwOjAwKzAwOjAw",
            "externalTrigger": "false",
            "created": "2020-01-08T14:30:00Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDE0OjMwOjAwKzAwOjAw",
                    "method": "GET"
                },
                "cancel": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDE0OjMwOjAwKzAwOjAw",
                    "method": "PATCH",
                    "body": "{ \"op\": \"cancel\" }"
                }
            }
        },
        {
            "state": "IN_PROGRESS",
            "version": 1,
            "id": "c2NoZWR1bGVkX18yMDIwLTAxLTA4VDE4OjQ1OjAwKzAwOjAw",
            "externalTrigger": "false",
            "created": "2020-01-08T15:30:00Z",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDE4OjQ1OjAwKzAwOjAw",
                    "method": "GET"
                },
                "cancel": {
                    "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDE4OjQ1OjAwKzAwOjAw",
                    "method": "PATCH",
                    "body": "{ \"op\": \"cancel\" }"
                }
            }
        }
    ],
    "_page": {
        "orderby": "+created",
        "start": "2020-01-08T12:30:00Z",
        "count": 4
    },
    "_links": {},
    "version": 1
}
```

>[!NOTE]
>
> `_links.cancel` の値を使用して、[指定したスケジュール済みクエリの実行を停止](#immediately-stop-a-run-for-a-specific-scheduled-query)できます。

### 特定のスケジュール済みクエリ実行の即時トリガー

`/schedules/{SCHEDULE_ID}/runs` エンドポイントに POST リクエストを送信することで、指定したスケジュール済みクエリに対する実行を直ちにトリガーできます。ここで、`{SCHEDULE_ID}` は、実行をトリガーするスケジュール済みクエリの `id` 値です。

**API 形式**

```http
POST /schedules/{SCHEDULE_ID}/runs
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、次のメッセージと共に HTTP ステータス 202（受理）を返します。

```json
{
    "message": "Request to trigger run of a scheduled query accepted.",
    "statusCode": 202
}
```

### 特定のスケジュール済みクエリ実行の詳細の取得

特定のスケジュール済みクエリの実行に関する詳細を取得するには、`/schedules/{SCHEDULE_ID}/runs/{RUN_ID}` エンドポイントに対して GET リクエストを実行して、リクエストパスでスケジュール済みクエリの ID と実行の両方を指定します。 

**API 形式**

```http
GET /schedules/{SCHEDULE_ID}/runs/{RUN_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 実行詳細を取得するスケジュール済みクエリの `id` 値。 |
| `{RUN_ID}` | 取得する実行の `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定された実行の詳細と共に HTTP ステータス 200 を返します。

```json
{
    "state": "success",
    "taskStatusList": [
        {
            "duration": 303,
            "endDate": "2020-01-08T23:49:02.346318",
            "state": "SUCCESS",
            "message": "Processed Successfully",
            "startDate": "2020-01-08T23:43:58.936269",
            "taskId": "7Omob151BM"
        }
    ],
    "version": 1,
    "id": "c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw",
    "scheduleId": "e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm",
    "externalTrigger": "false",
    "created": "2020-01-08T20:45:00",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw",
            "method": "GET"
        },
        "cancel": {
            "href": "https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw",
            "method": "PATCH",
            "body": "{ \"op\": \"cancel\" }"
        }
    }
}
```

### 特定のスケジュール済みクエリ実行の即時停止

特定のスケジュールされたクエリに対する実行を直ちに停止するには、`/schedules/{SCHEDULE_ID}/runs/{RUN_ID}` エンドポイントに対して PATCH リクエストを送信し、リクエストパスでスケジュール済みクエリの ID と実行の両方を指定します。

**API 形式**

```http
PATCH /schedules/{SCHEDULE_ID}/runs/{RUN_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 実行詳細を取得するスケジュール済みクエリの `id` 値。 |
| `{RUN_ID}` | 取得する実行の `id` 値。 |

**リクエスト**

この API リクエストは、ペイロードに JSON パッチ構文を使用します。JSON パッチの仕組みについて詳しくは、API の基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
     "op": "cancel"
 }'
```

**応答**

成功応答は、HTTP ステータス 202（許可済み）とともに次のメッセージを返します。

```json
{
    "message": "Request to cancel run of a scheduled query accepted",
    "statusCode": 202
}
```
