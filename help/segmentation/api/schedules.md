---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スケジュール
topic: developer guide
translation-type: tm+mt
source-git-commit: b3e6a6f1671a456b2ffa61139247c5799c495d92
workflow-type: tm+mt
source-wordcount: '1171'
ht-degree: 3%

---


# スケジュールエンドポイント

スケジュールは、バッチセグメントジョブを1日1回自動的に実行するために使用できるツールです。 エンドポイントを使用して、スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除を行うことができます。 `/config/schedules`

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、 [入門ガイドを参照して](./getting-started.md) 、必要なヘッダーやAPI呼び出し例を読む方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## スケジュールのリストの取得 {#retrieve-list}

エンドポイントにGETリクエストを行うことで、IMS組織のすべてのスケジュールのリストを取得でき `/config/schedules` ます。

**API形式**

エンドポイントでは、結果のフィルタリングに役立ついくつかのクエリパラメーターが `/config/schedules` サポートされています。 これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュールが取得されます。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。

```http
GET /config/schedules
GET /config/schedules?start={START}
GET /config/schedules?limit={LIMIT}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{START}` | オフセットの開始元となるページを指定します。 デフォルトでは、この値は0です。 |
| `{LIMIT}` | 返されるスケジュールの数を指定します。 デフォルトでは、この値は100になります。 |

**リクエスト**

次のリクエストは、IMS組織内で投稿された最新10件のスケジュールを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules?limit=10 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたIMS組織のスケジュールのリストを持つHTTPステータス200をJSONとして返します。

>[!NOTE] 次の応答は領域のために切り捨てられ、最初に返されたスケジュールのみを表示します。

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
| `children.name` | スケジュールの名前を文字列で指定します。 |
| `children.type` | 文字列としてのジョブのタイプ。 サポートされる2つのタイプは、「batch_segmentation」と「export」です。 |
| `children.properties` | 集計表に関連する追加のプロパティを含むオブジェクト。 |
| `children.properties.segments` | を使用すると、すべてのセグメントが確実に含まれます。 `["*"]` |
| `children.schedule` | ジョブスケジュールを含む文字列。 ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間の間にジョブを2回以上実行するようにスケジュールすることはできません。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 この例で、「0 0 1 * *」は、このスケジュールが毎月1日の午前0時に実行されることを意味します。 |
| `children.state` | スケジュールの状態を含む文字列。 サポートされる2つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

## Create a new schedule {#create}

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
| `name` | **必須。** スケジュールの名前を文字列で指定します。 |
| `type` | **必須。** 文字列としてのジョブのタイプ。 サポートされる2つのタイプは、「batch_segmentation」と「export」です。 |
| `properties` | **必須。** 集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | **「batch_segmentation」`type`と等しい場合に必須です。** を使用すると、すべてのセグメントが確実に含まれます。 `["*"]` |
| `schedule` | *オプション.* ジョブスケジュールを含む文字列。 ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間の間にジョブを2回以上実行するようにスケジュールすることはできません。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 この例で、「0 0 1 * *」は、このスケジュールが毎月1日の午前0時に実行されることを意味します。 <br><br>この文字列を指定しない場合、システム生成スケジュールが自動的に生成されます。 |
| `state` | *オプション.* スケジュールの状態を含む文字列。 サポートされる2つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

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

## 特定のスケジュールの取得 {#get}

特定のスケジュールに関する詳細な情報を取得するには、エンドポイントにGETリクエストを送信し、リクエストパスに取得するスケジュールのIDを指定し `/config/schedules` ます。

**API形式**

```http
GET /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュールの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b
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
}
```

| プロパティ | 説明 |
| -------- | ------------ |
| `name` | スケジュールの名前を文字列で指定します。 |
| `type` | 文字列としてのジョブのタイプ。 サポートされる2つのタイプは `batch_segmentation` とで `export`す。 |
| `properties` | 集計表に関連する追加のプロパティを含むオブジェクト。 |
| `properties.segments` | を使用すると、すべてのセグメントが確実に含まれます。 `["*"]` |
| `schedule` | ジョブスケジュールを含む文字列。 ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間にジョブを2回以上実行するようにスケジュールすることはできません。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。 この例で、「0 0 1 * *」は、このスケジュールが毎月1日の午前0時に実行されることを意味します。 |
| `state` | スケジュールの状態を含む文字列。 サポートされる2つの状態は、 `active` およびで `inactive`す。 By default, the state is set to `inactive`. |

## 特定のスケジュールの詳細を更新する {#update}

エンドポイントにPATCHリクエストを送信し、更新しようとしているスケジュールのIDを要求パスに指定することで、特定のスケジュールを更新でき `/config/schedules` ます。

PATCHリクエストを使用すると、 [状態](#update-state) または [cronスケジュールを個々のスケジュールに対して更新できます](#update-schedule) 。

### スケジュール状態の更新 {#update-state}

「JSONパッチ」操作を使用して、スケジュールの状態を更新できます。 状態を更新するには、 `path` プロパティをと宣言し、を `/state` またはに設定し `value``active``inactive`ます。 JSONパッチの詳細については、 [JSONパッチのドキュメントを読んでください](http://jsonpatch.com/) 。

**API形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 更新するスケジュールの `id` 値。 |

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
| `path` | パッチを適用する値のパス。 この場合、スケジュールの状態を更新するので、の値を「/state」に設定する必要 `path` があります。 |
| `value` | スケジュールの状態の更新された値。 この値を「active」または「inactive」に設定して、スケジュールをアクティブ化または非アクティブ化できます。 |

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）が返されます。

### Cronスケジュールの更新 {#update-schedule}

「JSONパッチ」操作を使用してCronスケジュールを更新できます。 集計表を更新するには、 `path` プロパティをと宣言し、を有効なCron集計表 `/schedule``value` に設定します。 JSONパッチの詳細については、 [JSONパッチのドキュメントを読んでください](http://jsonpatch.com/) 。 cronスケジュールの詳細については、cron式形式のドキュメントを参照して [ください](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) 。

**API形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 更新するスケジュールの `id` 値。 |

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
| `path` | 更新する値のパス。 この場合、Cronスケジュールを更新するので、の値をに設定する必要があ `path` り `/schedule`ます。 |
| `value` | Cronスケジュールの更新された値。 この値は、Cronスケジュールの形式にする必要があります。 この例では、スケジュールは毎月2日に実行されます。 |

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）が返されます。

## 特定のスケジュールの削除

エンドポイントにDELETEリクエストを送信し、削除するスケジュールのIDをリクエストパスに指定することで、特定のスケジュールを削除するようにリクエストでき `/config/schedules` ます。

**API形式**

```http
DELETE /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 削除するスケジュールの `id` 値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）が返されます。

## 次の手順

このガイドを読むと、スケジュールの動作に関する理解が深まります。