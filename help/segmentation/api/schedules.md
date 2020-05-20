---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スケジュール
topic: developer guide
translation-type: tm+mt
source-git-commit: 45a196d13b50031d635ceb7c5c952e42c09bd893
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 4%

---


# スケジュール開発ガイド

導入

- スケジュールのリストの取得
- 新しいスケジュールの作成
- 特定のスケジュールの取得
- 特定のスケジュールの更新
- 特定のスケジュールの削除

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメント化APIの一部です。 先に進む前に、 [セグメント化開発ガイドを参照してください](./getting-started.md)。

特に、セグメント化開発ガイドの「 [はじめに](./getting-started.md#getting-started) 」の節には、関連トピックへのリンク、ドキュメント内のサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## スケジュールのリストの取得

エンドポイントにGETリクエストを行うことで、IMS組織のすべてのスケジュールのリストを取得でき `/config/schedules` ます。

**API形式**

```http
GET /config/schedules
GET /config/schedules?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`: (*オプション*)リクエストパスに追加されるパラメーター。応答で返される結果を設定します。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。 使用可能なパラメーターを以下に示します。

**クエリパラメーター**

次に、集計表を一覧表示する際に使用できるクエリパラメータのリストを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュールが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `start` | オフセットの開始元となるページを指定します。 デフォルトでは、この値は0です。 |
| `limit` | 返されるスケジュールの数を指定します。 デフォルトでは、この値は100になります。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules?limit=X \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたIMS組織のスケジュールのリストを持つHTTPステータス200をJSONとして返します。

```json
{
    "_page": {
        "totalCount": 1,
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

## 新しいスケジュールの作成

エンドポイントにPOSTリクエストを作成して、新しいスケジュールを作成でき `/config/schedules` ます。

**API形式**

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
  "name": "profile-default",
  "type": "batch_segmentation",
  "properties": {
    "segments": [
      "*"
    ]
  },
  "schedule": "0 0 1 * * ?",
  "state": "inactive"
}
 '
```

| パラメーター | 説明 |
| --------- | ------------ |
| `name` | **必須。** スケジュールの名前を文字列で指定します。 |
| `type` | **必須。** 文字列としてのジョブのタイプ。 サポートされる2つのタイプは `batch_segmentation` とで `export`す。 |
| `properties` | **必須。** 集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **が次の値と等しい場合`type`は必須`batch_segmentation`です。** を使用すると、すべてのセグメントが確実に含まれます。 `["*"]` |
| `schedule` | **必須。** ジョブスケジュールを含む文字列。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 この例で、「0 0 1 * *」は、このスケジュールが毎月1日の午前0時に実行されることを意味します。 |
| `state` | *オプション.* スケジュールの状態を含む文字列。 サポートされる2つの状態は、 `active` およびで `inactive`す。 By default, the state is set to `inactive`. |

**応答**

正常に応答すると、HTTPステータス200が返され、新しく作成したスケジュールの詳細が表示されます。

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

## 特定のスケジュールの取得

エンドポイントにGETリクエストを送信し、リクエストパスにスケジュールの `/config/schedules``id` 値を指定することで、特定のスケジュールに関する詳細な情報を取得できます。

**API形式**

```http
GET /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: 取得するスケジュールの `id` 値。

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID}
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTPステータス200が返され、指定されたスケジュールに関する詳細情報が返されます。

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
```

## 特定のスケジュールの詳細を更新する

エンドポイントにPATCHリクエストを送信し、リクエストパスにスケジュールの `/config/schedules``id` 値を指定することで、指定したスケジュールを更新できます。

PATCHリクエストは、次の2つの異なるパスをサポートします。 `/state` と `/schedule`。

### スケジュール状態の更新

を使用して、スケジュールの状態（ACTIVEまたはINACTIVE） `/state` を更新できます。 状態を更新するには、値をまたはに設定する必要があり `active` ま `inactive`す。

**API形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: 更新するスケジュールの `id` 値。

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
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
]
'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `path` | パッチを適用する値のパス。 この場合、スケジュールの状態を更新するので、の値をに設定する必要があ `path` り `/state`ます。 |
| `value` | の更新された値 `/state`。 この値は、スケジュールをアクティブ化または非アクティブ化す `active` るた `inactive` めに、またはとして設定できます。 |

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）が返されます。

### スケジュールcronスケジュールの更新

を使用して、Cronスケジュール `schedule` を更新できます。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。

**API形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: 更新するスケジュールの `id` 値。

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
[
  {
    "op": "add",
    "path": "/schedule",
    "value": "0 0 2 * *"
  }
]
'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `path` | パッチを適用する値のパス。 この場合、スケジュールのCronスケジュールを更新するので、の値をに設定する必要があ `path` り `/schedule`ます。 |
| `value` | の更新された値 `/state`。 この値は、Cronスケジュールの形式にする必要があります。 この例では、スケジュールは毎月2日に実行されます。 |

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）が返されます。

## 特定のスケジュールの削除

リクエストパスにDELETEリクエストを作成し、リクエストパスにスケジュールの `/config/schedules``id` 値を指定することで、指定したスケジュールを削除するようにリクエストできます。

**API形式**

```http
DELETE /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: 削除するスケジュールの `id` 値。

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）が返され、次のメッセージが表示されます。

```json
(No Content) Schedule deleted successfully
```

## 次の手順
