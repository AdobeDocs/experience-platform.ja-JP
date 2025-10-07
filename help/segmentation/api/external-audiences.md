---
title: 外部オーディエンス API エンドポイント
description: 外部オーディエンス API を使用して、Adobe Experience Platformから外部オーディエンスを作成、更新、アクティブ化および削除する方法について説明します。
exl-id: eaa83933-d301-48cb-8a4d-dfeba059bae1
source-git-commit: 0a37ef2f5fc08eb515c7c5056936fd904ea6d360
workflow-type: tm+mt
source-wordcount: '2253'
ht-degree: 9%

---

# 外部オーディエンスエンドポイント

外部オーディエンスを使用すると、外部ソースからAdobe Experience Platformにプロファイルデータをアップロードできます。 Segmentation Service API の `/external-audience` エンドポイントを使用すると、外部オーディエンスをExperience Platformに取り込んだり、詳細を表示したり、外部オーディエンスを更新したり、外部オーディエンスを削除したりできます。

## はじめに

>[!IMPORTANT]
>
>このガイドのエンドポイントには、`/core/ais` ではなく、`/core/ups` がプレフィックスとして付けられています。

Experience Platform API を使用するには、[ 認証チュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの使用について詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## 外部オーディエンスを作成 {#create-audience}

`/external-audience/` エンドポイントに POST リクエストを実行することで、外部オーディエンスを作成できます。

**API 形式**

```http
POST /external-audience/
```

**リクエスト**

+++ 外部オーディエンスを作成するリクエストのサンプル。

```shell
curl -X POST https://platform.adobe.io/data/core/ais/external-audience/ \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "Sample external audience",
        "description": "A sample version of an external audience",
        "fields": [
            {
                "name": "ppid",
                "type": "string",
                "identityNs": "email"
            },
            {
                "name": "list_id",
                "type": "string",
                "labels": ["core/C2", "custom/deep"]
            },
            {
                "name": "delete",
                "type": "number"
            },
            {
                "name": "process_consent",
                "type": "string"
            }
        ],
        "sourceSpec": {
            "params": {
                "path": "activation/sample-source/example.csv",
                "type": "file",
                "sourceType": "Cloud Storage",
                "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b"
            }
        },
        "ttlInDays": "40",
        "labels": ["core/C1"],
        "audienceType": "people",
        "originName": "CUSTOM_UPLOAD"
    }'
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `name` | 文字列 | 外部オーディエンスの名前。 |
| `description` | 文字列 | 外部オーディエンスのオプションの説明。 |
| `customAudienceId` | 文字列 | 外部オーディエンスのオプションの識別子。 |
| `fields` | オブジェクトの配列 | フィールドとそのデータタイプのリスト。 フィールドのリストを作成する際には、次の項目を追加できます。 <ul><li>`name`: **必須** 外部オーディエンスの仕様に含まれるフィールドの名前。</li><li>`type`: **必須** フィールドに入力されるデータのタイプ。 サポートされる値は、`string`、`number`、`long`、`integer`、`date` （`2025-05-13`）、`datetime` （`2025-05-23T20:19:00+00:00`）、`boolean` です。</li><li>`identityNs`: **ID フィールドには必須** ID フィールドで使用される名前空間。 サポートされる値には、`ECID` や `email` など、有効なすべての名前空間が含まれます。</li><li>`labels`: *オプション* フィールドのアクセス制御ラベルの配列。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベルの用語集 ](/help/data-governance/labels/reference.md) を参照してください。 </li></ul> |
| `sourceSpec` | オブジェクト | 外部オーディエンスが配置されている情報を含むオブジェクト。 このオブジェクトを使用する場合、次の情報を含める **必要があります**。 <ul><li>`path`: **必須**：ソース内の外部オーディエンスを含む外部オーディエンスまたはフォルダーの場所。 ファイルパス **にスペースを含めることはできません**。 例えば、パスが `activation/sample-source/Example CSV File.csv` の場合、パスを `activation/sample-source/ExampleCSVFile.csv` に設定します。 データフローセクションの **Source data** 列内でソースへのパスを見つけることができます。</li><li>`type`: **必須** ソースから取得するオブジェクトのタイプ。 この値は、`file` または `folder` のいずれかです。</li><li>`sourceType`: *オプション* 取得元のソースのタイプ。 現在、サポートされている値は `Cloud Storage` のみです。</li><li>`cloudType`: **必須** ソースタイプに基づく、クラウドストレージのタイプ。 サポートされる値は、`S3`、`DLZ`、`GCS`、`Azure`、`SFTP` です。</li><li>`baseConnectionId`: ベース接続の ID。ソースプロバイダーから提供されます。 **値の**、`cloudType` または `S3` を使用する場合、この値は `GCS` 必須 `SFTP` です。 それ以外の場合は、このパラメーターを含める必要は **ありません**。 詳しくは、[ ソースコネクタの概要 ](../../sources/home.md) を参照してください。</li></ul> |
| `ttlInDays` | 整数 | 外部オーディエンスのデータ有効期限（日数）。 この値は 1～90 に設定できます。 デフォルトでは、データの有効期限は 30 日に設定されています。 |
| `audienceType` | 文字列 | 外部オーディエンスのオーディエンスタイプ。 現在は、`people` のみがサポートされています。 |
| `originName` | 文字列 | **必須** オーディエンスの接触チャネル。 これは、オーディエンスがどこから来たかを示します。外部オーディエンスの場合は、`CUSTOM_UPLOAD` を使用する必要があります。 |
| `namespace` | 文字列 | オーディエンスの名前空間。 デフォルトでは、この値は `CustomerAudienceUpload` に設定されています。 |
| `labels` | 文字列の配列 | 外部オーディエンスに適用されるアクセス制御ラベル。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベルの用語集 ](/help/data-governance/labels/reference.md) を参照してください。 |
| `tags` | 文字列の配列 | 外部オーディエンスに適用するタグ。 タグについて詳しくは、[ タグ管理ガイド ](/help/administrative-tags/ui/managing-tags.md) を参照してください。 |

+++

**応答**

応答が成功すると、HTTP ステータス 202 が、新しく作成された外部オーディエンスの詳細と共に返されます。

+++ 外部オーディエンスを作成する際の応答例。

```json
{
    "operationId": "df8cd82f-a214-4b72-b549-d6ee23f1ff1a",
    "operationDetails": {
        "name": "Sample external audience",
        "description": "A sample version of an external audience",
        "fields": [
            {
                "name": "ppid",
                "type": "string",
                "identityNs": "Email"
            },
            {
                "name": "list_id",
                "type": "string",
                "labels": ["core/C2", "custom/deep"]
            },
            {
                "name": "delete",
                "type": "number"
            },
            {
                "name": "process_consent",
                "type": "string"
            }
        ],
        "sourceSpec": {
            "params": {
                "path": "activation/sample-source/example.csv",
                "type": "file",
                "sourceType": "Cloud Storage",
                "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b"
            }
        },
        "ttlInDays": 40,
        "labels": ["core/C1"],
        "audienceType": "people",
        "originName": "CUSTOM_UPLOAD"
    }   
}
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `operationId` | 文字列 | 操作の ID。 その後、この ID を使用して、オーディエンスの作成ステータスを取得できます。 |
| `operationDetails` | オブジェクト | 外部オーディエンスの作成のために送信したリクエストの詳細を含むオブジェクト。 |
| `name` | 文字列 | 外部オーディエンスの名前。 |
| `description` | 文字列 | 外部オーディエンスの説明。 |
| `fields` | オブジェクトの配列 | フィールドとそのデータタイプのリスト。 この配列は、外部オーディエンスで必要なフィールドを決定します。 |
| `sourceSpec` | オブジェクト | 外部オーディエンスが配置されている情報を含むオブジェクト。 |
| `ttlInDays` | 整数 | 外部オーディエンスのデータ有効期限（日数）。 この値は 1～90 に設定できます。 デフォルトでは、データの有効期限は 30 日に設定されています。 |
| `audienceType` | 文字列 | 外部オーディエンスのオーディエンスタイプ。 |
| `originName` | 文字列 | **必須** オーディエンスの接触チャネル。 これは、オーディエンスのソースを示します。 |
| `namespace` | 文字列 | オーディエンスの名前空間。 |
| `labels` | 文字列の配列 | 外部オーディエンスに適用されるアクセス制御ラベル。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベルの用語集 ](/help/data-governance/labels/reference.md) を参照してください。 |


+++

## オーディエンス作成ステータスの取得 {#retrieve-status}

外部オーディエンス送信のステータスを取得するには、`/external-audiences/operations` エンドポイントにGET リクエストを実行し、外部オーディエンス作成応答から受け取った操作の ID を指定します。

**API 形式**

```http
GET /external-audiences/operations/{OPERATION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{OPERATION_ID}` | 取得する操作の `id` 値。 |

**リクエスト**

+++ 外部オーディエンスの操作ステータスを取得するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/operations/df8cd82f-a214-4b72-b549-d6ee23f1ff1a \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、外部オーディエンスのタスクステータスの詳細と共に返されます。

+++ 外部オーディエンスのタスクステータスを取得する場合の応答例。

```json
{
    "operationId": "df8cd82f-a214-4b72-b549-d6ee23f1ff1a",
    "status": "SUCCESS",
    "operationDetails": {
        "name": "Sample external audience",
        "description": "A sample version of an external audience",
        "fields": [
            {
                "name": "ppid",
                "type": "string",
                "identityNs": "Email"
            },
            {
                "name": "list_id",
                "type": "string",
                "labels": ["core/C2", "custom/deep"]
            },
            {
                "name": "delete",
                "type": "number"
            },
            {
                "name": "process_consent",
                "type": "string"
            }
        ],
        "sourceSpec": {
            "params": {
                "path": "activation/sample-source/example.csv",
                "type": "file",
                "sourceType": "Cloud Storage",
                "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b"
            }
        },
        "ttlInDays": 40,
        "labels": ["core/C1"],
        "audienceType": "people",
        "originName": "CUSTOM_UPLOAD"
    },
    "audienceName": "Sample external audience",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "createdBy": "{USER_ID}",
    "createdAt": 1749324248,
    "updatedBy": "{USER_ID}",
    "updatedAt": 1749624273
}
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `operationId` | 文字列 | 取得する操作の ID。 |
| `status` | 文字列 | 操作のステータス。 `SUCCESS`、`FAILED`、`PROCESSING` のいずれかの値を指定できます。 |
| `operationDetails` | オブジェクト | オーディエンスの詳細を含むオブジェクト。 |
| `audienceId` | 文字列 | 操作によって送信される外部オーディエンスの ID。 |
| `createdBy` | 文字列 | 外部オーディエンスを作成したユーザーの ID。 |
| `createdAt` | ロングエポックタイムスタンプ | 外部オーディエンスを作成するリクエストが送信された際のタイムスタンプ（秒）。 |
| `updatedBy` | 文字列 | オーディエンスを最後に更新したユーザーの ID。 |
| `updatedAt` | ロングエポックタイムスタンプ | オーディエンスが最後に更新された際のタイムスタンプ（秒）。 |

+++

## 外部オーディエンスの更新 {#update-audience}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの `audienceId` が必要です。 `audienceId` エンドポイントへの呼び出しが成功すると、`GET /external-audiences/operations/{OPERATION_ID}` を取得できます。

`/external-audience` エンドポイントに対してPATCH リクエストを実行し、リクエストパスにオーディエンスの ID を指定することで、外部オーディエンスのフィールドを更新できます。

このエンドポイントを使用する場合、次のフィールドを更新できます。

- オーディエンスの説明
- フィールドレベルのアクセス制御ラベル
- オーディエンスレベルのアクセス制御ラベル
- オーディエンスのデータ有効期限

このエンドポイントを使用してフィールドを更新すると **リクエストしたフィールドのコンテンツを置き換え** ます。

**API 形式**

```http
PATCH /external-audience/{AUDIENCE_ID}
```

**リクエスト**

+++ 外部オーディエンスの説明を更新するリクエストのサンプル。

```shell
curl -X PATCH https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab\
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "description": "New sample description"
 }'
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `description` | 文字列 | 外部オーディエンスの更新された説明。 |

さらに、次のパラメーターを更新できます。

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `labels` | 配列 | オーディエンスのアクセスラベルの更新されたリストを含む配列。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベルの用語集 ](/help/data-governance/labels/reference.md) を参照してください。 |
| `fields` | オブジェクトの配列 | 外部オーディエンスのフィールドおよび関連するラベルを含む配列。 PATCH リクエストにリストされているフィールドのみが更新されます。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベルの用語集 ](/help/data-governance/labels/reference.md) を参照してください。 |
| `ttlInDays` | 整数 | 外部オーディエンスのデータ有効期限（日数）。 この値は 1～90 に設定できます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、更新された外部オーディエンスの詳細と共に返されます。

+++ 外部オーディエンスの説明を更新する際のサンプル応答。

```json
{
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceName": "Sample external audience",
    "description": "New sample description",
    "fields": [
        {
            "name": "ppid",
            "type": "string",
            "identityNs": "Email"
        },
        {
            "name": "list_id",
            "type": "string",
            "labels": ["core/C2", "custom/deep"]
        },
        {
            "name": "delete",
            "type": "number"
        },
        {
            "name": "process_consent",
            "type": "string"
        }
    ],
    "sourceSpec": {
        "path": "activation/sample-source/example.csv",
        "type": "file",
        "sourceType": "Cloud Storage",
        "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b"
        },
    "ttlInDays": 40,
    "labels": ["core/C1"],
    "audienceType": "people",
    "originName": "CUSTOM_UPLOAD",
    "createdBy": "{USER_ID}",
    "createdAt": 1749324248,
    "updatedBy": "{USER_ID}",
    "updatedAt": 1749624273
}
```

+++

## オーディエンスの取り込みを開始 {#start-audience-ingestion}

>[!IMPORTANT]
>
>以前のオーディエンスの取り込みが既に進行中の場合は **外部オーディエンスに対する新しい取り込みを開始で** ません。

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの `audienceId` が必要です。 `audienceId` エンドポイントへの呼び出しが成功すると、`GET /external-audiences/operations/{OPERATION_ID}` を取得できます。

オーディエンス ID を指定しながら次のエンドポイントに対して POST リクエストを実行することで、オーディエンスの取り込みを開始できます。

**API 形式**

```http
POST /external-audience/{AUDIENCE_ID}/runs
```

**リクエスト**

次のリクエストは、外部オーディエンスに対して取り込み実行をトリガーします。

+++ オーディエンスの取り込みを開始するリクエストのサンプル。

```shell
curl -X POST https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/runs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
    "dataFilterStartTime": 764245635
 }' 
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `dataFilterStartTime` | エポックタイムスタンプ | **必須** 処理するファイルを決定する開始時間を指定する範囲。 つまり、選択されたファイルは、指定された時間が経過した **後** ファイルになります。 |
| `dataFilterEndTime` | エポックタイムスタンプ | 処理するファイルを選択するためにフローを実行する終了時刻を指定する範囲。 つまり、選択されたファイルは、指定された時間より前の **ファイル** なります。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、取り込み実行の詳細と共に返されます。

+++ オーディエンスの取り込みを開始する際のサンプル応答。

```json
{
    "audienceName": "Sample external audience",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "runId": "fb342311-725d-4b48-ab7d-c6105fbc2b8b",
    "differentialIngestion": true,
    "dataFilterStartTime": 764245635,
    "dataFilterEndTime": 4565657575,
    "createdAt": 4565657575,
    "createdBy:" "{USER_ID}"
}
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `audienceName` | 文字列 | 取り込み実行を開始するオーディエンスの名前。 |
| `audienceId` | 文字列 | オーディエンスの ID。 |
| `runId` | 文字列 | 開始した取得実行の ID。 |
| `differentialIngestion` | ブール値 | 前回の取り込み以降の違いに基づいて、取り込みが部分取り込みか、完全なオーディエンス取り込みかを決定するフィールド。 |
| `dataFilterStartTime` | エポックタイムスタンプ | 処理されたファイルを選択するためにフローを実行する開始時間を指定する範囲。 |
| `dataFilterEndTime` | エポックタイムスタンプ | 処理されたファイルを選択するためにフローを実行する終了時刻を指定する範囲。 |
| `createdAt` | ロングエポックタイムスタンプ | 外部オーディエンスを作成するリクエストが送信された際のタイムスタンプ（秒）。 |
| `createdBy` | 文字列 | 外部オーディエンスを作成したユーザーの ID。 |

+++

## 特定のオーディエンス取り込みステータスの取得 {#retrieve-ingestion-status}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンス `audienceId` と取り込み実行 ID の `runId` の両方が必要です。 `audienceId` エンドポイントへの呼び出しが成功すると、`GET /external-audiences/operations/{OPERATION_ID}` を取得でき、`runId` エンドポイントの以前の呼び出しが成功すると、`POST /external-audience/{AUDIENCE_ID}/runs` を取得できます。

オーディエンス ID と実行 ID の両方を提供しながら、次のエンドポイントに対してGET リクエストを実行することで、オーディエンス取り込みのステータスを取得できます。

**API 形式**

```http
GET /external-audience/{AUDIENCE_ID}/runs/{RUN_ID}
```

**リクエスト**

次のリクエストは、外部オーディエンスの取り込みステータスを取得します。

+++ 外部オーディエンスの取り込みステータスを取得するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/runs/fb342311-725d-4b48-ab7d-c6105fbc2b8b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、外部オーディエンス取り込みの詳細と共に返されます。

+++ 外部オーディエンスの取り込みを取得する際の応答例。

```json
{
    "audienceName": "Sample external audience",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "runId": "fb342311-725d-4b48-ab7d-c6105fbc2b8b",
    "status": "SUCCESS",
    "differentialIngestion": true,
    "dataFilterStartTime": 764245635,
    "dataFilterEndTime": 3456788568,
    "createdAt": 1749324248,
    "createdBy": "{USER_ID}",
    "details": [
        {
            "stage": "DATASET_INGEST",
            "status": "SUCCESS",
            "flowRunId": "{FLOW_RUN_ID}"
        },
        {
            "stage": "PROFILE_STORE_INGEST",
            "status": "SUCCESS",
            "flowRunId": "{FLOW_RUN_ID}"
        }
    ]
}
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `audienceName` | 文字列 | オーディエンスの名前。 |
| `audienceId` | 文字列 | オーディエンスの ID。 |
| `runId` | 文字列 | 取り込み実行の ID。 |
| `status` | 文字列 | 取得実行のステータス。 可能なステータスには、`SUCCESS` および `FAILED` があります。 |
| `differentialIngestion` | ブール値 | 前回の取り込み以降の違いに基づいて、取り込みが部分取り込みか、完全なオーディエンス取り込みかを決定するフィールド。 |
| `dataFilterStartTime` | エポックタイムスタンプ | 処理されたファイルを選択するためにフローを実行する開始時間を指定する範囲。 |
| `dataFilterEndTime` | エポックタイムスタンプ | 処理されたファイルを選択するためにフローを実行する終了時刻を指定する範囲。 |
| `createdAt` | ロングエポックタイムスタンプ | 外部オーディエンスを作成するリクエストが送信された際のタイムスタンプ（秒）。 |
| `createdBy` | 文字列 | 外部オーディエンスを作成したユーザーの ID。 |
| `details` | オブジェクトの配列 | 取り込み実行の詳細を含むオブジェクト。 <ul><li>`stage`：取得実行のステージ。 これは `DATASET_INGEST` または `PROFILE_STORE_INGEST` であり、データレイクの取り込みとプロファイルの取り込みを表します。</li><li>`status`：ステージでの取り込みのステータス。 可能なステータスには、`SUCCESS` および `FAILED` があります。</li><li>`flowRunId`：ステージの取り込みフロー実行の ID。</li></ul> |

+++

## オーディエンス取り込み実行のリスト {#list-ingestion-runs}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの `audienceId` が必要です。 `audienceId` エンドポイントへの呼び出しが成功すると、`GET /external-audiences/operations/{OPERATION_ID}` を取得できます。

オーディエンス ID を指定した際に、次のエンドポイントに対してGET リクエストを行うことで、選択した外部オーディエンスに対して実行されたすべての取り込みを取得できます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

**API 形式**

<!-- The following endpoint supports several query parameters to help filter your results. While these parameters are optional, their use is strongly recommended to help focus your results. -->

```http
GET /external-audience/{AUDIENCE_ID}/runs
```

<!-- **Query parameters**

+++ A list of available query parameters. 

| Parameter | Description | Example |
| --------- | ----------- | ------- |
| `limit` | The maximum number of items returned in the response. This value can range from 1 to 40. By default, the limit is set to 20. | `limit=30` |
| `sortBy` | The order in which the returned items are sorted. You can sort by either `name` or by `createdAt`. Additionally, you can add a `-` sign to sort by **descending** order instead of **ascending** order. By default, the items are sorted by `createdAt` in descending order. | `sortBy=name` |
| `property` | A filter to determine which audience ingestion runs are displayed. You can filter on the following properties: <ul><li>`name`: Lets you filter by the audience name. If using this property, you can compare by using `=`, `!=`, `=contains`, or `!=contains`. </li><li>`createdAt`: Lets you filter by the ingestion time. If using this property, you can compare by using `>=` or `<=`.</li><li>`status`: Lets you filter by the ingestion run's status. If using this property, you can compare by using `=`, `!=`, `=contains`, or `!=contains`. </li></ul>  | `property=createdAt<1683669114845`<br/>`property=name=demo_audience`<br/>`property=status=SUCCESS` |

+++ -->

**リクエスト**

次のリクエストでは、外部オーディエンスに対するすべての取り込み実行を取得します。

+++ オーディエンス取り込みの実行リストを取得するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/runs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、指定された外部オーディエンスに対する取り込み実行のリストと共に返されます。

+++ オーディエンス取り込みの実行リストを取得する際の応答のサンプルです。

```json
{
    "runs": [
        {
            "audienceName": "Sample external audience",
            "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "runId": "fb342311-725d-4b48-ab7d-c6105fbc2b8b",
            "status": "SUCCESS",
            "differentialIngestion": true,
            "dataFilterStartTime": 764245635,
            "dataFilterEndTime": 3456788568,
            "createdAt": 1785678909,
            "createdBy": "{USER_NAME}"
        },
        {
            "audienceName": "Sample external audience 2",
            "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "runId": "406e38e4-fbd5-43e1-8d0c-01ccb3f9ad10",
            "status": "SUCCESS",
            "differentialIngestion": true,
            "dataFilterStartTime": 764245635,
            "dataFilterEndTime": 3456788568,
            "createdAt": 1749324248,
            "createdBy": "{USER_ID}"
        }
    ]
}
```

<!-- ,
    "_page": {
        "limit": 20,
        "count": 2,
        "totalCount": 2
    }
    
| `_page` | Object | An object that contains the pagination information about the list of results. |
     -->

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `runs` | オブジェクト | オーディエンスに属する取り込み実行のリストを含むオブジェクト。 このオブジェクトについて詳しくは、[ 取り込みステータスの取得 ](#retrieve-ingestion-status) を参照してください。 |

+++

## 外部オーディエンスの削除 {#delete-audience}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの `audienceId` が必要です。 `audienceId` エンドポイントへの呼び出しが成功すると、`GET /external-audiences/operations/{OPERATION_ID}` を取得できます。

オーディエンス ID を指定した状態で次のエンドポイントに対してDELETE リクエストを行うことで、外部オーディエンスを削除できます。

**API 形式**

```http
DELETE /external-audience/{AUDIENCE_ID}
```

**リクエスト**

次のリクエストは、指定された外部オーディエンスを削除します。

+++ 外部オーディエンスを削除するリクエストのサンプル。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/ \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 204 が、空の応答本文と共に返されます。

## 次の手順 {#next-steps}

このガイドを読むことで、Experience Platform API を使用して外部オーディエンスを作成、管理および削除する方法について、理解が深まりました。 Experience Platform UI で外部オーディエンスを使用する方法については、[ オーディエンスポータルドキュメント ](../ui/audience-portal.md) を参照してください。

## 付録 {#appendix}

次の節では、外部オーディエンス API を使用する場合に使用可能なエラーコードを示します。

| Platform エラーコード | ステータスコード | メッセージ | 説明 |
| ------------------- | ----------- | ------- | ----------- |
| 100910-400 | 400 | `BAD_REQUEST` | リクエストの検証中にエラーが発生したため、無効なリクエストが発生しました。 |
| 100911-400 | 400 | `BAD_REQUEST` | 無効なトークンが指定されました。 |
| 100920-401 | 401 | `UNAUTHORIZED` | ヘッダーがありません。 |
| 100921-401 | 401 | `UNAUTHORIZED` | 無効な `imsOrgId` が指定されました。 |
| 100922-401 | 401 | `UNAUTHORIZED` | 外部オーディエンス API を使用する権限がありません。 |
| 100940-404 | 404 | `NOT_FOUND` | リクエストされたオーディエンスが見つかりませんでした。 |
| 100950-409 | 409 | `DUPLICATE_RESOURCE` | オーディエンスは既に存在します。 |
| 100960-422 | 422 | `UNPROCESSABLE_ENTITY` | リクエスト構造は有効ですが、論理エラーまたはセマンティックエラーが原因で処理できません。 |
| 100970-500 | 500 | `INTERNAL_SERVER_ERROR` | システムでリクエストを処理中に問題が発生しました。 |
| 100970-502 | 502 | `BAD_GATEWAY` | ダウンストリームの依存関係に関する問題があります。 |
