---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スケジュール
topic: developer guide
translation-type: tm+mt
source-git-commit: 45a196d13b50031d635ceb7c5c952e42c09bd893

---


# スケジュール開発ガイド

導入

- スケジュールのリストの取得
- 新しいスケジュールの作成
- 特定のスケジュールの取得
- 特定のスケジュールの更新
- 特定のスケジュールの削除

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメントAPIの一部です。 先に進む前に、セグメント化開発ガイ [ドを参照してください](./getting-started.md)。

特に、『セグメン [ト開発者](./getting-started.md#getting-started) 』ガイドの「はじめに」の節には、関連トピックへのリンク、ドキュメントでのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## スケジュールのリストの取得

IMS組織のすべてのスケジュールのリストを取得するには、エンドポイントにGETリクエストを作成 `/config/schedules` します。

**API形式**

```http
GET /config/schedules
GET /config/schedules?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`:(オ&#x200B;*プション*)応答に返される結果を設定する要求パスに追加されるパラメーター。 複数のパラメーターを含め、アンパサンド(`&`)で区切ることができます。 使用可能なパラメーターを以下に示します。

**クエリパラメータ**

次に、集計表を一覧表示するために使用できるクエリパラメータのリストを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュールが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `start` | オフセットの元となるページを開始します。 デフォルトでは、この値は0です。 |
| `limit` | 返されるスケジュールの数を指定します。 デフォルトでは、この値は100です。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules?limit=X \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したIMS組織のスケジュールのリストをJSONとしてHTTPステータス200を返します。

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

エンドポイントにPOSTリクエストを行うことで、新しいスケジュールを作成で `/config/schedules` きます。

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
| `name` | **必須.** 文字列としてのスケジュールの名前。 |
| `type` | **必須.** 文字列としてのジョブのタイプ。 サポートされているタイプはとの2 `batch_segmentation` つで `export`す。 |
| `properties` | **必須.** 集計表に関連する追加のプロパティを含むオブジェクトです。 |
| `properties.segments` | **が次の値に等しい場`type`合は必須で`batch_segmentation`す。** を使用すると、 `["*"]` すべてのセグメントが確実に含まれます。 |
| `schedule` | **必須.** ジョブスケジュールを含む文字列。 cronスケジュールの詳細については、Cron式形式のドキュメントを [参照し](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 、 この例では、「0 0 1 * *」は、このスケジュールが毎月1日午前0時に実行されることを意味します。 |
| `state` | *オプション.* スケジュールの状態を含む文字列。 サポートされている2つの状態は、と `active` です `inactive`。 By default, the state is set to `inactive`. |

**応答**

正常に応答すると、新しく作成したスケジュールの詳細と共にHTTPステータス200が返されます。

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

特定のスケジュールに関する詳細な情報を取得するには、エンドポイントにGETリクエストを送信し、 `/config/schedules` リクエストパスにスケジュールの値を `id` 指定して、そのスケジュールの値を取得します。

**API形式**

```http
GET /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`:取得 `id` するスケジュールの値。

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID}
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200を返し、指定されたスケジュールに関する詳細情報が返されます。

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

エンドポイントにPATCHリクエストを作成し、リクエストパスにスケジュ `/config/schedules` ールの値を指定することで、指定 `id` したスケジュールを更新できます。

PATCHリクエストは、次の2つの異なるパスをサポートします。 `/state` と `/schedule`

### スケジュール状態の更新

を使用して、スケジ `/state` ュールの状態（「アクティブ」または「非アクティブ」）を更新できます。 状態を更新するには、値をまたはに設定する必要があ `active` ります `inactive`。

**API形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`:更新 `id` するスケジュールの値。

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールの状態を更新するので、の値をに設定する必要があり `path` ます `/state`。 |
| `value` | の更新された値 `/state`。 この値は、スケジュールをアクティブ化または非アク `active` ティブ化す `inactive` るために、またはとして設定できます。 |

**応答**

応答が成功すると、HTTPステータス204 (No Content)が返されます。

### スケジュールcronスケジュールの更新

を使用して、Cronスケジ `schedule` ュールを更新することができます。 cronスケジュールの詳細については、Cron式形式のドキュメントを [参照し](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 、

**API形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`:更新 `id` するスケジュールの値。

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールのCronスケジュールを更新するので、の値をに設定する必要があり `path` ます `/schedule`。 |
| `value` | の更新された値 `/state`。 この値は、Cronスケジュールの形式で指定する必要があります。 この例では、スケジュールは毎月の2番目に実行されます。 |

**応答**

応答が成功すると、HTTPステータス204 (No Content)が返されます。

## 特定のスケジュールの削除

リクエストパスにDELETEリクエストを作成し、そのスケジュールの値を指定するこ `/config/schedules` とで、指定したスケジュー `id` ルの削除をリクエストできます。

**API形式**

```http
DELETE /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`:削除 `id` するスケジュールの値。

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、次のメッセージと共にHTTPステータス204 （コンテンツなし）が返されます。

```json
(No Content) Schedule deleted successfully
```

## 次の手順
