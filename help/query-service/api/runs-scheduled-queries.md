---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: runs for scheduled queries
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 2%

---


# スケジュールされたクエリに対して実行

## サンプルAPI呼び出し

これで、使用するヘッダーが分かったので、クエリサービスAPIの呼び出しを開始する準備が整いました。 以下の節では、クエリサービスAPIを使用して実行できる様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

### 指定したスケジュール済みクエリのすべての実行のリストを取得します

現在実行中か、既に完了しているかに関係なく、特定のスケジュールされたクエリに対するすべての実行のリストを取得できます。 これは、エンドポイントにGETリクエストを行うことで行われます。ここ `/schedules/{SCHEDULE_ID}/runs` で、は、実行を取得する予定のスケジュール済みクエリの `{SCHEDULE_ID}``id` 値です。

**API形式**

```http
GET /schedules/{SCHEDULE_ID}/runs
GET /schedules/{SCHEDULE_ID}/runs?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュール済みクエリの `id` 値。 |
| `{QUERY_PARAMETERS}` | (*オプション*)リクエストパスに追加されるパラメーター。応答で返される結果を設定します。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。 使用可能なパラメーターを以下に示します。 |

**クエリパラメーター**

次のリストは、指定したスケジュール済みクエリの実行をリストするために使用できるクエリパラメーターの一覧です。 これらのパラメーターはすべてオプションです。 パラメーターを指定せずにこのエンドポイントを呼び出すと、指定したスケジュール済みクエリで使用可能なすべての実行が取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の並べ替えに使用するフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、 `orderby=created` は、作成された結果を昇順で並べ替えます。 作成 `-` 前(`orderby=-created`)を追加すると、作成されたアイテムを降順で並べ替えます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズ制限を指定します。 (*Default value: 20*) |
| `start` | 0から始まる番号を使用して、応答のリストをオフセットします。 例えば、3番目 `start=2` にリストされたクエリから開始するリストが返されます。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** 、HTMLエスケープする必要があります。 複数のフィルターセットを組み合わせる場合は、コンマを使用します。 サポートされているフィールドは、 `created`、 `state`および `externalTrigger`です。 サポートされる演算子は、 `>` （より大きい）、 `<` （より小さい）、 `==` （等しい）および `!=` （等しくない）です。 例えば、2019年4月20日以降に手動で作成、成功、および作成されたすべての実行が返されます。 `externalTrigger==true,state==SUCCESS,created>2019-04-20T13:37:00Z` |

**リクエスト**

次のリクエストは、指定したスケジュール済みクエリに対して最後に4回実行されたものを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs?limit=4
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたスケジュール済みクエリの実行のリストを持つHTTPステータス200をJSONとして返します。 次の応答は、指定されたスケジュール済みクエリに対する最近の4回の実行を返します。

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
>の値を使用して、指定したスケジュール済みクエリ `_links.cancel` の実行を [停止できます](#immediately-stop-a-run-for-a-specific-scheduled-query)。

### 特定のスケジュールされたクエリに対する実行を直ちにトリガーする

エンドポイントにPOSTリクエストを行うと、指定したスケジュール済みクエリに対して実行を直ちにトリガーできます。 `/schedules/{SCHEDULE_ID}/runs` には、実行をトリガーするスケジュール済みクエリの `{SCHEDULE_ID}``id` 値を指定します。

**API形式**

```http
POST /schedules/{SCHEDULE_ID}/runs
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、次のメッセージと共にHTTPステータス202（受け入れ済み）が返されます。

```json
{
    "message": "Request to trigger run of a scheduled query accepted.",
    "statusCode": 202
}
```

### 特定のスケジュールされたクエリの実行の詳細の取得

特定のスケジュール済みクエリに対する実行の詳細を取得するには、エンドポイントにGETリクエストを送信し、スケジュール済みクエリのIDと要求パスで実行の両方を指定します。 `/schedules/{SCHEDULE_ID}/runs/{RUN_ID}`

**API形式**

```http
GET /schedules/{SCHEDULE_ID}/runs/{RUN_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 詳細を取得する実行のスケジュール済みクエリの `id` 値。 |
| `{RUN_ID}` | 取得する実行の `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定された実行の詳細と共にHTTPステータス200を返します。

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

### 特定のスケジュールされたクエリの実行を直ちに停止する

エンドポイントにPATCHリクエストを送信し、スケジュールされたクエリのIDと要求パスで実行の両方を提供することで、特定のスケジュールされたクエリの実行を直ちに停止できます。 `/schedules/{SCHEDULE_ID}/runs/{RUN_ID}`

**API形式**

```http
PATCH /schedules/{SCHEDULE_ID}/runs/{RUN_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 詳細を取得する実行のスケジュール済みクエリの `id` 値。 |
| `{RUN_ID}` | 取得する実行の `id` 値。 |

**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの機能について詳しくは、APIの基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
     "op": "cancel"
 }'
```

**応答**

応答が成功すると、次のメッセージと共にHTTPステータス202（受け入れ済み）が返されます。

```json
{
    "message": "Request to cancel run of a scheduled query accepted",
    "statusCode": 202
}
```
