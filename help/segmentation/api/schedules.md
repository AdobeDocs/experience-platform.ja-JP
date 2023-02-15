---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；スケジュール；スケジュール；api;API;
solution: Experience Platform
title: Schedules API エンドポイント
description: スケジュールは、1 日 1 回バッチセグメント化ジョブを自動的に実行するために使用できるツールです。
exl-id: 92477add-2e7d-4d7b-bd81-47d340998ff1
source-git-commit: e24a2ba0321ebaa8e91f96477f58bfa4915f47ce
workflow-type: tm+mt
source-wordcount: '2011'
ht-degree: 23%

---

# Schedules エンドポイント

スケジュールは、1 日 1 回バッチセグメント化ジョブを自動的に実行するために使用できるツールです。 以下を使用して、 `/config/schedules` エンドポイント：スケジュールのリストの取得、新しいスケジュールの作成、特定のスケジュールの詳細の取得、特定のスケジュールの更新または特定のスケジュールの削除をおこないます。

## はじめに

このガイドで使用する エンドポイントは、[!DNL Adobe Experience Platform Segmentation Service]API の一部です。続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## スケジュールのリストの取得 {#retrieve-list}

組織のすべてのスケジュールのリストを取得するには、 `/config/schedules` endpoint.

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

次のリクエストでは、組織内で投稿された最新 10 件のスケジュールを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules?limit=10 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
            "imsOrgId": "{ORG_ID}",
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
| `children.schedule` |  ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。 Cron スケジュールの詳細については、 [cron 式形式](#appendix). この例では、「0 0 1 * *」は、このスケジュールが毎日午前 1 時に実行されることを意味します。 |
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
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `schedule` | *オプション。* ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。 Cron スケジュールの詳細については、 [cron 式形式](#appendix). この例では、「0 0 1 * *」は、このスケジュールが毎日午前 1 時に実行されることを意味します。 <br><br>この文字列を指定しない場合、システム生成スケジュールが自動的に生成されます。 |
| `state` | *オプション。* スケジュールの状態を含む文字列。サポートされている 2 つの状態は、「アクティブ」と「非アクティブ」です。 デフォルトでは、状態は「inactive」に設定されています。 |

**応答**

正常な応答では、新しく作成したスケジュールの詳細と共に HTTP ステータス 200 が返されます。

```json
{
    "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
    "imsOrgId": "{ORG_ID}",
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
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、指定されたスケジュールの詳細情報と共に HTTP ステータス 200 が返されます。

```json
{
    "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
    "imsOrgId": "{ORG_ID}",
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
| `schedule` |  ジョブスケジュールを含む文字列。ジョブは 1 日に 1 回のみ実行するようにスケジュールできます。つまり、24 時間の間に 2 回以上実行するようにジョブをスケジュールすることはできません。Cron スケジュールの詳細については、 [cron 式形式](#appendix). この例では、「0 0 1 * *」は、このスケジュールが毎日午前 1 時に実行されることを意味します。 |
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
curl -X PATCH https://platform.adobe.io/data/core/ups/config/schedules/4e538382-dbd8-449e-988a-4ac639ebe72b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `value` | スケジュールの状態の更新された値。 この値は、「アクティブ」または「非アクティブ」に設定して、スケジュールをアクティブ化または非アクティブ化できます。 なお、 **できません** IMS 組織がストリーミングを有効にしている場合、スケジュールを無効にします。 |

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

### Cron スケジュールを更新 {#update-schedule}

JSON パッチ操作を使用して、cron スケジュールを更新できます。 スケジュールを更新するには、 `path` プロパティ `/schedule` そして、 `value` を有効な cron スケジュールに追加します。 JSON パッチの詳細については、 [JSON パッチ](https://datatracker.ietf.org/doc/html/rfc6902) ドキュメント。 Cron スケジュールの詳細については、 [cron 式形式](#appendix).

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
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
[
    {
        "op":"add",
        "path":"/schedule",
        "value":"0 0 2 * * ?"
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
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、HTTP ステータス 204（コンテントなし）が返されます。

## 次の手順

このガイドを読むと、スケジュールの動作をより深く理解できます。

## 付録 {#appendix}

次の付録では、スケジュールで使用される Cron 式の形式について説明します。

### 形式

Cron 式は、6 つまたは 7 つのフィールドで構成される文字列です。 式は次のようになります。

`0 0 12 * * ?`

Cron 式文字列の場合、最初のフィールドは秒を表し、2 番目のフィールドは分を表し、3 番目のフィールドは時間を表し、4 番目のフィールドは月の日を表し、5 番目のフィールドは月を表し、6 番目のフィールドは曜日を表します。 また、年を表す 7 番目のフィールドを含めることもできます。

| フィールド名 | 必須 | 可能な値 | 許可される特殊文字 |
| ---------- | -------- | --------------- | -------------------------- |
| Seconds | ○ | 0-59 | `, - * /` |
| Minutes | ○ | 0-59 | `, - * /` |
| 時間 | ○ | 0 ～ 23 | `, - * /` |
| 月間通算日 | ○ | 1 ～ 31 | `, - * ? / L W` |
| 月 | ○ | 1 月～12 月 12 日 | `, - * /` |
| 曜日 | ○ | 1-7、日 — 土 | `, - * ? / L #` |
| 年 | いいえ | 空、1970-2099 年 | `, - * /` |

>[!NOTE]
>
>月の名前と曜日の名前は次のとおりです **not** 大文字と小文字を区別します。 したがって `SUN` は、 `sun`.

使用できる特殊文字は、次の意味を表します。

| 特殊文字 | 説明 |
| ----------------- | ----------- |
| `*` | この値は、 **すべて** の値を含める必要があります。 例： `*` 「時間」フィールドでは **毎** 時間。 |
| `?` | この値は、特定の値が必要でないことを意味します。 通常、これは文字が許可される 1 つのフィールドで何かを指定するために使用されますが、他のフィールドでは指定しません。 例えば、イベントを毎月 3 日にトリガーし、何曜日かは気にしない場合、 `3` （「日」フィールド）と `?` （曜日フィールド）。 |
| `-` | この値は、 **包括的** 範囲を指定します。 例えば、 `9-15` 「時間」フィールドで指定する時間は、9、10、11、12、13、14、15 です。 |
| `,` | この値は、追加の値を指定するために使用されます。 例えば、 `MON, FRI, SAT` 「曜日」フィールドで、曜日が月、金曜日、土曜日を含むことを意味します。 |
| `/` | この値は、増分を指定するために使用されます。 の前に配置された値 `/` は、増分元を決定します。一方、 `/` は、の増分量を決定します。 例えば、 `1/7` 「分」フィールドの場合、分には 1、8、15、22、29、36、43、50、57 が含まれます。 |
| `L` | この値は、 `Last`を含み、使用するフィールドに応じて異なる意味を持ちます。 日付フィールドと共に使用する場合は、月の最終日を表します。 単独で曜日フィールドを使用する場合は、週の最終日（土曜日）を表します (`SAT`) をクリックします。 曜日フィールドと共に使用する場合、別の値と組み合わせて、その月のそのタイプの最終日を表します。 例えば、 `5L` 曜日フィールドでは、 **のみ** 月の最後の金曜日を含めます。 |
| `W` | この値は、指定した日に最も近い曜日を指定するために使用されます。 例えば、 `18W` 「月の日」フィールドで、その月の 18 日が土曜日の場合、最も近い平日の 17 日（金曜日）がトリガーになります。 その月の 18 日が日曜日の場合は、月曜日の 19 日（最も近い平日）がトリガーになります。 注意： `1W` 「月の日」フィールドで指定し、最も近い平日が前月の場合、イベントは **現在** 月</br></br>さらに、 `L` および `W` 作る `LW`：月の最後の曜日を指定します。 |
| `#` | この値は、月の n 番目の曜日を指定するために使用されます。 の前に配置された値 `#` は曜日を表し、 `#` は、その月の発生を表します。 例えば、 `1#3`に設定した場合、イベントは月の 3 日曜日にトリガーします。 注意： `X#5` そして、その月には、その曜日の 5 番目の出現はない。 **not** をトリガーします。 例えば、 `1#5`そして、その月には第 5 の日曜日はないので、このイベントは **not** をトリガーします。 |

### 例

次の表に、cron 式文字列の例と、その意味を示します。

| 式 | 説明 |
| ---------- | ----------- |
| `0 0 13 * * ?` | イベントは毎日午後 1 時に発生します。 |
| `0 30 9 * * ? 2022` | このイベントは、2022 年の午前 9 時 30 分に毎日開催されます。 |
| `0 * 18 * * ?` | イベントは、毎日午後 6 時から毎日午後 6 時 59 分まで、毎分実行されます。 |
| `0 0/10 17 * * ?` | イベントは、午後 5 時から午後 6 時まで、毎日 10 分ごとに実行されます。 |
| `0 13,38 5 ? 6 WED` | イベントは、6 月の毎週水曜日の午前 5 時 13 分と午前 5 時 38 分に発生します。 |
| `0 30 12 ? * 4#3` | イベントは、毎月第 3 水曜日の午後 12 時 30 分に開始されます。 |
| `0 30 12 ? * 6L` | イベントは、毎月最後の金曜日の午後 12 時 30 分に発生します。 |
| `0 45 11 ? * MON-THU` | イベントは、毎週月曜日、火曜日、水曜日、木曜日の午前 11 時 45 分に発生します。 |