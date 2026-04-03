---
title: External Audiences API エンドポイント
description: 外部オーディエンス APIを使用して、Adobe Experience Platformから外部オーディエンスを作成、更新、アクティブ化、削除する方法を説明します。
exl-id: eaa83933-d301-48cb-8a4d-dfeba059bae1
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '2622'
ht-degree: 8%

---

# 外部オーディエンスエンドポイント

外部オーディエンスを利用すると、外部ソースからAdobe Experience Platformにプロファイルデータをアップロードできます。 Segmentation Service APIの`/external-audience` エンドポイントを使用して、外部オーディエンスをExperience Platformに取り込み、詳細を表示し、外部オーディエンスを更新したり、外部オーディエンスを削除したりできます。

## ガードレール

3月リリース以降、外部オーディエンスエンドポイントを使用する場合、次のガードレールが適用されます。

| ガードレール | 上限 | 制限タイプ | 説明 |
| --------- | ----- | ---------- | ----------- |
| 1日あたりのオーディエンス取り込み実行数 | 100 | システム強制ガードレール | 1日に許可されるオーディエンス取り込み実行の最大数。 この制限は、**サンドボックス** レベルごとです。 |
| オーディエンスあたりの取り込み数 | 10 | システム強制ガードレール | 指定したオーディエンスに対して実行できる取り込みの数。 |
| 外部オーディエンスサイズ | 10 GB | パフォーマンスガードレール | 外部オーディエンスの推奨される合計サイズは10 GBです。 |

## はじめに

>[!IMPORTANT]
>
>このガイドのエンドポイントには、`/core/ais`ではなく`/core/ups`が付いています。

Experience Platform APIを使用するには、[認証チュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの使用について詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## 外部オーディエンスの作成 {#create-audience}

`/external-audience/` エンドポイントにPOST リクエストを行うことで、外部オーディエンスを作成できます。

**API 形式**

```http
POST /external-audience/
```

**リクエスト**

+++ 外部オーディエンスを作成するためのサンプルリクエスト。

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
                "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b",
                "encryption": {
                    "publicKeyId": "e31ae895-7896-469a-8e06-eb9207ddf1c2",
                    "signVerificationId": "ZTMxYWU4OTUtNzg5Ni00NjlhLThlMDYtZWI5MjA3ZGRmMWMy"
                }
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
| `customAudienceId` | 文字列 | 外部オーディエンスのオプション識別子。 |
| `fields` | オブジェクトの配列 | フィールドとそのデータタイプのリスト。 配列内に最小1 フィールドと最大41 フィールドが必要です。 フィールド **の1つはID フィールドである必要があり、**&#x200B;が含まれています。 `identityNs`フィールドのリストを作成する場合は、次の項目を追加できます。 <ul><li>`name`: **必須**&#x200B;外部オーディエンス仕様の一部であるフィールドの名前。</li><li>`type`: **必須** フィールドに取り込まれるデータのタイプ。 サポートされている値には、`string`、`number`、`long`、`integer`、`date` （`2025-05-13`）、`datetime` （`2025-05-23T20:19:00+00:00`）および`boolean`が含まれます。</li><li>`identityNs`: **ID フィールドに必須** ID フィールドで使用される名前空間。 サポートされている値には、`ECID`や`email`など、有効なすべての名前空間が含まれます。</li><li>`labels`: *オプション* フィールドのアクセス制御ラベルの配列。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベル用語集](/help/data-governance/labels/reference.md)を参照してください。 </li></ul> |
| `sourceSpec` | オブジェクト | 外部オーディエンスが配置されている情報を含むオブジェクト。 このオブジェクトを使用する場合、**には次の情報を含める必要があります。** <ul><li>`path`: **必須**：外部オーディエンスの場所、またはソース内の外部オーディエンスを含むフォルダー。 ファイルパス **にスペースを含めることはできません**。 例えば、パスが`activation/sample-source/Example CSV File.csv`の場合、パスを`activation/sample-source/ExampleCSVFile.csv`に設定します。 データフローセクションの&#x200B;**Source data**&#x200B;列内で、ソースへのパスを見つけることができます。</li><li>`type`: **必須** ソースから取得するオブジェクトのタイプ。 この値は`file`または`folder`のいずれかです。</li><li>`sourceType`: *オプション*&#x200B;取得するソースのタイプ。 現在、サポートされている値は`Cloud Storage`のみです。</li><li>`cloudType`: **必須** ソースタイプに基づくクラウドストレージのタイプ。 サポートされている値には、`S3`、`DLZ`、`GCS`、`Azure`および`SFTP`が含まれます。</li><li>`baseConnectionId`: ベース接続のID。ソースプロバイダーから提供されます。 この値は、**値**、`cloudType`、または`S3`を使用する場合、`GCS`必須`SFTP`です。 それ以外の場合は、**not**&#x200B;でこのパラメーターを含める必要があります。 詳しくは、[ ソースコネクタの概要](../../sources/home.md)を参照してください。</li><li>`encryption`: *オプション*&#x200B;非同期暗号化データ取り込みに必要な暗号化キーを含むオブジェクト。</li><ul><li>`publicKeyId`: **必須**：暗号化キーのペアを生成したときに返された公開鍵ID。 詳しくは、[暗号化データ ガイド ](/help/sources/tutorials/api/encrypt-data.md#create-encryption-key-pair)を参照してください。 </li><li>`signVerificationKeyId`: *オプション*: カスタマー管理キーをExperience Platformと共有したときに返された公開鍵ID。 **注：**&#x200B;このフィールドは、そのAPI リクエストの応答で`publicKeyId`としてラベル付けされています。 詳しくは、[暗号化データ ガイド ](/help/sources/tutorials/api/encrypt-data.md##share-your-public-key-to-experience-platform)を参照してください。</li></ul></ul> |
| `ttlInDays` | 整数 | 外部オーディエンスのデータの有効期限（日数）。 この値は1 ～ 90の範囲で設定できます。 デフォルトでは、データの有効期限は30日に設定されています。 |
| `audienceType` | 文字列 | 外部オーディエンスのオーディエンスタイプ。 現在は、`people` のみがサポートされています。 |
| `originName` | 文字列 | **必須** オーディエンスのオリジン。 これは、オーディエンスがどこから来たかを示します。外部オーディエンスの場合は、`CUSTOM_UPLOAD`を使用する必要があります。 |
| `namespace` | 文字列 | オーディエンスの名前空間。 デフォルトでは、この値は `CustomerAudienceUpload` に設定されています。 |
| `labels` | 文字列の配列 | 外部オーディエンスに適用されるアクセス制御ラベル。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベル用語集](/help/data-governance/labels/reference.md)を参照してください。 |
| `tags` | 文字列の配列 | 外部オーディエンスに適用するタグ。 タグの配列を追加する場合、**は**&#x200B;を`tagId`使用する必要があります。 タグについて詳しくは、[ タグの管理ガイド ](/help/administrative-tags/ui/managing-tags.md)を参照してください。 |

+++

**応答**

応答が成功すると、HTTP ステータス 202が、新しく作成された外部オーディエンスの詳細と共に返されます。

+++ 外部オーディエンスを作成する際の応答サンプル。

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
                "baseConnectionId": "1d1d4bc5-b527-46a3-9863-530246a61b2b",
                "encryption": {
                    "publicKeyId": "e31ae895-7896-469a-8e06-eb9207ddf1c2",
                    "signVerificationId": "ZTMxYWU4OTUtNzg5Ni00NjlhLThlMDYtZWI5MjA3ZGRmMWMy"
                }
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
| `operationId` | 文字列 | 操作のID。 このIDを使用して、オーディエンスの作成ステータスを取得できます。 |
| `operationDetails` | オブジェクト | 外部オーディエンスを作成するために送信したリクエストの詳細を含むオブジェクト。 |
| `name` | 文字列 | 外部オーディエンスの名前。 |
| `description` | 文字列 | 外部オーディエンスの説明。 |
| `fields` | オブジェクトの配列 | フィールドとそのデータタイプのリスト。 この配列は、外部オーディエンスに必要なフィールドを決定します。 |
| `sourceSpec` | オブジェクト | 外部オーディエンスが配置されている情報を含むオブジェクト。 |
| `ttlInDays` | 整数 | 外部オーディエンスのデータの有効期限（日数）。 この値は1 ～ 90の範囲で設定できます。 デフォルトでは、データの有効期限は30日に設定されています。 |
| `audienceType` | 文字列 | 外部オーディエンスのオーディエンスタイプ。 |
| `originName` | 文字列 | **必須** オーディエンスのオリジン。 これはオーディエンスの出所を示しています。 |
| `namespace` | 文字列 | オーディエンスの名前空間。 |
| `labels` | 文字列の配列 | 外部オーディエンスに適用されるアクセス制御ラベル。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベル用語集](/help/data-governance/labels/reference.md)を参照してください。 |


+++

## オーディエンス作成ステータスの取得 {#retrieve-status}

`/external-audiences/operations` エンドポイントに対してGET リクエストを行い、「外部オーディエンスを作成」レスポンスから受け取った操作のIDを指定することで、外部オーディエンス送信のステータスを取得できます。

**API 形式**

```http
GET /external-audiences/operations/{OPERATION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{OPERATION_ID}` | 取得する操作の`id`値。 |

**リクエスト**

+++ 外部オーディエンスの操作ステータスを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/operations/df8cd82f-a214-4b72-b549-d6ee23f1ff1a \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、外部オーディエンスのタスクステータスの詳細が表示されます。

+++ 外部オーディエンスのタスクステータスを取得する際の応答のサンプル。

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
| `operationId` | 文字列 | 取得する操作のID。 |
| `status` | 文字列 | 操作のステータス。 次のいずれかの値を指定できます：`SUCCESS`、`FAILED`、`PROCESSING`。 |
| `operationDetails` | オブジェクト | オーディエンスの詳細を含むオブジェクト。 |
| `audienceId` | 文字列 | 操作によって送信される外部オーディエンスのID。 |
| `createdBy` | 文字列 | 外部オーディエンスを作成したユーザーのID。 |
| `createdAt` | 長いエポックタイムスタンプ | 外部オーディエンスを作成するリクエストが送信された際のタイムスタンプ（秒単位）。 |
| `updatedBy` | 文字列 | オーディエンスを最後に更新したユーザーのID。 |
| `updatedAt` | 長いエポックタイムスタンプ | オーディエンスが最後に更新されたタイムスタンプ（秒単位）。 |

+++

## 外部オーディエンスの更新 {#update-audience}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの`audienceId`が必要です。 `audienceId` エンドポイントへの正常な呼び出しから`GET /external-audiences/operations/{OPERATION_ID}`を取得できます。

`/external-audience` エンドポイントに対してPATCH リクエストを行い、リクエストパスにオーディエンスのIDを指定することで、外部オーディエンスのフィールドを更新できます。

このエンドポイントを使用する場合は、次のフィールドを更新できます。

- オーディエンスの説明
- フィールドレベルのアクセス制御ラベル
- オーディエンスレベルのアクセス制御ラベル
- オーディエンスのデータ有効期限

このエンドポイント **を使用してフィールドを更新すると、リクエストしたフィールドのコンテンツが**&#x200B;に置き換えられます。

**API 形式**

```http
PATCH /external-audience/{AUDIENCE_ID}
```

**リクエスト**

+++ 外部オーディエンスの説明を更新するサンプルリクエスト。

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
| `labels` | 配列 | オーディエンスのアクセスラベルの更新されたリストを含む配列。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベル用語集](/help/data-governance/labels/reference.md)を参照してください。 |
| `fields` | オブジェクトの配列 | 外部オーディエンスのフィールドと関連するラベルを含む配列。 PATCH リクエストにリストされているフィールドのみが更新されます。 使用可能なアクセス制御ラベルについて詳しくは、[ データ使用ラベル用語集](/help/data-governance/labels/reference.md)を参照してください。 |
| `ttlInDays` | 整数 | 外部オーディエンスのデータの有効期限（日数）。 この値は1 ～ 90の範囲で設定できます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200が、更新された外部オーディエンスの詳細とともに返されます。

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

## オーディエンス収集を開始 {#start-audience-ingestion}

>[!IMPORTANT]
>
>以前のオーディエンスの取り込みが既に進行中の場合、**not**&#x200B;は外部オーディエンスの新しい取り込みを開始できます。

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの`audienceId`が必要です。 `audienceId` エンドポイントへの正常な呼び出しから`GET /external-audiences/operations/{OPERATION_ID}`を取得できます。
>
>さらに、このエンドポイントを使用して、以前に取り込んだオーディエンスのデータを更新できます。

オーディエンス IDを指定しながら、次のエンドポイントにPOST リクエストを実行することで、オーディエンスの取り込みを開始できます。

**API 形式**

```http
POST /external-audience/{AUDIENCE_ID}/runs
```

**リクエスト**

次のリクエストは、外部オーディエンスに対する取り込み実行をトリガーします。

+++ オーディエンス取り込みを開始するためのサンプルリクエスト。

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
| `dataFilterStartTime` | エポックタイムスタンプ | **必須**&#x200B;どのファイルが処理されるかを決定する開始時間を指定する範囲。 これは、選択されたファイルが指定された時間の&#x200B;**後**&#x200B;のファイルであることを意味します。 |
| `dataFilterEndTime` | エポックタイムスタンプ | フローを実行して処理するファイルを選択する終了時間を指定する範囲。 これは、選択されたファイルが指定された時間より&#x200B;**前**&#x200B;のファイルであることを意味します。 |

+++

**応答**

応答が成功すると、取り込み実行に関する詳細が記載されたHTTP ステータス 200が返されます。

+++ オーディエンス取り込みを開始する際の応答のサンプル。

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
| `audienceName` | 文字列 | 取り込みを開始するオーディエンスの名前。 |
| `audienceId` | 文字列 | オーディエンスのID。 |
| `runId` | 文字列 | 開始した取り込み実行のID。 |
| `differentialIngestion` | ブール | 最後の取り込み以降の違いまたは完全なオーディエンス取り込みからの差分に基づいて、取り込みが部分的な取り込みかどうかを判断するフィールド。 |
| `dataFilterStartTime` | エポックタイムスタンプ | フローが実行される開始時間を指定して、処理されたファイルを選択する範囲。 |
| `dataFilterEndTime` | エポックタイムスタンプ | フローが実行され、処理されたファイルを選択する終了時間を指定する範囲。 |
| `createdAt` | 長いエポックタイムスタンプ | 外部オーディエンスを作成するリクエストが送信された際のタイムスタンプ（秒単位）。 |
| `createdBy` | 文字列 | 外部オーディエンスを作成したユーザーのID。 |

+++

## 特定のオーディエンス取り込みステータスの取得 {#retrieve-ingestion-status}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの`audienceId`と取り込みの実行IDの`runId`の両方が必要です。 `audienceId`は、`GET /external-audiences/operations/{OPERATION_ID}` エンドポイントへの正常な呼び出しから取得でき、`runId`は`POST /external-audience/{AUDIENCE_ID}/runs` エンドポイントの以前の正常な呼び出しから取得できます。

オーディエンスと実行IDの両方を指定しながら、次のエンドポイントに対してGET リクエストを実行することで、オーディエンス取り込みのステータスを取得できます。

**API 形式**

```http
GET /external-audience/{AUDIENCE_ID}/runs/{RUN_ID}
```

**リクエスト**

次のリクエストは、外部オーディエンスの取り込みステータスを取得します。

+++ 外部オーディエンスの取り込みステータスを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/runs/fb342311-725d-4b48-ab7d-c6105fbc2b8b \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、外部オーディエンス取り込みの詳細が表示されます。

+++ 外部オーディエンスの取り込みを取得する際の応答のサンプル。

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
| `audienceId` | 文字列 | オーディエンスのID。 |
| `runId` | 文字列 | 取り込み実行のID。 |
| `status` | 文字列 | 取り込み実行のステータス。 可能なステータスは`SUCCESS`と`FAILED`です。 |
| `differentialIngestion` | ブール | 最後の取り込み以降の違いまたは完全なオーディエンス取り込みからの差分に基づいて、取り込みが部分的な取り込みかどうかを判断するフィールド。 |
| `dataFilterStartTime` | エポックタイムスタンプ | フローが実行される開始時間を指定して、処理されたファイルを選択する範囲。 |
| `dataFilterEndTime` | エポックタイムスタンプ | フローが実行され、処理されたファイルを選択する終了時間を指定する範囲。 |
| `createdAt` | 長いエポックタイムスタンプ | 外部オーディエンスを作成するリクエストが送信された際のタイムスタンプ（秒単位）。 |
| `createdBy` | 文字列 | 外部オーディエンスを作成したユーザーのID。 |
| `details` | オブジェクトの配列 | 取り込み実行の詳細を含むオブジェクト。 <ul><li>`stage`：取り込み実行のステージ。 これは、`DATASET_INGEST`または`PROFILE_STORE_INGEST`のいずれかであり、データレイクの取り込みとプロファイルの取り込みを表します。</li><li>`status`: ステージでの取り込みのステータス。 可能なステータスは`SUCCESS`と`FAILED`です。</li><li>`flowRunId`: ステージの取り込みフロー実行のID。</li></ul> |

+++

## オーディエンス取り込み実行のリスト {#list-ingestion-runs}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの`audienceId`が必要です。 `audienceId` エンドポイントへの正常な呼び出しから`GET /external-audiences/operations/{OPERATION_ID}`を取得できます。

オーディエンス IDを指定しながら、次のエンドポイントに対してGET リクエストを実行することで、選択した外部オーディエンスに対するすべての取り込み実行を取得できます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

**API 形式**

<!-- The following endpoint supports several query parameters to help filter your results. While these parameters are optional, their use is strongly recommended to help focus your results. -->

```http
GET /external-audience/{AUDIENCE_ID}/runs
```

<!-- 
**Query parameters**

+++ A list of available query parameters. 

| Parameter | Description | Example |
| --------- | ----------- | ------- |
| `limit` | The maximum number of items returned in the response. This value can range from 1 to 40. By default, the limit is set to 20. | `limit=30` |
| `sortBy` | The order in which the returned items are sorted. You can sort by either `name` or by `createdAt`. Additionally, you can add a `-` sign to sort by **descending** order instead of **ascending** order. By default, the items are sorted by `createdAt` in descending order. | `sortBy=name` |
| `property` | A filter to determine which audience ingestion runs are displayed. You can filter on the following properties: <ul><li>`name`: Lets you filter by the audience name. If using this property, you can compare by using `=`, `!=`, `=contains`, or `!=contains`. </li><li>`createdAt`: Lets you filter by the ingestion time. If using this property, you can compare by using `>=` or `<=`.</li><li>`status`: Lets you filter by the ingestion run's status. If using this property, you can compare by using `=`, `!=`, `=contains`, or `!=contains`. </li></ul>  | `property=createdAt<1683669114845`<br/>`property=name=demo_audience`<br/>`property=status=SUCCESS` |

+++ 
-->

**リクエスト**

次のリクエストは、外部オーディエンスに対するすべての取り込み実行を取得します。

+++ オーディエンス取り込みの実行のリストを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/runs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、指定された外部オーディエンスの取り込み実行のリストが表示されます。

+++ オーディエンス取り込みのリストを取得する際のサンプル応答が実行されます。

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

<!--
 ,
    "_page": {
        "limit": 20,
        "count": 2,
        "totalCount": 2
    }
    
| `_page` | Object | An object that contains the pagination information about the list of results. |
   
-->

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `runs` | オブジェクト | オーディエンスに属する取り込み実行のリストを含むオブジェクト。 このオブジェクトの詳細については、[取り込みステータスの取得セクション ](#retrieve-ingestion-status)を参照してください。 |

+++

## 外部オーディエンスのデータ有効期限の拡張 {#extend-data-expiration}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの`audienceId`が必要です。 `audienceId` エンドポイントへの正常な呼び出しから`GET /external-audiences/operations/{OPERATION_ID}`を取得できます。

オーディエンス IDを指定しながら、次のエンドポイントにPOST リクエストを行うことで、外部オーディエンスのデータ有効期限を延長できます。

データの有効期限は、取り込み時に設定された元の期間だけ延長されます。 期間が指定されていない場合は、デフォルトの30日間の延長が適用されます。 データの有効期限を延長すると、最後に正常に取り込まれたデータでオーディエンスが再び取り込まれます。

**API 形式**

```http
/ais/external-audience/extend-ttl/{AUDIENCE_ID}
```

**リクエスト**

次のリクエストは、指定された外部オーディエンスのデータ有効期限を拡張します。

+++ 外部オーディエンスのデータ有効期限を拡張するためのサンプルリクエスト。

```shell
curl -x POST https://platform.adobe.io/data/core/ais/external-audience/extend-ttl/60ccea95-1435-4180-97a5-58af4aa285ab \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200とオーディエンスの詳細が返されます。

+++ データの有効期限を拡張する際のサンプル応答。

```json
{
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "name": "Sample external audience"
}
```

+++

## 外部オーディエンスの削除 {#delete-audience}

>[!NOTE]
>
>次のエンドポイントを使用するには、外部オーディエンスの`audienceId`が必要です。 `audienceId` エンドポイントへの正常な呼び出しから`GET /external-audiences/operations/{OPERATION_ID}`を取得できます。

オーディエンス IDを指定しながら、次のエンドポイントに対してDELETE リクエストを行うことで、外部オーディエンスを削除できます。

**API 形式**

```http
DELETE /external-audience/{AUDIENCE_ID}
```

**リクエスト**

次のリクエストは、指定された外部オーディエンスを削除します。

+++ 外部オーディエンスを削除するためのサンプルリクエスト。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ais/external-audience/60ccea95-1435-4180-97a5-58af4aa285ab/ \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 204が空の応答本文で返されます。

## 次の手順 {#next-steps}

このガイドでは、Experience Platform APIを使用して外部オーディエンスを作成、管理、削除する方法について詳しく説明します。 Experience Platform UIで外部オーディエンスを使用する方法については、[ オーディエンスポータルのドキュメント ](../ui/audience-portal.md)を参照してください。

## 付録 {#appendix}

次の節では、外部オーディエンス APIを使用する際に使用できるエラーコードを示します。

| プラットフォームエラーコード | ステータスコード | メッセージ | 説明 |
| ------------------- | ----------- | ------- | ----------- |
| 100910-400 | 400 | `BAD_REQUEST` | リクエストの検証中にエラーが発生したため、不正なリクエストが発生しました。 |
| 100911-400 | 400 | `BAD_REQUEST` | 無効なトークンが指定されています。 |
| 100920-401 | 401 | `UNAUTHORIZED` | ヘッダーがありません。 |
| 100921-401 | 401 | `UNAUTHORIZED` | 無効な`imsOrgId`が指定されています。 |
| 100922-401 | 401 | `UNAUTHORIZED` | 外部オーディエンス APIを使用する権限がありません。 |
| 100940-404 | 404 | `NOT_FOUND` | リクエストされたオーディエンスが見つかりませんでした。 |
| 100950-409 | 409 | `DUPLICATE_RESOURCE` | オーディエンスは既に存在します。 |
| 100960-422 | 422 | `UNPROCESSABLE_ENTITY` | リクエスト構造は有効ですが、論理エラーまたはセマンティックエラーにより処理できません。 |
| 100970-500 | 500 | `INTERNAL_SERVER_ERROR` | システム内でのリクエストの処理中に問題が発生しました。 |
| 100970-502 | 502 | `BAD_GATEWAY` | ダウンストリームの依存関係に問題があります。 |
