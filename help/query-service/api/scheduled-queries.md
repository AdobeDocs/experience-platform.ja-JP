---
keywords: Experience Platform；ホーム；人気のトピック；Query Service;Query Service；スケジュール済みクエリ；スケジュール済みクエリ；
solution: Experience Platform
title: スケジュールエンドポイント
description: 以下の節では、Query Service API を使用してスケジュールされたクエリに対して実行できる様々な API 呼び出しについて説明します。
role: Developer
exl-id: f57dbda5-da50-4812-a924-c8571349f1cd
source-git-commit: 10c0c5c639226879b1ca25391fc4a1006cf40003
workflow-type: tm+mt
source-wordcount: '1410'
ht-degree: 46%

---

# スケジュールエンドポイント

Query Service スケジュール API を使用して、スケジュールされたクエリをプログラムで作成、管理、監視する方法と、詳細な情報および例について説明します。

## 要件と前提条件

テクニカルアカウント（OAuth サーバー間資格情報を介して認証）または個人用ユーザーアカウント（ユーザートークン）を使用して、スケジュールされたクエリを作成できます。 ただし、Adobeでは、特に長期的なワークロードや実稼動のワークロードのために、中断のない安全なスケジュール済みクエリの実行を確保するために、テクニカルアカウントを使用することを強くお勧めします。

個人のユーザーアカウントで作成されたクエリは、そのユーザーのアクセス権が取り消された場合、またはアカウントが無効になっている場合は失敗します。 テクニカルアカウントは、個々のユーザーの雇用状況やアクセス権に結び付けられないため、安定性が高まります。

>[!IMPORTANT]
>
>スケジュール済みクエリを管理する際の重要な考慮事項：<ul><li>スケジュールされたクエリは、クエリの作成に使用したアカウント（技術またはユーザー）がアクセス権または権限を失った場合に失敗します。</li><li>スケジュールされたクエリは、API または UI で削除する前に無効にする必要があります。</li><li>終了日を指定せずに無期限にスケジュールすることはできません。終了日は必ず指定する必要があります。</li></ul>

アカウント要件、権限の設定、スケジュールされたクエリの管理に関する詳しいガイダンスについては、[&#x200B; クエリスケジュールのドキュメント &#x200B;](../ui/query-schedules.md#technical-account-user-requirements) を参照してください。 テクニカルアカウントの作成と設定に関する詳細な手順については、[Developer Consoleの設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/set-up-developer-console-and-postman) および [&#x200B; エンドツーエンドのテクニカルアカウントの設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/platform-learn/tutorial-comprehensive-technical/setup) を参照してください。

## サンプル API 呼び出し

必要な認証ヘッダーを設定したら（『 [API 認証ガイド &#x200B;](../../landing/api-authentication.md) を参照）、[!DNL Query Service] API への呼び出しを開始できます。 以下の節では、一般的な形式を持つ様々な API 呼び出し、必要なヘッダーを含むリクエストの例、応答のサンプルを示します。

### スケジュールされたクエリのリストの取得

`/schedules` エンドポイントにGET リクエストを送信すると、組織でスケジュールされたすべてのクエリのリストを取得できます。

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
| `start` | 結果を並べ替える ISO 形式タイムスタンプを指定します。 開始日が指定されていない場合、API 呼び出しでは、最も古く作成されたスケジュール済みクエリが最初に返され、次に、より新しい結果が引き続きリストされます。ISO タイムスタンプ <br> 使用すると、日付と時間に様々なレベルの精度を設定できます。 基本の ISO タイムスタンプは、2020 年 9 月 7 日を表すために `2020-09-07` の形式を取ります。 より複雑な例は、`2022-11-05T08:15:30-05:00` と記述され、2022 年 11 月 5 日午前 8:15:30 分（米国東部標準時）に対応します。 タイムゾーンには UTC オフセットを指定でき、サフィックス「Z」（`2020-01-01T01:01:01Z`）で示されます。 タイムゾーンが指定されない場合は、デフォルトで 0 に設定されます。 |
| `property` | フィールドに基づいて結果をフィルタリングします。フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。サポートされるフィールドは `created`、`templateId`、および `userId` です。サポートされる演算子のリストは `>`（より大きい）、`<`（より小さい）、および `==`（等しい）です。例えば、`userId==6ebd9c2d-494d-425a-aa91-24033f3abeec` は、指定されたものと同じたユーザー ID を持つスケジュール済みクエリをすべて返します。 |

**リクエスト**

次のリクエストでは、組織に対して作成された最新のスケジュール済みクエリを取得します。

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/schedules?limit=1
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTP ステータス 200 が、指定された組織に対してスケジュールされたクエリのリストと共に返されます。 次の応答では、組織に対して作成された最新のスケジュール済みクエリが返されます。

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

`/schedules` エンドポイントに POST リクエストをおこなうと、新しいスケジュールされたクエリを作成できます。 API でスケジュールされたクエリを作成すると、クエリエディターでも表示できます。 UI のスケジュール済みクエリについて詳しくは、[&#x200B; クエリエディターのドキュメント &#x200B;](../ui/user-guide.md#scheduled-queries) を参照してください。

**API 形式**

```http
POST /schedules
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/schedules
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `query.dbName` | スケジュールされたクエリを実行するデータベースの名前。 |
| `query.sql` | 定義したスケジュールで実行する SQL クエリ。 |
| `query.name` | スケジュール済みクエリの名前。 |
| `query.description` | スケジュールされたクエリのオプション説明。 |
| `schedule.schedule` | クエリの cron スケジュール。Cron 式を作成、検証、理解するためのインタラクティブな方法については、[Crontab.guru](https://crontab.guru/) を参照してください。 この例では、「30 * * * *」は、クエリが毎時 30 分に実行されることを意味します。<br><br> または、次の短縮形の式を使用することもできます。<ul><li>`@once`：クエリは 1 回だけ実行されます。</li><li>`@hourly`：クエリは 1 時間ごとに、その時間の初めに実行されます。 これは cron 式 `0 * * * *` と同等です。</li><li>`@daily`：クエリは 1 日 1 回午前 0 時に実行されます。 これは cron 式 `0 0 * * *` と同等です。</li><li>`@weekly`：クエリは、週に 1 回、日曜日、深夜に実行されます。 これは cron 式 `0 0 * * 0` と同等です。</li><li>`@monthly`：クエリは月に 1 回、月の初日の午前 0 時に実行されます。 これは cron 式 `0 0 1 * *` と同等です。</li><li>`@yearly`: クエリは、年に 1 回、1 月 1 日、深夜に実行されます。 これは cron 式 `0 0 1 1 *` と同等です。 |
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
>`_links.delete` の値を使用して、[&#x200B; 作成したスケジュール済みクエリを削除 &#x200B;](#delete-a-specified-scheduled-query) できます。

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
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
>`_links.delete` の値を使用して、[&#x200B; 作成したスケジュール済みクエリを削除 &#x200B;](#delete-a-specified-scheduled-query) できます。

### 指定したスケジュール済みクエリの詳細の更新

指定したスケジュール済みクエリの詳細を更新するには、`/schedules` エンドポイントに PATCH リクエストを送信し、リクエストパスでその ID を指定します。

PATCH リクエストは、`/state` と `/schedule/schedule` の 2 つのパスをサポートします。

### スケジュール済みクエリの状態の更新

選択したスケジュール済みクエリの状態を更新するには、`path` プロパティを `/state` に設定し、`value` プロパティを `enable` または `disable` に設定します。

**API 形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | PATCHに設定するスケジュール済みクエリの `id` 値。 |


**リクエスト**

この API リクエストは、ペイロードに JSON パッチ構文を使用します。JSON パッチの仕組みについて詳しくは、API の基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `op` | クエリ スケジュールに対して実行する操作。 指定できる値は、`replace` です。 |
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

リクエスト本文で `path` プロパティを `/schedule/schedule` に設定することで、スケジュールされたクエリの cron スケジュールを更新できます。 cron スケジュールの詳細については、[cron 式形式のドキュメント](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。

**API 形式**

```http
PATCH /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | PATCHに設定するスケジュール済みクエリの `id` 値。 |

**リクエスト**

この API リクエストは、ペイロードに JSON パッチ構文を使用します。JSON パッチの仕組みについて詳しくは、API の基本ドキュメントを参照してください。

```shell
curl -X PATCH https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `op` | クエリ スケジュールに対して実行する操作。 指定できる値は、`replace` です。 |
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
>スケジュール **削除する前に** 無効にする必要があります。

**API 形式**

```http
DELETE /schedules/{SCHEDULE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{SCHEDULE_ID}` | DELETEに設定するスケジュール済みクエリの `id` 値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/query/schedules/e95186d65a28abf00a495d82_28e74200-e3de-11e9-8f5d-7f27416c5f0d_sample_scheduled_query7omob151bm_birvwm
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
