---
keywords: Experience Platform;ホーム;人気のトピック;クエリサービス;Query service;アラート;
title: アラート購読エンドポイント
description: このガイドは、Query Service API を使用してアラート購読エンドポイントに対して実行できる、様々な API 呼び出しに対する HTTP リクエストの例と応答を提供します。
exl-id: 30ac587a-2286-4a52-9199-7a2a8acd5362
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '2661'
ht-degree: 89%

---

# アラート購読エンドポイント

Adobe Experience Platform クエリサービスを使用すると、アドホッククエリとスケジュールされたクエリの両方でアラートを受け取ることができます。アラートは、メール、Platform UI 内またはその両方で受け取ることができます。通知コンテンツは、Platform 内アラートとメールアラートで同じです。 現在、クエリアラートは、 [クエリサービス API](https://developer.adobe.com/experience-platform-apis/references/query-service/).

>[!IMPORTANT]
>
>メールアラートを受け取るには、まず UI 内でこの設定を有効にする必要があります。詳しくは、[メールアラートを有効にする手順](../../observability/alerts/ui.md#enable-email-alerts)についてのドキュメントを参照してください。

次の表に、様々なタイプのクエリでサポートされるアラートのタイプを示します。

| クエリタイプ | サポートされるアラートのタイプ |
|---|---|
| アドホッククエリ | `success` または `failed` の実行。 |
| スケジュール済みクエリ | `start`、`success` または `failed` の実行。 |

>[!NOTE]
>
>非 SELECT クエリはすべてアラート購読をサポートし、クエリ作成者でなくてもアラートを購読できます。他のユーザーも、自分が作成しなかったクエリに関するアラートを登録できます。

次のアラートは、アラート購読なしで適用されます。

* バッチクエリジョブが終了すると、ユーザーは通知を受け取ります。
* バッチクエリジョブの期間がしきい値を超えると、クエリをスケジュールしたユーザーに対してアラートがトリガーされます。

>[!NOTE]
>
>テストに使用するクエリは、適切に設定されていれば、これらのアラートから除外できます。

## サンプル API 呼び出し

以下の節では、クエリサービス API を使用しておこなえる様々な API 呼び出しについて説明します。各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

## 組織およびサンドボックスのすべてのアラートのリストの取得 {#get-list-of-org-alert-subs}

`/alert-subscriptions` エンドポイントに対して GET リクエストを実行して、組織のサンドボックスのすべてのアラートのリストを取得します。

**API 形式**

```http
GET /alert-subscriptions
GET /alert-subscriptions?{QUERY_PARAMETERS}
```

| プロパティ | 説明 |
| --------- | ----------- |
| `{QUERY_PARAMETERS}` | （オプション）リクエストパスに追加されるパラメーター。このリクエストパスは応答で返される結果を設定します。複数のパラメーターを含める場合は、アンパサンド（&amp;）で区切ります。使用できるパラメーターは以下のとおりです。 |

**クエリパラメータ**

次に、クエリの一覧表示に使用可能なクエリパラメーターのリストを示します。これらのパラメーターはすべてオプションです。パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのクエリが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `orderby` | 結果の順序を指定するフィールド。 サポートされているフィールドは `created` と `updated` です。プロパティ名の前にを追加します。 `+` ( 昇順と `-` 降順。 デフォルトは `-created` です。なお、プラス記号 (`+`) はでエスケープする必要があります `%2B`. 例： `%2Bcreated` は、昇順で作成される順序の値です。 |
| `pagesize` | ページごとに API 呼び出しから取得するレコードの数を制御するには、このパラメーターを使用します。 デフォルトの制限は、1 ページあたり最大 50 レコード数に設定されています。 |
| `page` | レコードを表示する、返された結果のページ番号を指定します。 |
| `property` | 選択したフィールドに基づいて結果をフィルターします。 フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。次のプロパティを使用して、フィルタリングを実行できます。 <ul><li>ID</li><li>assetId</li><li>status</li><li>alertType</li></ul> サポートされる演算子は次のとおりです。 `==` （等しい）。 例： `id==6ebd9c2d-494d-425a-aa91-24033f3abeec` は、一致する ID を持つアラートを返します。 |

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

応答が成功すると、HTTP 200 ステータスと、ページネーションおよびバージョン情報を含む `alerts` 配列が返されます。この `alerts` 配列には、組織と特定のサンドボックスに関するすべてのアラートの詳細が含まれます。 1 回の応答で最大 3 つのアラートを使用できます。応答本文には、各アラートタイプごとに 1 つのアラートが含まれます。

>[!NOTE]
>
>`alerts` 配列の `alerts._links` オブジェクトは、簡潔にするために切り捨てられました。`alerts._links` オブジェクトの完全な例は、[POST リクエスト](#subscribe-users)の応答で見ることができます。

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
| `alerts.id` | アラート名。この名前は、アラートサービスによって生成され、アラートダッシュボードで使用されます。アラート名は、アラートを保存するフォルダー、`alertType`、フロー ID で構成されます。 使用可能なアラートに関する情報は、[Platform のアラートダッシュボードドキュメント](../../observability/alerts/ui.md)を参照してください。 |
| `alerts.status` | アラートのステータス値は、`enabled`、`enabling`、`disabled`、`disabling` の 4 つです。アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま将来利用するために一時停止されているか、または、これらのステート間を移行中です。 |
| `alerts.alertType` | アラートのタイプ。1 つのアラートに対して、次の 3 つの値が考えられす。 <ul><li>`start`：クエリの実行が開始されたときに、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `alerts._links` | このアラート ID に関連する情報の取得、更新、編集、削除に使用できる、使用可能なメソッドおよびエンドポイントに関する情報を提供します。 |
| `_page` | このオブジェクトには、順序、サイズ、合計ページ数および現在のページを説明するプロパティが含まれています。 |
| `_links` | オブジェクトには、リソースの次のページまたは前のページを取得するために使用できる URI 参照が含まれています。 |

## 特定のクエリまたはスケジュール ID のアラート購読情報を取得 {#retrieve-all-alert-subscriptions-by-id}

`/alert-subscriptions/{QUERY_ID}` または `/alert-subscriptions/{SCHEDULE_ID}` エンドポイントに対して GET リクエストを実行して、特定のクエリ ID またはスケジュール ID のアラート購読情報を取得します。

**API 形式**

```http
GET /alert-subscriptions/{QUERY_ID}
GET /alert-subscriptions/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{QUERY_ID}` | 購読情報を返すクエリの ID。 |
| `{SCHEDULE_ID}` | 購読情報を返すスケジュールされたクエリの ID。 |

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

正常な応答は、HTTP ステータス 200 と、指定されたクエリ ID またはスケジュール ID の購読情報を含む `alerts` 配列を返します。

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
| `assetId` | アラートはこの ID に関連付けられています。ID は、クエリ ID またはスケジュール ID のどちらかです。 |
| `id` | アラート名。この名前は、アラートサービスによって生成され、アラートダッシュボードで使用されます。アラート名は、アラートを保存するフォルダー、`alertType`、フロー ID で構成されます。 使用可能なアラートに関する情報は、[Platform のアラートダッシュボードドキュメント](../../observability/alerts/ui.md)を参照してください。 |
| `status` | アラートのステータス値は、`enabled`、`enabling`、`disabled`、`disabling` の 4 つです。アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま将来利用するために一時停止されているか、または、これらのステート間を移行中です。 |
| `alertType` | 各アラートは、3 つの異なるアラートタイプを持つことができます。 次のとおりです。 <ul><li>`start`：クエリの実行が開始されると、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合、ユーザーに通知します。</li></ul> |
| `subscriptions.emailNotifications` | アラートのメール受信を申し込んだユーザーのアドビ登録メールアドレスの配列。 |
| `subscriptions.inContextNotifications` | アラートの UI 通知に登録しているユーザーのアドビ登録メールアドレスの配列。 |

## 特定のクエリ、スケジュール ID とアラートタイプのアラート購読情報を取得する {#get-alert-info-by-id-and-alert-type}

`/alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}` エンドポイントに対して GET リクエストを実行して、特定の ID およびアラートタイプのアラート購読情報を取得します。これは、クエリ ID とスケジュール済みクエリ ID の両方に適用できます。

**API 形式**

```http
GET /alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}
GET /alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `ALERT_TYPE` | このプロパティは、アラートを実行するクエリ実行のトリガーを示します。 応答には、このタイプのアラートのアラート購読情報のみが含まれます。 各アラートは、3 つの異なるアラートタイプを持つことができます。 次のとおりです。 <ul><li>`start`：クエリの実行が開始されると、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合、ユーザーに通知します。</li></ul> |
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

応答が成功すると、HTTP ステータス 200 と、購読しているすべてのアラートが返されます。 これには、アラート ID、アラートのタイプ、購読者のアドビ登録メール ID、優先通知チャネルが含まれます。

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
| `alertType` | アラートのタイプ。1 つのアラートに対して、次の 3 つの値が考えられす。 <ul><li>`start`：クエリの実行が開始されたときに、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合、ユーザーに通知します。</li></ul> |
| `subscriptions` | アラートに関連付けられたアドビ登録メール ID と、ユーザーがアラートを受け取るチャネルを渡すために使用されるオブジェクト。 |
| `subscriptions.inContextNotifications` | アラートの UI 通知に登録しているユーザーのアドビ登録メールアドレスの配列。 |
| `subscriptions.emailNotifications` | アラートのメール受信を申し込んだユーザーのアドビ登録メールアドレスの配列。 |

## ユーザーが購読しているすべてのアラートのリストを取得 {#get-alert-subscription-list}

`/alert-subscriptions/user-subscriptions/{EMAIL_ID}` エンドポイントに対して GET リクエストを行って、ユーザーが購読しているすべてのアラートのリストを取得します。応答には、アラート名、ID、ステータス、アラートタイプ、通知チャネルが含まれます。

**API 形式**

```http
GET /alert-subscriptions/user-subscriptions/{EMAIL_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{EMAIL_ID}` | Adobe アカウントに登録されているメールアドレスは、アラートを購読しているユーザーを識別するために使用されます。 |
| `orderby` | 結果の順序を指定するフィールド。 サポートされているフィールドは `created` と `updated` です。プロパティ名の前にを追加します。 `+` ( 昇順と `-` 降順。 デフォルトは `-created` です。なお、プラス記号 (`+`) はでエスケープする必要があります `%2B`. 例： `%2Bcreated` は、昇順で作成される順序の値です。 |
| `pagesize` | ページごとに API 呼び出しから取得するレコードの数を制御するには、このパラメーターを使用します。 デフォルトの制限は、1 ページあたり最大 50 レコード数に設定されています。 |
| `page` | レコードを表示する、返された結果のページ番号を指定します。 |
| `property` | 選択したフィールドに基づいて結果をフィルターします。 フィルターは HTML エスケープする&#x200B;**必要があります**。複数のフィルターのセットを組み合わせるには、コンマを使用します。次のプロパティを使用してフィルタリングを実行できます。 <ul><li>ID</li><li>assetId</li><li>status</li><li>alertType</li></ul> サポートされる演算子は次のとおりです。 `==` （等しい）。 例： `id==6ebd9c2d-494d-425a-aa91-24033f3abeec` は、一致する ID を持つアラートを返します。 |

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

正常な応答は、HTTP ステータス 200 と、指定した `emailId` が購読しているアラートの詳細を含む `items` 配列が返されます。

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
| `name` | アラート名。この名前は、アラートサービスによって生成され、アラートダッシュボードで使用されます。アラート名は、アラートを保存するフォルダー、`alertType`、フロー ID で構成されます。 使用可能なアラートに関する情報は、[Platform のアラートダッシュボードドキュメント](../../observability/alerts/ui.md)を参照してください。 |
| `assetId` | アラートを特定のクエリに関連付けたクエリ ID。 |
| `status` | アラートのステータス値は、`enabled`、`enabling`、`disabled`、`disabling` の 4 つです。アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま将来利用するために一時停止されているか、または、これらのステート間を移行中です。 |
| `alertType` | アラートのタイプ。1 つのアラートに対して、次の 3 つの値が考えられす。 <ul><li>`start`：クエリの実行が開始されたときに、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合、ユーザーに通知します。</li></ul> |
| `subscriptions` | アラートに関連付けられたアドビ登録メール ID と、ユーザーがアラートを受け取るチャネルを渡すために使用されるオブジェクト。 |
| `subscriptions.inContextNotifications` | ユーザーがアラート通知を受け取る方法を決定するブーリアン値。`true` 値は、UI を通じてアラートを提供する必要があることを確認します。`false` 値は、そのチャネルを通じてユーザーに通知されないことを確認します。 |
| `subscriptions.emailNotifications` | ユーザーがアラート通知を受け取る方法を決定するブーリアン値。`true` 値は、アラートをメールで提供する必要があることを確認します。`false` 値は、そのチャネルを通じてユーザーに通知されないことを確認します。 |

## アラートの作成とユーザーの購読 {#subscribe-users}

アラートを作成し、ユーザーが購読して受信するには、`/alert-subscriptions` エンドポイントに対して `POST` リクエストを行います。このリクエストは、`assetId` プロパティを使用して新しく作成されたアラートにクエリを関連付け、`emailIds` を使用してそのクエリのアラートをユーザーが購読できるようにします。

>[!IMPORTANT]
>
>1 回のリクエストで最大 5 個のアドビ登録メール ID を渡すことができます。1 つのアラートに対して 5 人以上のユーザーを登録するには、後続のリクエストを行う必要があります。

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
| `assetId` | アラートはこの ID に関連付けられています。ID は、クエリ ID またはスケジュール ID のどちらかです。 |
| `alertType` | アラートのタイプ。1 つのアラートに対して、次の 3 つの値が考えられす。 <ul><li>`start`：クエリの実行が開始されたときに、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合、ユーザーに通知します。</li></ul> |
| `subscriptions` | アラートに関連付けられたアドビ登録メール ID と、ユーザーがアラートを受け取るチャネルを渡すために使用されるオブジェクト。 |
| `subscriptions.emailIds` | アラートを受信する必要のあるユーザーを識別するメールアドレスの配列。 メールアドレスは&#x200B;**必ず** Adobe アカウントに登録されている必要があります。 |
| `subscriptions.inContextNotifications` | ユーザーがアラート通知を受け取る方法を決定するブーリアン値。`true` 値は、UI を通じてアラートを提供する必要があることを確認します。`false` 値は、そのチャネルを通じてユーザーに通知されないことを確認します。 |
| `subscriptions.emailNotifications` | ユーザーがアラート通知を受け取る方法を決定するブーリアン値。`true` 値は、アラートをメールで提供する必要があることを確認します。`false` 値は、そのチャネルを通じてユーザーに通知されないことを確認します。 |

{style="table-layout:auto"}

**応答**

正常な応答は HTTP ステータス 202（許可済み）とともに、新たに作成されたクエリの詳細を返します。

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
| `id` | アラート名。この名前は、アラートサービスによって生成され、アラートダッシュボードで使用されます。アラート名は、アラートを保存するフォルダー、`alertType`、フロー ID で構成されます。 使用可能なアラートに関する情報は、[Platform のアラートダッシュボードドキュメント](../../observability/alerts/ui.md)を参照してください。 |
| `_links` | このアラート ID に関連する情報の取得、更新、編集、削除に使用できる、使用可能なメソッドおよびエンドポイントに関する情報を提供します。 |

## アラートの有効／無効を切り替える {#enable-or-disable-alert}

このリクエストは、クエリ ID またはスケジュール ID とアラートタイプを使用して特定のアラートを参照し、アラートステータスを `enable` または `disable` に更新します。アラートのステータスは、`/alert-subscriptions/{queryId}/{alertType}` または `/alert-subscriptions/{scheduleId}/{alertType}` エンドポイントに対して `PATCH` リクエストを行います。

**API 形式**

```http
PATCH /alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}
PATCH /alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `ALERT_TYPE` | アラートのタイプ。1 つのアラートに対して、次の 3 つの値が考えられす。 <ul><li>`start`：クエリの実行が開始されたときに、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合にユーザーに通知します。</li></ul>変更するには、エンドポイントの名前空間で現在のアラートタイプを指定する必要があります。 |
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
| `op` | 実行する操作。現在、許可されている値は `replace` のみです。 |
| `path` | この値は、エンドポイントの名前空間に関連します。現在、許可されている値は `/status` のみです。 |
| `value` | 正常に行われた PATCH リクエストでは、これはアラートの `status` 値に変更されます。現在、指定できる値は `enable` または `disable` です。 |

{style="table-layout:auto"}

**応答**

正常に行われた応答は、HTTP ステータス 200 と、アラートのステータス、タイプ、ID の詳細および関連するクエリを返します。

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
| `id` | アラート名。この名前は、アラートサービスによって生成され、アラートダッシュボードで使用されます。アラート名は、アラートを保存するフォルダー、`alertType`、フロー ID で構成されます。 使用可能なアラートに関する情報は、[Platform のアラートダッシュボードドキュメント](../../observability/alerts/ui.md)を参照してください。 |
| `assetId` | アラートはこの ID に関連付けられています。ID は、クエリ ID またはスケジュール ID のどちらかです。 |
| `alertType` | 各アラートは、3 つの異なるアラートタイプを持つことができます。 次のとおりです。 <ul><li>`start`：クエリの実行が開始されると、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合にユーザーに通知します。</li></ul> |
| `status` | アラートのステータス値は、`enabled`、`enabling`、`disabled`、`disabling` の 4 つです。アラートは、イベントをアクティブにリッスンしているか、関連するすべての購読者と設定を保持したまま将来利用するために一時停止されているか、または、これらのステート間を移行中です。 |

## 特定のクエリおよびアラートタイプのアラートの削除 {#delete-alert-info-by-id-and-alert-type}

`/alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}` または `/alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}` エンドポイントに DELETE リクエストを行い、特定のクエリまたはスケジュール ID およびアラートタイプのアラートを削除できます。

```http
DELETE /alert-subscriptions/{QUERY_ID}/{ALERT_TYPE}
DELETE /alert-subscriptions/{SCHEDULE_ID}/{ALERT_TYPE}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `ALERT_TYPE` | アラートのタイプ。1 つのアラートに対して、次の 3 つの値が考えられす。 <ul><li>`start`：クエリの実行が開始されたときに、ユーザーに通知します。</li><li>`success`：クエリが完了すると、ユーザーに通知します。</li><li>`failure`：クエリが失敗した場合にユーザーに通知します。</li></ul> DELETE リクエストは、指定された特定のアラートタイプにのみ適用されます。 |
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

応答が成功すると、HTTP 200 ステータスと、アセット ID と削除されたアラートのアラートタイプを含む確認メッセージが返されます。

```json
{
"message": "Alert Deleted Successfully for assetId: 6df22232-f427-4250-a4e1-43cd30990641 and alertType: success",
"statusCode": 200
}
```

## 次の手順

このガイドでは、Query Service API の `/alert-subscriptions` エンドポイントの使用について説明しました。このガイドを読むことで、クエリのアラートの作成、ユーザーのアラートへの登録、使用可能なアラートのタイプ、アラート購読情報の取得、更新、削除方法に関する理解が深まりました。

詳しくは、[Query Service API ガイド](./getting-started.md)を参照して、その他の使用可能な機能や操作をご確認ください。
