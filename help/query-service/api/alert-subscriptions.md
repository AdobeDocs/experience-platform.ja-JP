---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；アラート；
title: アラート購読 API エンドポイント
description: このガイドは、クエリサービス API を使用してアラート購読エンドポイントに対して実行できる様々な API 呼び出しに対する HTTP リクエストの例と応答を提供します。
source-git-commit: cab7fcfda1bd8f6462af6e631f1fcee1f354d26b
workflow-type: tm+mt
source-wordcount: '2289'
ht-degree: 5%

---

# アラート購読 API エンドポイント

Adobe Experience Platformクエリサービスを使用すると、アドホッククエリとスケジュールされたクエリの両方でアラートを購読できます。 アラートは、E メール、Platform UI 内、またはその両方で受け取ることができます。 通知コンテンツは、プラットフォーム内アラートと E メールアラートで同じです。 現在、クエリアラートは、 [クエリサービス API](https://developer.adobe.com/experience-platform-apis/references/query-service/).

>[!IMPORTANT]
>
>電子メールアラートを受け取るには、まず UI 内でこの設定を有効にする必要があります。 詳しくは、 [メールアラートを有効にする方法の手順](../../observability/alerts/ui.md#enable-email-alerts).

次の表に、様々なタイプのクエリでサポートされるアラートのタイプを示します。

| クエリタイプ | サポートされるアラートのタイプ |
|---|---|
| アドホッククエリ | `success` または `failed` 実行数 |
| スケジュール済みクエリ | `start`, `success`または `failed` 実行数 |

>[!NOTE]
>
>非 SELECT クエリはすべてアラート購読をサポートし、アラートを購読するには、クエリ作成者である必要はありません。 また、他のユーザーは、作成しなかったクエリに関するアラートを登録して登録することもできます。

次のアラートは、アラート購読なしで適用されます。

* バッチクエリジョブが終了すると、ユーザーは通知を受け取ります。
* バッチクエリジョブの期間がしきい値を超えると、クエリをスケジュールした人に対してアラートがトリガーされます。

>[!NOTE]
>
>テストに使用するクエリは、適切に設定されていれば、これらのアラートから除外できます。

## サンプル API 呼び出し

以下の節では、クエリサービス API を使用しておこなえる様々な API 呼び出しについて説明します。各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

## 組織およびサンドボックスのすべてのアラートのリストの取得 {#get-list-of-org-alert-subs}

に対してGETリクエストを実行して、組織のサンドボックスのすべてのアラートのリストを取得する `/alert-subscriptions` endpoint.

**API 形式**

```http
GET /alert-subscriptions
```

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/query/alert-subscriptions' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-id: {SANDBOX_ID}'
```

**応答**

正常な応答は、HTTP 200 ステータスと `alerts` ページネーションおよびバージョン情報を含む配列。 この `alerts` 配列には、組織と特定のサンドボックスに関するすべてのアラートの詳細が含まれます。 1 回の応答で最大 3 つのアラートを使用できます。応答本文には、各アラートタイプごとに 1 つのアラートが含まれます。

>[!NOTE]
>
>この `alerts._links` オブジェクトを `alerts` 配列は簡潔にするために切り捨てられました。 以下に、 `alerts._links` オブジェクトは [応答のPOST](#subscribe-users).

```json
{
    "alerts": [
        {
            "assetId": "0ca168f4-e46b-4f7f-be6a-bdc386271b4a",
            "id": "query_service_flow_run_start-dcf7b4be-ccd7-4c73-ae0c-a4bb34a40adada84",
            "status": "enabled",
            "alertType": "start",
            "_links":{
                "self": {…},
                "subscribe": {…},
                "patch_status": {…},
                "get_list_of_subscribers_by_alert_type": {…},
                "delete": {…}
            }
        },
        {
            "assetId": "0ca168f4-e46b-4f7f-be6a-bdc386271b4a",
            "id": "query_service_flow_run_success-dcf7b4be-ccd7-4c73-ae0c-a4bb34a40adada84",
            "status": "enabled",
            "alertType": "success",
            "_links":{
                "self": {…},
                "subscribe": {…},
                "patch_status": {…},
                "get_list_of_subscribers_by_alert_type": {…},
                "delete": {…}
            }
        },
        {
            "assetId": "700d43d9-3b99-4d4c-8dbb-29c911c0e0df",
            "id": "query_service_flow_run_start-75da972a-e859-47a5-934c-629904daa1ef",
            "status": "enabled",
            "alertType": "start",
            "_links":{
                "self": {…},
                "subscribe": {…},
                "patch_status": {…},
                "get_list_of_subscribers_by_alert_type": {…},
                "delete": {…}
            }
        }
    ], 
    "_page": {
        "orderby": "-created",
        "page": 1,
        "count": 26,
        "pageSize": 50
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/alert-subscriptions?orderby=-created&page=2"
        },
        "prev": {
            "href": "https://platform.adobe.io/data/foundation/query/queries/alert-subscriptions?orderby=-created&page=0"
        }
    },
    "version": 1
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `alerts.assetId` | アラートを特定のクエリに関連付けたクエリ ID。 |
| `alerts.id` | アラートの名前。 この名前は、 Alerts サービスによって生成され、 Alerts ダッシュボードで使用されます。 アラート名は、アラートを保存するフォルダー ( `alertType`、フロー ID。 使用可能なアラートに関する情報は、 [Platform Alerts ダッシュボードドキュメント](../../observability/alerts/ui.md). |
| `alerts.status` | アラートのステータス値は 4 つです。 `enabled`, `enabling`, `disabled`、および `disabling`. アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま、将来の使用のために一時停止されているか、これらの状態間を移行しています。 |
| `alerts.alertType` | アラートのタイプ。 1 つのアラートには、次の 3 つの潜在的な値があります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `alerts._links` | このアラート ID に関連する情報の取得、更新、編集、削除に使用できる、使用可能なメソッドおよびエンドポイントに関する情報を提供します。 |
| `_page` | このオブジェクトには、順序、サイズ、合計ページ数および現在のページを説明するプロパティが含まれています。 |
| `_links` | オブジェクトには、次のページまたは前のページのリソースを取得するために使用できる URI 参照が含まれています。 |

## 特定のクエリ ID またはスケジュール ID のアラート購読情報を取得する {#retrieve-all-alert-subscriptions-by-id}

に対してGETリクエストを実行して、特定のクエリ ID またはスケジュール ID のアラート購読情報を取得する `/alert-subscriptions/{QUERY_ID}` または `/alert-subscriptions/{SCHEDULE_ID}` endpoint.

**API 形式**

```http
GET /alert-subscriptions/{QUERY_ID}
GET /alert-subscriptions/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | 購読情報を返すクエリの ID。 |
| `{SCHEDULE_ID}` | 購読情報を返すスケジュール済みクエリの ID。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/query/alert-subscriptions/4422fc69-eaa7-464e-945b-63cfd435d3d1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-id: {SANDBOX_ID}'
```

**応答**

正常な応答は、HTTP ステータス 200、 `alerts` 指定されたクエリ ID またはスケジュール ID の購読情報を含む配列。

```json
{
    "alerts": [
        {
            "assetId": "6df22232-f427-4250-a4e1-43cd30990641",
            "id": "query_service_flow_run_failure-5cdc3bbe-750a-4d80-9c43-96e5e09f1a96",
            "status": "enabled",
            "alertType": "failure",
            "subscriptions": {
                "emailNotifications": [
                    "rrunner@adobe.com",
                    "jsnow@adobe.com",
                    "keverdeen@adobe.com"
                ],
                "inContextNotifications": [
                    "rrunner@adobe.com",
                    "jsnow@adobe.com",
                    "keverdeen@adobe.com"
                ]
            },
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928",
                    "method": "GET"
                },
                "subscribe": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions",
                    "method": "POST",
                    "body": "{\"assetId\": \"queryId/scheduleId\", \"alertType\": \"start/success/failure\", \"subscriptions\": {\n\"emailIds\": [\"xyz@example.com\", \"abc@example.com\"], \"email\": true, \"inContext\": false}}"
                },
                "patch_status": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "PATCH",
                    "body": "{ \"op\": \"replace\", \"path\": \"/status\", \"value\": \"enable/disable\" }"
                },
                "get_list_of_subscribers_by_alert_type": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "DELETE"
                }
            }
        },
        {
            "assetId": "6df22232-f427-4250-a4e1-43cd30990641",
            "id": "query_service_flow_run_start-5cdc3bbe-750a-4d80-9c43-96e5e09f1a96",
            "status": "enabled",
            "alertType": "start",
            "subscriptions": {
                "emailNotifications": [
                    "rrunner@adobe.com"
                ],
                "inContextNotifications": [
                    "rrunner@adobe.com"
                ]
            },
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928",
                    "method": "GET"
                },
                "subscribe": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions",
                    "method": "POST",
                    "body": "{\"assetId\": \"queryId/scheduleId\", \"alertType\": \"start/success/failure\", \"subscriptions\": {\n\"emailIds\": [\"xyz@example.com\", \"abc@example.com\"], \"email\": true, \"inContext\": false}}"
                },
                "patch_status": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "PATCH",
                    "body": "{ \"op\": \"replace\", \"path\": \"/status\", \"value\": \"enable/disable\" }"
                },
                "get_list_of_subscribers_by_alert_type": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "DELETE"
                }
            }
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `assetId` | アラートはこの ID に関連付けられています。 ID は、クエリ ID またはスケジュール ID のどちらかです。 |
| `id` | アラートの名前。 この名前は、 Alerts サービスによって生成され、 Alerts ダッシュボードで使用されます。 アラート名は、アラートを保存するフォルダー ( `alertType`、フロー ID。 使用可能なアラートに関する情報は、 [Platform Alerts ダッシュボードドキュメント](../../observability/alerts/ui.md). |
| `status` | アラートのステータス値は 4 つです。 `enabled`, `enabling`, `disabled`、および `disabling`. アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま、将来の使用のために一時停止されているか、これらの状態間を移行しています。 |
| `alertType` | 各アラートは、3 つの異なるアラートタイプを持つことができます。 次のようになります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `subscriptions.emailNotifications` | アラートのAdobeを購読したユーザーがアラートの電子メールを受信するために登録した電子メールアドレスの配列。 |
| `subscriptions.inContextNotifications` | アラートの UI 通知をAdobeしたユーザー用に登録された電子メールアドレスの配列。 |

## 特定のクエリまたはスケジュール ID とアラートタイプのアラート購読情報を取得する {#get-alert-info-by-id-and-alert-type}

に対してGETリクエストを実行して、特定の ID およびアラートタイプのアラート購読情報を取得する `/alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}` endpoint. これは、クエリ ID とスケジュール済みクエリ ID の両方に適用できます。

**API 形式**

```http
GET /alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}
GET /alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `ALERT_TYPE` | 各アラートは、3 つの異なるアラートタイプを持つことができます。 次のようになります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `QUERY_ID` | 更新するクエリの一意の ID。 |
| `SCHEDULE_ID` | 更新するスケジュール済みクエリの一意の ID。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/query/alert-subscriptions/4422fc69-eaa7-464e-945b-63cfd435d3d1/start'' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-id: {SANDBOX_ID}'
```

**応答**

正常な応答は、200 の HTTP ステータスと、購読しているすべてのアラートを返します。 これには、アラート ID、アラートのタイプ、購読者のAdobe登録電子メール ID、優先通知チャネルが含まれます。

```json
{
    "alerts": [
        {
            "assetId": "6df22232-f427-4250-a4e1-43cd30990641",
            "id": "query_service_flow_run_success-5cdc3bbe-750a-4d80-9c43-96e5e09f1a96",
            "status": "enabled",
            "alertType": "success",
            "subscriptions": {
                "emailNotifications": [
                    "rrunner@adobe.com",
                    "jsnow@adobe.com"
                ],
                "inContextNotifications": [
                    "jsnow@adobe.com"
                ]
            },
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928",
                    "method": "GET"
                },
                "subscribe": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions",
                    "method": "POST",
                    "body": "{\"assetId\": \"queryId/scheduleId\", \"alertType\": \"start/success/failure\", \"subscriptions\": {\n\"emailIds\": [\"xyz@example.com\", \"abc@example.com\"], \"email\": true, \"inContext\": false}}"
                },
                "patch_status": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "PATCH",
                    "body": "{ \"op\": \"replace\", \"path\": \"/status\", \"value\": \"enable/disable\" }"
                },
                "get_list_of_subscribers_by_alert_type": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "DELETE"
                }
            }
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `assetId` | アラートを特定のクエリに関連付けたクエリ ID。 |
| `alertType` | アラートのタイプ。 1 つのアラートには、次の 3 つの潜在的な値があります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `subscriptions` | アラートに関連付けられたAdobe登録 E メール ID と、ユーザーがアラートを受け取るチャネルを渡すために使用されるオブジェクト。 |
| `subscriptions.inContextNotifications` | アラートの UI 通知をAdobeしたユーザー用に登録された電子メールアドレスの配列。 |
| `subscriptions.emailNotifications` | アラートのAdobeを購読したユーザーがアラートの電子メールを受信するために登録した電子メールアドレスの配列。 |

## ユーザーが購読しているすべてのアラートのリストを取得する {#get-alert-subscription-list}

に対してGETリクエストをおこなって、ユーザーが購読しているすべてのアラートのリストを取得します `/alert-subscriptions/user-subscriptions/{EMAIL_ID}` endpoint. 応答には、アラート名、ID、ステータス、アラートタイプ、通知チャネルが含まれます。

**API 形式**

```http
GET /alert-subscriptions/user-subscriptions/{EMAIL_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{EMAIL_ID}` | アラートアカウントに登録されている電子メールアドレスは、Adobeを購読しているユーザーを識別するために使用されます。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/query/alert-subscriptions/user-subscriptions/rrunner@adobe.com' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-id: {SANDBOX_ID}'
```

**応答**

正常な応答は、HTTP ステータス 200、および `items` が購読しているアラートの詳細を含む配列 `emailId` 提供済み

```json
{
    "items": [
        {
            "name": "query_service_flow_run_success-8f057161-b312-4274-b629-f346c7d15c1f",
            "assetId": "39e65373-e47a-4feb-9e5a-dffa2f677bca",
            "status": "enabled",
            "alertType": "success",
            "subscriptions": {
                "inContextNotification": true,
                "emailNotifications": true
            },
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928",
                    "method": "GET"
                },
                "subscribe": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions",
                    "method": "POST",
                    "body": "{\"assetId\": \"queryId/scheduleId\", \"alertType\": \"start/success/failure\", \"subscriptions\": {\n\"emailIds\": [\"xyz@example.com\", \"abc@example.com\"], \"email\": true, \"inContext\": false}}"
                },
                "patch_status": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "PATCH",
                    "body": "{ \"op\": \"replace\", \"path\": \"/status\", \"value\": \"enable/disable\" }"
                },
                "get_list_of_subscribers_by_alert_type": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "DELETE"
                }
            }
        },
        {
            "name": "query_service_flow_run_start-8f057161-b312-4274-b629-f346c7d15c1f",
            "assetId": "39e65373-e47a-4feb-9e5a-dffa2f677bca",
            "status": "enabled",
            "alertType": "start",
            "subscriptions": {
                "inContextNotification": true,
                "emailNotifications": true
            },
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928",
                    "method": "GET"
                },
                "subscribe": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions",
                    "method": "POST",
                    "body": "{\"assetId\": \"queryId/scheduleId\", \"alertType\": \"start/success/failure\", \"subscriptions\": {\n\"emailIds\": [\"xyz@example.com\", \"abc@example.com\"], \"email\": true, \"inContext\": false}}"
                },
                "patch_status": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "PATCH",
                    "body": "{ \"op\": \"replace\", \"path\": \"/status\", \"value\": \"enable/disable\" }"
                },
                "get_list_of_subscribers_by_alert_type": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "GET"
                },
                "delete": {
                    "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
                    "method": "DELETE"
                }
            }
        }
    ], "_page": {
            "orderby": "-created",
            "page": 1,
            "count": 26,
            "pageSize": 50
        },
    "_links": {
        "next": {
            "href": "https://platform-int.adobe.io/data/foundation/query/queries/alert-subscriptions?orderby=-created&page=2"
        },
        "prev": {
            "href": "https://platform-int.adobe.io/data/foundation/query/queries/alert-subscriptions?orderby=-created&page=0"
        }
    },
    "version": 1
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | アラートの名前。 この名前は、 Alerts サービスによって生成され、 Alerts ダッシュボードで使用されます。 アラート名は、アラートを保存するフォルダー ( `alertType`、フロー ID。 使用可能なアラートに関する情報は、 [Platform Alerts ダッシュボードドキュメント](../../observability/alerts/ui.md). |
| `assetId` | アラートを特定のクエリに関連付けたクエリ ID。 |
| `status` | アラートのステータス値は 4 つです。 `enabled`, `enabling`, `disabled`、および `disabling`. アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま、将来の使用のために一時停止されているか、これらの状態間を移行しています。 |
| `alertType` | アラートのタイプ。 1 つのアラートには、次の 3 つの潜在的な値があります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `subscriptions` | アラートに関連付けられたAdobe登録 E メール ID と、ユーザーがアラートを受け取るチャネルを渡すために使用されるオブジェクト。 |
| `subscriptions.inContextNotifications` | ユーザーがアラート通知を受け取る方法を決定する boolean 値です。 A `true` の値によって、UI を通じてアラートを提供する必要があることを確認します。 A `false` の値を指定することで、そのチャネルを通じてユーザーに通知されなくなります。 |
| `subscriptions.emailNotifications` | ユーザーがアラート通知を受け取る方法を決定する boolean 値です。 A `true` 値は、アラートを e メールで提供する必要があることを確認します。 A `false` の値を指定することで、そのチャネルを通じてユーザーに通知されなくなります。 |

## アラートの作成とユーザーの購読 {#subscribe-users}

アラートを作成し、ユーザーを購読して受信するには、 `POST` にリクエスト `/alert-subscriptions` endpoint. このリクエストは、 `assetId` プロパティを使用して、ユーザーをそのクエリに関するアラートに登録します。 `emailIds`.

>[!IMPORTANT]
>
>1 回のリクエストで最大 5 個のAdobe登録電子メール ID を渡すことができます。 1 つのアラートに対して 5 人以上のユーザーを登録するには、後続のリクエストをおこなう必要があります。

**API 形式**

```http
POST /alert-subscriptions
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/alert-subscriptions
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '
    {
    "assetId": "a679dd0e-bcb2-4e69-a610-22d17ba98cac",
    "alertType": "failure",
    "subscriptions": {
        "emailIds": [
            "rrunner@adobe.com",
            "jsnow@adobe.com"
        ],
        "inContextNotifications": true,
        "emailNotifications": true
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `assetId` | アラートはこの ID に関連付けられています。 ID は、クエリ ID またはスケジュール ID のどちらかです。 |
| `alertType` | アラートのタイプ。 1 つのアラートには、次の 3 つの潜在的な値があります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `subscriptions` | アラートに関連付けられたAdobe登録 E メール ID と、ユーザーがアラートを受け取るチャネルを渡すために使用されるオブジェクト。 |
| `subscriptions.emailIds` | アラートを受信する必要のあるユーザーを識別する電子メールアドレスの配列。 電子メールアドレス **必須** が登録されていることをAdobeします。 |
| `subscriptions.inContextNotifications` | ユーザーがアラート通知を受け取る方法を決定する boolean 値です。 A `true` の値によって、UI を通じてアラートを提供する必要があることを確認します。 A `false` の値を指定することで、そのチャネルを通じてユーザーに通知されなくなります。 |
| `subscriptions.emailNotifications` | ユーザーがアラート通知を受け取る方法を決定する boolean 値です。 A `true` 値は、アラートを e メールで提供する必要があることを確認します。 A `false` の値を指定することで、そのチャネルを通じてユーザーに通知されなくなります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 202（許可済み）と、新しく作成されたアラートの詳細を返します。

```json
{
    "assetId": "c4f67291-1161-4943-bc29-8736469bb928",
    "id": "query_service_flow_run_failure-5f4cb942-b67c-4ea4-a90d-5b6245e60aca",
    "alertType": "failure",
    "subscriptions": {
        "emailIds": [
            "{USER_EMAIL_ID}"
        ],
        "inContextNotifications": false,
        "emailNotifications": true
    },
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928",
            "method": "GET"
        },
        "subscribe": {
            "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions",
            "method": "POST",
            "body": "{\"assetId\": \"queryId/scheduleId\", \"alertType\": \"start/success/failure\", \"subscriptions\": {\n\"emailIds\": [\"xyz@example.com\", \"abc@example.com\"], \"email\": true, \"inContext\": false}}"
        },
        "patch_status": {
            "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
            "method": "PATCH",
            "body": "{ \"op\": \"replace\", \"path\": \"/status\", \"value\": \"enable/disable\" }"
        },
        "get_list_of_subscribers_by_alert_type": {
            "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
            "method": "GET"
        },
        "delete": {
            "href": "https://platform.adobe.io/data/foundation/query/alert-subscriptions/c4f67291-1161-4943-bc29-8736469bb928/failure",
            "method": "DELETE"
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | アラートの名前。 この名前は、 Alerts サービスによって生成され、 Alerts ダッシュボードで使用されます。 アラート名は、アラートを保存するフォルダー ( `alertType`、フロー ID。 使用可能なアラートに関する情報は、 [Platform Alerts ダッシュボードドキュメント](../../observability/alerts/ui.md). |
| `_links` | このアラート ID に関連する情報の取得、更新、編集、削除に使用できる、使用可能なメソッドおよびエンドポイントに関する情報を提供します。 |

## アラートの有効化または無効化 {#enable-or-disable-alert}

このリクエストは、クエリ ID またはスケジュール ID とアラートタイプを使用して特定のアラートを参照し、アラートステータスを `enable` または `disable`. アラートのステータスは、 `PATCH` にリクエスト `/alert-subscriptions/{queryId}/{alertType}` または `/alert-subscriptions/{scheduleId}/{alertType}` endpoint.

**API 形式**

```http
PATCH /alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}
PATCH /alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `ALERT_TYPE` | アラートのタイプ。 1 つのアラートには、次の 3 つの潜在的な値があります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul>変更するには、エンドポイントの名前空間で現在のアラートタイプを指定する必要があります。 |
| `QUERY_ID` | 更新するクエリの一意の ID。 |
| `SCHEDULE_ID` | 更新するスケジュール済みクエリの一意の ID。 |

**リクエスト**

```shell
curl -X PATCH 'https://platform.adobe.io/data/foundation/query/alert-subscriptions/4422fc69-eaa7-464e-945b-63cfd435d3d1/start'' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-id: {SANDBOX_ID}' \
  -d '{
        "op": "replace",
        "path" : "/status",
        "value": "enable"
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `op` | 実行する 操作。現在、許可されている値は `replace` のみです。 |
| `path` | この値は、エンドポイントの名前空間に関連します。 現在、許可されている値は `/status` のみです。 |
| `value` | 成功したPATCHリクエストでは、 `status` アラートの値。 現在、指定できる値は次のとおりです。 `enable` または `disable`. |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 200 と、アラートのステータス、タイプ、ID の詳細、および関連するクエリを返します。

```json
{
    "id" : "query_service_flow_run_success-4422fc69-eaa7-464e-945b-63cfd435d3d1",
    "assetId": "4422fc69-eaa7-464e-945b-63cfd435d3d1", 
    "alertType": "start",
    "status": "enabled"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | アラートの名前。 この名前は、 Alerts サービスによって生成され、 Alerts ダッシュボードで使用されます。 アラート名は、アラートを保存するフォルダー ( `alertType`、フロー ID。 使用可能なアラートに関する情報は、 [Platform Alerts ダッシュボードドキュメント](../../observability/alerts/ui.md). |
| `assetId` | アラートはこの ID に関連付けられています。 ID は、クエリ ID またはスケジュール ID のどちらかです。 |
| `alertType` | 各アラートは、3 つの異なるアラートタイプを持つことができます。 次のようになります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `status` | アラートのステータス値は 4 つです。 `enabled`, `enabling`, `disabled`、および `disabling`. アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま、将来の使用のために一時停止されているか、これらの状態間を移行しています。 |

## 特定のクエリおよびアラートタイプのアラートの削除 {#delete-alert-info-by-id-and-alert-type}

にDELETEリクエストを実行して、特定のクエリまたはスケジュール ID とアラートタイプのアラートを削除する `/alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}` または `/alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}` endpoint.

```http
DELETE /alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}
DELETE /alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `ALERT_TYPE` | アラートのタイプ。 1 つのアラートには、次の 3 つの潜在的な値があります。 <ul><li>`start`:クエリの実行が開始された場合に、ユーザーに通知します。</li><li>`success`:クエリが完了すると、ユーザーに通知します。</li><li>`failure`:クエリが失敗した場合にユーザーに通知します。</li></ul> DELETEリクエストは、指定された特定のアラートタイプにのみ適用されます。 |
| `QUERY_ID` | 更新するクエリの一意の ID。 |
| `SCHEDULE_ID` | 更新するスケジュール済みクエリの一意の ID。 |

**リクエスト**

```shell
curl -X DELETE 'https://platform.adobe.io/data/foundation/query/alert-subscriptions/4422fc69-eaa7-464e-945b-63cfd435d3d1/start' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-id: {SANDBOX_ID}'
```

**応答**

正常な応答は、HTTP 200 ステータスと、アセット ID と削除されたアラートのアラートタイプを含む確認メッセージを返します。

```json
{
"message": "Alert Deleted Successfully for assetId: 6df22232-f427-4250-a4e1-43cd30990641 and alertType: success",
"statusCode": 200
}
```

## 次の手順

このガイドでは、 `/alert-subscriptions` エンドポイント（クエリサービス API 内）を使用して、 このガイドを読むと、クエリのアラートの作成、ユーザーのアラートへの登録、使用可能なアラートの種類、アラート購読情報の取得、更新、削除方法に関する理解が深まりました。

詳しくは、 [クエリサービス API ガイド](./getting-started.md) を参照して、他の使用可能な機能や操作の詳細を確認してください。
