---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: runs for scheduled queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# スケジュールされたクエリ

## サンプルAPI呼び出し

これで、使用するヘッダーを理解できたので、クエリサービスAPIの呼び出しを開始できます。 以下の節では、クエリサービスAPIを使用して行える様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプル応答が含まれます。

### 指定したスケジュールリストのすべての実行のクエリの取得

特定のスケジュールされたリストに対するすべての実行のクエリを取得できます。現在実行中か、既に完了しているかは関係ありません。 これは、エンドポイントにGETリクエストを行うこ `/schedules/{SCHEDULE_ID}/runs` とで行われます。 `{SCHEDULE_ID}` は、実行 `id` を取得するスケジュール済みクエリの値です。

**API形式**

```http
GET /schedules/{SCHEDULE_ID}/runs
GET /schedules/{SCHEDULE_ID}/runs?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 取得す `id` るスケジュール済みクエリの値。 |
| `{QUERY_PARAMETERS}` | (オ&#x200B;*プション*)応答に返される結果を設定する要求パスに追加されるパラメーター。 複数のパラメーターを含め、アンパサンド(`&`)で区切ることができます。 使用可能なパラメーターを以下に示します。 |

**クエリパラメータ**

次に、指定したスケジュール済みクエリの実行を一覧表示するために使用できるクエリパラメータのリストを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、指定したスケジュール済みエンドポイントで使用可能なすべての実行がクエリされます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果を並べ替えるフィールドを指定します。 サポートされているフィールドは `created` とで `updated`す。 例えば、は、 `orderby=created` 昇順で結果を並べ替えます。 「作成前( `-` )」を追加す`orderby=-created`ると、アイテムが作成された降順で並べ替えられます。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。 (*Default value: 20*) |
| `start` | ゼロベースの番号付けを使用して、応答リストをオフセットします。 例えば、3番目 `start=2` のリストのリストからクエリを返します。 (*Default value: 0*) |
| `property` | フィールドに基づいて結果をフィルターします。 フィルター **は** HTMLエスケープする必要があります。 複数のコンマを使用して、複数のフィルターを組み合わせます。 サポートされているフィ `created`ールドは、、 `state`およびで `externalTrigger`す。 サポートされる演 `>` 算子のリストは、（より大きい）、 `<` （より小さい）、 `==` （次に等しい）、 `!=` （次に等しくない）です。 例えば、2019 `externalTrigger==true,state==SUCCESS,created>2019-04-20T13:37:00Z` 年4月20日以降に手動で作成、成功および作成されたすべての実行が返されます。 |

**リクエスト**

次のリクエストは、指定したスケジュール済みクエリの最後の4回の実行を取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs?limit=4
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定されたスケジュールされたクエリの実行のリストをJSONとしてHTTPステータス200を返します。 次の応答は、指定されたスケジュール済みクエリの最後の4回の実行を返します。

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

>[!NOTE] の値を使用して、指定したスケジ `_links.cancel` ュー [ルされたクエリの実行を停止できます](#immediately-stop-a-run-for-a-specific-scheduled-query)。

### 特定のスケジュールされたクエリの実行を直ちにトリガー

エンドポイントにPOSTリクエストを送信することで、指定したスケジュール済みクエリに対する実行を直ちにトリガーできます。 `/schedules/{SCHEDULE_ID}/runs` は、実行をト `{SCHEDULE_ID}``id` リガーするスケジュール済みクエリの値です。

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

成功した応答は、次のメッセージと共にHTTPステータス202（受け入れ済み）を返します。

```json
{
    "message": "Request to trigger run of a scheduled query accepted.",
    "statusCode": 202
}
```

### 特定のスケジュールされたクエリの実行の詳細の取得

特定のスケジュールされたクエリの実行に関する詳細を取得するには、エンドポイントにGET要求を行い、スケジュールされたクエリのIDと要求パスでの実行の両方を指定します。 `/schedules/{SCHEDULE_ID}/runs/{RUN_ID}`

**API形式**

```http
GET /schedules/{SCHEDULE_ID}/runs/{RUN_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 詳細を `id` 取得する実行対象のスケジュール済みクエリの値。 |
| `{RUN_ID}` | 取得 `id` する実行の値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm/runs/c2NoZWR1bGVkX18yMDIwLTAxLTA4VDIwOjQ1OjAwKzAwOjAw
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定された実行の詳細を含むHTTPステータス200を返します。

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

### 特定のスケジュールされたクエリの実行を直ちに停止

エンドポイントにPATCHリクエストを送信し、スケジュールされたクエリのIDと要求パスで実行の両方を指定することで、特定のスケジュールされたクエリに対する実行を直ちに停止できます。 `/schedules/{SCHEDULE_ID}/runs/{RUN_ID}`

**API形式**

```http
PATCH /schedules/{SCHEDULE_ID}/runs/{RUN_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | 詳細を `id` 取得する実行対象のスケジュール済みクエリの値。 |
| `{RUN_ID}` | 取得 `id` する実行の値。 |

**リクエスト**

このAPIリクエストは、ペイロードにJSONパッチ構文を使用します。 JSONパッチの仕組みについて詳しくは、APIの基本ドキュメントを参照してください。

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

成功した応答は、次のメッセージと共にHTTPステータス202（受け入れ済み）を返します。

```json
{
    "message": "Request to cancel run of a scheduled query accepted",
    "statusCode": 202
}
```
