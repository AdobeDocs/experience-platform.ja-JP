---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；スケジュール；スケジュール；api;API;
solution: Experience Platform
title: APIエンドポイントのスケジュール
topic-legacy: developer guide
description: スケジュールは、バッチセグメントジョブを1日1回自動的に実行するために使用できるツールです。
exl-id: 92477add-2e7d-4d7b-bd81-47d340998ff1
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1203'
ht-degree: 46%

---

# スケジュールエンドポイント

スケジュールは、バッチセグメントジョブを1日1回自動的に実行するために使用できるツールです。 `/config/schedules`エンドポイントを使用して、スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除を行うことができます。

## はじめに

このガイドで使用されるエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、[はじめにガイド](./getting-started.md)を見て、必要なヘッダーやAPI呼び出し例の読み方など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## スケジュールのリストの取得 {#retrieve-list}

IMS 組織のすべてのスケジュールのリストを取得するには、`/config/schedules` エンドポイントに GET リクエストを作成します。

**API 形式**

`/config/schedules`エンドポイントは、結果のフィルタリングに役立ついくつかのクエリパラメーターをサポートしています。 これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのスケジュールが取得されます。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

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

次のリクエストは、IMS組織内で投稿された最新10件のスケジュールを取得します。

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
>次の応答は領域のために切り捨てられ、最初に返されたスケジュールのみを表示します。

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
| `children.type` |  文字列としてのジョブのタイプ。サポートされる2つのタイプは、「batch_segmentation」と「export」です。 |
| `children.properties` |  スケジュールに関連する追加のプロパティを含むオブジェクトです。 |
| `children.properties.segments` | `["*"]` を使用すると、すべてのセグメントが確実に含まれます。 |
| `children.schedule` |  ジョブスケジュールを含む文字列。ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間の間にジョブを2回以上実行するようにスケジュールすることはできません。 Cron スケジュールの詳細については、[Cron 式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「0 0 1 * *」は、このスケジュールが毎月 1 日午前 0 時に実行されることを意味します。 |
| `children.state` |  スケジュールの状態を含む文字列。サポートされる2つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

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
| `type` | **必須。** 文字列としてのジョブのタイプ。サポートされる2つのタイプは、「batch_segmentation」と「export」です。 |
| `properties` | **必須。** スケジュールに関連する追加のプロパティを含むオブジェクトです。 |
| `properties.segments` | **「batch_segmentation」 `type` と等しい場合に必須です。**`["*"]` を使用すると、すべてのセグメントが確実に含まれます。 |
| `schedule` | *オプション。* ジョブスケジュールを含む文字列。ジョブは、1日に1回しか実行するようにスケジュールできません。つまり、24時間の間にジョブを2回以上実行するようにスケジュールすることはできません。 Cron スケジュールの詳細については、[cron 式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「0 0 1 * *」は、このスケジュールが毎月 1 日午前 0 時に実行されることを意味します。<br><br>この文字列を指定しない場合、システム生成スケジュールが自動的に生成されます。 |
| `state` | *オプション。* スケジュールの状態を含む文字列。サポートされる2つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

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

## 特定のスケジュールの取得  {#get}

`/config/schedules`エンドポイントにGETリクエストを送信し、取得するスケジュールのIDをリクエストパスに指定することで、特定のスケジュールに関する詳細な情報を取得できます。

**API 形式**

```http
GET /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 取得するスケジュールの`id`値。 |

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
| `schedule` |  ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。Cron スケジュールの詳細については、[Cron 式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。この例では、「0 0 1 * *」は、このスケジュールが毎月 1 日午前 0 時に実行されることを意味します。 |
| `state` |  スケジュールの状態を含む文字列。サポートされている 2 つの状態は、`active` と `inactive` です。デフォルトでは、状態は `inactive` に設定されています。 |

## 特定のスケジュールの詳細を更新{#update}

`/config/schedules`エンドポイントにPATCHリクエストを送信し、更新しようとしているスケジュールのIDをリクエストパスに指定することで、特定のスケジュールを更新できます。

PATCHリクエストを使用すると、個々のスケジュールの[state](#update-state)または[cronスケジュール](#update-schedule)を更新できます。

### スケジュール状態の更新 {#update-state}

「JSONパッチ」操作を使用して、スケジュールの状態を更新できます。 状態を更新するには、`path`プロパティを`/state`として宣言し、`value`を`active`または`inactive`に設定します。 JSONパッチの詳細については、[JSONパッチ](http://jsonpatch.com/)のドキュメントをお読みください。

**API 形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 更新するスケジュールの`id`値。 |

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
| `path` | パッチを適用する値のパス。この場合、スケジュールの状態を更新するので、`path`の値を「/state」に設定する必要があります。 |
| `value` | スケジュールの状態の更新された値。 この値を「active」または「inactive」に設定して、スケジュールをアクティブ化または非アクティブ化できます。 |

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

### cronスケジュールの更新{#update-schedule}

「JSONパッチ」操作を使用してCronスケジュールを更新できます。 スケジュールを更新するには、`path`プロパティを`/schedule`として宣言し、`value`を有効なcronスケジュールに設定します。 JSONパッチの詳細については、[JSONパッチ](http://jsonpatch.com/)のドキュメントをお読みください。 cron スケジュールの詳細については、[cron 式形式のドキュメント](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)を参照してください。

**API 形式**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 更新するスケジュールの`id`値。 |

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
| `path` | 更新する値のパス。 この場合、cronスケジュールを更新するので、`path`の値を`/schedule`に設定する必要があります。 |
| `value` | Cronスケジュールの更新された値。 この値は、Cron スケジュールの形式で指定する必要があります。この例では、スケジュールは毎月 2 日に実行されます。 |

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

## 特定のスケジュールの削除

`/config/schedules`エンドポイントにDELETEリクエストを送信し、削除するスケジュールのIDをリクエストパスに指定することで、特定のスケジュールの削除をリクエストできます。

**API 形式**

```http
DELETE /config/schedules/{SCHEDULE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SCHEDULE_ID}` | 削除するスケジュールの`id`値。 |

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

このガイドを読むと、スケジュールの動作に関する理解が深まります。
