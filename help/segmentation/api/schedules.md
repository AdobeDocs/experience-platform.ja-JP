---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；スケジュール；スケジュール；api;API;
solution: Experience Platform
title: Schedules API エンドポイント
topic-legacy: developer guide
description: スケジュールは、1 日 1 回バッチセグメント化ジョブを自動的に実行するために使用できるツールです。
exl-id: 92477add-2e7d-4d7b-bd81-47d340998ff1
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 46%

---

# Schedules エンドポイント

スケジュールは、1 日 1 回バッチセグメント化ジョブを自動的に実行するために使用できるツールです。 以下を使用して、 `/config/schedules` エンドポイント：スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除をおこないます。

## はじめに

このガイドで使用する エンドポイントは、[!DNL Adobe Experience Platform Segmentation Service]API の一部です。続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## スケジュールのリストの取得 {#retrieve-list}

IMS 組織のすべてのスケジュールのリストを取得するには、`/config/schedules` エンドポイントに GET リクエストを作成します。

**API 形式**

`/config/schedules` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュールが取得されます。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /config/schedules
GET /config/schedules?start={START}
GET /config/schedules?limit={LIMIT}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{START}` | オフセットの元となるページを開始します。デフォルトでは、この値は 0 です。 |
| `{LIMIT}` | 返されるスケジュールの数を指定します。デフォルトでは、この値は 100 です。 |

**リクエスト**

次のリクエストでは、IMS 組織内で投稿された最新 10 件のスケジュールを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules?limit=10 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、JSON として指定された IMS 組織のスケジュールのリストと共に HTTP ステータス 200 が返されます。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられ、最初に返されたスケジュールのみが表示されます。

```json
{
    "_page": {
        "totalCount": 10,
        "pageSize": 1
    },
    "children": [
        {
            "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
            "imsOrgId": "{IMS_ORG}",
            "sandbox": {
                "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "name": "Batch Segmentation",
            "state": "active",
            "type": "batch_segmentation",
            "schedule": "0 0 1 * * ?",
            "properties": {
                "segments": []
            },
            "createEpoch": 1573158851,
            "updateEpoch": 1574365202
        }
    ],
    "_links": {
        "next": {}
    }
}
```

| プロパティ | 説明 |
| -------- | ------------ |
| `_page.totalCount` | 返されたスケジュールの合計数。 |
| `_page.pageSize` | スケジュールのページのサイズ。 |
| `children.name` |  文字列としてのスケジュールの名前。 |
| `children.type` |  文字列としてのジョブのタイプ。サポートされているタイプは、「batch_segmentation」と「export」の 2 つです。 |
| `children.properties` |  スケジュールに関連する追加のプロパティを含むオブジェクトです。 |
| `children.properties.segments` | `["*"]` を使用すると、すべてのセグメントが確実に含まれます。 |
| `children.schedule` |  ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。 Cron スケジュールの詳細については、[Cron 式形式のドキュメント](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「0 0 1 * *」は、このスケジュールが毎月 1 日午前 0 時に実行されることを意味します。 |
| `children.state` |  スケジュールの状態を含む文字列。サポートされている 2 つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

## 新しいスケジュールの作成 {#create}

`/config/schedules` エンドポイントに POST リクエストをおこなうことで、新しいスケジュールを作成できます。

**API 形式**

```http
POST /config/schedules
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/config/schedules \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
{
    "name":"profile-default",
    "type":"batch_segmentation",
    "properties":{
        "segments":[
            "*"
        ]
    },
    "schedule":"0 0 1 * * ?",
    "state":"inactive"
}'
```

| プロパティ | 説明 |
| -------- | ------------ |
| `name` | **必須。** 文字列としてのスケジュールの名前。 |
| `type` | **必須。** 文字列としてのジョブのタイプ。サポートされているタイプは、「batch_segmentation」と「export」の 2 つです。 |
| `properties` | **必須。** スケジュールに関連する追加のプロパティを含むオブジェクトです。 |
| `properties.segments` | **必須の場合 `type` は、「batch_segmentation」と等しいです。**`["*"]` を使用すると、すべてのセグメントが確実に含まれます。 |
| `schedule` | *オプション。* ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。 Cron スケジュールの詳細については、[cron 式形式のドキュメント](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「0 0 1 * *」は、このスケジュールが毎月 1 日午前 0 時に実行されることを意味します。<br><br>この文字列を指定しない場合、システム生成スケジュールが自動的に生成されます。 |
| `state` | *オプション。* スケジュールの状態を含む文字列。サポートされている 2 つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

**応答**

正常な応答では、新しく作成したスケジュールの詳細と共に HTTP ステータス 200 が返されます。

```json
{
    "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "e7e17720-c5bb-11e9-aafb-87c71c35cac8",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "{SCHEDULE_NAME}",
    "state": "inactive",
    "type": "batch_segmentation",
    "schedule": "0 0 1 * * ?",
    "properties": {
        "segments": [
            "*"
        ]
    },
    "createEpoch": 1568267948,
    "updateEpoch": 1568267948
}
```

## 特定のスケジュールの取得 {#get}

特定のスケジュールに関する詳細な情報を取得するには、 `/config/schedules` エンドポイントを検索し、リクエストパスで取得するスケジュールの ID を指定します。

**API 形式**

```http
GET /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | この `id` 取得するスケジュールの値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、指定されたスケジュールの詳細情報と共に HTTP ステータス 200 が返されます。

```json
{
    "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "e7e17720-c5bb-11e9-aafb-87c71c35cac8",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "{SCHEDULE_NAME}",
    "state": "inactive",
    "type": "batch_segmentation",
    "schedule": "0 0 1 * * ?",
    "properties": {
        "segments": [
            "*"
        ]
    },
    "createEpoch": 1568267948,
    "updateEpoch": 1568267948
}
```

| プロパティ | 説明 |
| -------- | ------------ |
| `name` |  文字列としてのスケジュールの名前。 |
| `type` |  文字列としてのジョブのタイプ。サポートされているタイプは `batch_segmentation` と `export` の 2 つです。 |
| `properties` |  スケジュールに関連する追加のプロパティを含むオブジェクトです。 |
| `properties.segments` | `["*"]` を使用すると、すべてのセグメントが確実に含まれます。 |
| `schedule` |  ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。Cron スケジュールの詳細については、[Cron 式形式のドキュメント](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「0 0 1 * *」は、このスケジュールが毎月 1 日午前 0 時に実行されることを意味します。 |
| `state` |  スケジュールの状態を含む文字列。サポートされている 2 つの状態は、`active` と `inactive` です。デフォルトでは、状態は `inactive` に設定されています。 |

## 特定のスケジュールの詳細を更新 {#update}

特定のスケジュールを更新するには、 `/config/schedules` エンドポイントを開き、リクエストパスで更新しようとしているスケジュールの ID を指定します。

PATCHリクエストでは、 [state](#update-state) または [cron スケジュール](#update-schedule) 個々のスケジュールに対して。

### スケジュール状態の更新 {#update-state}

JSON パッチ操作を使用して、スケジュールの状態を更新できます。 状態を更新するには、 `path` プロパティ `/state` そして、 `value` どちらか `active` または `inactive`. JSON パッチの詳細については、 [JSON パッチ](https://datatracker.ietf.org/doc/html/rfc6902) ドキュメント。

**API 形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | この `id` 更新するスケジュールの値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
[
    {
        "op": "add",
        "path": "/state",
        "value": "active"
    }
]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `path` | パッチを適用する値のパス。この場合、スケジュールの状態を更新するので、の値を設定する必要があります。 `path` を&quot;/state&quot;に変更します。 |
| `value` | スケジュールの状態の更新された値。 この値は、「アクティブ」または「非アクティブ」に設定して、スケジュールをアクティブ化または非アクティブ化できます。 |

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

### Cron スケジュールを更新 {#update-schedule}

JSON パッチ操作を使用して、cron スケジュールを更新できます。 スケジュールを更新するには、 `path` プロパティ `/schedule` そして、 `value` を有効な cron スケジュールに追加します。 JSON パッチの詳細については、 [JSON パッチ](https://datatracker.ietf.org/doc/html/rfc6902) ドキュメント。 cron スケジュールの詳細については、[cron 式形式のドキュメント](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。

**API 形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | この `id` 更新するスケジュールの値。 |

**リクエスト**

```shell
curl -X PATCH https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
[
    {
        "op":"add",
        "path":"/schedule",
        "value":"0 0 2 * *"
    }
]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `path` | 更新する値のパス。 この場合、Cron スケジュールを更新するので、の値を設定する必要があります。 `path` から `/schedule`. |
| `value` | Cron スケジュールの更新された値。 この値は、Cron スケジュールの形式で指定する必要があります。この例では、スケジュールは毎月 2 日に実行されます。 |

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

## 特定のスケジュールの削除

特定のスケジュールの削除をリクエストするには、 `/config/schedules` エンドポイントを探し、リクエストパスで削除するスケジュールの ID を指定します。

**API 形式**

```http
DELETE /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | この `id` 削除するスケジュールの値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

## 次の手順

このガイドを読むと、スケジュールの動作をより深く理解できます。
