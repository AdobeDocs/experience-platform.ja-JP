---
keywords: Experience Platform；ホーム；人気のトピック；フローサービス；宛先データフローの更新
solution: Experience Platform
title: Flow Service API を使用した宛先データフローの更新
type: Tutorial
description: このチュートリアルでは、宛先データフローの更新手順を説明します。 Flow Service API を使用して、データフローを有効または無効にする方法、基本情報を更新する方法、オーディエンスと属性を追加および削除する方法について説明します。
exl-id: 3f69ad12-940a-4aa1-a1ae-5ceea997a9ba
source-git-commit: c1d4a0586111d9cd8a66f4239f67f2f7e6ac8633
workflow-type: tm+mt
source-wordcount: '2404'
ht-degree: 33%

---

# Flow Service API を使用した宛先データフローの更新

このチュートリアルでは、宛先データフローの更新手順を説明します。 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、データフローを有効または無効にする方法、基本情報を更新する方法、オーディエンスと属性を追加および削除する方法について説明します。 Experience PlatformUI を使用した宛先データフローの編集について詳しくは、[ アクティベーションフローの編集 ](/help/destinations/ui/edit-activation.md) を参照してください。

## はじめに {#get-started}

このチュートリアルは、有効なフロー ID を保有しているユーザーを対象としています。有効なフロー ID がない場合は、このチュートリアルの内容を試す前に、[ 宛先カタログ ](../catalog/overview.md) から宛先を選択し、[ 宛先に接続 ](../ui/connect-destination.md) および [ データをアクティブ化 ](../ui/activation-overview.md) の手順に従ってください。

>[!NOTE]
>
> このチュートリアルでは *フロー* および *データフロー* という用語が同じ意味で使用されています。 このチュートリアルのコンテキストでは、も同じ意味を持ちます。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ 宛先 ](../home.md):[!DNL Destinations] は、Adobe Experience Platformからのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してデータフローを正常に更新するために必要な追加情報を示しています。

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service] に属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>`x-sandbox-name` ヘッダーが指定されていない場合、リクエストは `prod` サンドボックスで解決されます。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## データフローの詳細の検索 {#look-up-dataflow-details}

宛先データフローを更新する最初の手順は、フロー ID を使用してデータフローの詳細を取得することです。 `/flows` エンドポイントに対して GET リクエストを実行することで、既存のデータフローの現在の詳細を表示できます。

**API 形式**

```http
GET /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 取得する宛先データフローの一意の `id` 値。 |

**リクエスト**

次のリクエストでは、フロー ID に関する情報を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、バージョン、一意の ID （`id`）およびその他の関連情報を含む、現在のデータフローの詳細が返されます。

```json
{
   "items":[
      {
         "id":"226fb2e1-db69-4760-b67e-9e671e05abfc",
         "createdAt":"{CREATED_AT}",
         "updatedAt":"{UPDATED_BY}",
         "createdBy":"{CREATED_BY}",
         "updatedBy":"{UPDATED_BY}",
         "createdClient":"{CREATED_CLIENT}",
         "updatedClient":"{UPDATED_CLIENT}",
         "sandboxId":"{SANDBOX_ID}",
         "sandboxName":"prod",
         "imsOrgId":"{ORG_ID}",
         "name":"2021 winter campaign",
         "description":"ACME company holiday campaign for high fidelity customers",
         "flowSpec":{
            "id":"71471eba-b620-49e4-90fd-23f1fa0174d8",
            "version":"1.0"
         },
         "state":"enabled",
         "version":"\"8b0351ca-0000-0200-0000-61c4d6700000\"",
         "etag":"\"8b0351ca-0000-0200-0000-61c4d6700000\"",
         "sourceConnectionIds":[
            "5e45582a-5336-4ea1-9ec9-d0004a9f344a"
         ],
         "targetConnectionIds":[
            "8ce3dc63-3766-4220-9f61-51d2f8f14618"
         ],
         "inheritedAttributes":{
            "sourceConnections":[
               {
                  "id":"5e45582a-5336-4ea1-9ec9-d0004a9f344a",
                  "connectionSpec":{
                     "id":"8a9c3494-9708-43d7-ae3f-cda01e5030e1",
                     "version":"1.0"
                  },
                  "baseConnection":{
                     "id":"0a82f29f-b457-47f7-bb30-33856e2ae5aa",
                     "connectionSpec":{
                        "id":"8a9c3494-9708-43d7-ae3f-cda01e5030e1",
                        "version":"1.0"
                     }
                  },
                  "typeInfo":{
                     "type":"ProfileFragments",
                     "id":"ups"
                  }
               }
            ],
            "targetConnections":[
               {
                  "id":"8ce3dc63-3766-4220-9f61-51d2f8f14618",
                  "connectionSpec":{
                     "id":"0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
                     "version":"1.0"
                  },
                  "baseConnection":{
                     "id":"7fbf542b-83ed-498f-8838-8fde0c4d4d69",
                     "connectionSpec":{
                        "id":"0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
                        "version":"1.0"
                     }
                  }
               }
            ]
         },
         "transformations":[
            {
               "name":"GeneralTransform",
               "params":{
                  "profileSelectors":{
                     "selectors":[
                        {
                           "type":"JSON_PATH",
                           "value":{
                              "path":"Email",
                              "operator":"EXISTS",
                              "identity":{
                                 "namespace":"Email"
                              },
                              "mapping":{
                                 "sourceType":"text/x.schema-path",
                                 "source":"Email",
                                 "destination":"Email",
                                 "identity":false,
                                 "primaryIdentity":false,
                                 "functionVersion":0,
                                 "copyModeMapping":false,
                                 "sourceAttribute":"Email",
                                 "destinationXdmPath":"Email"
                              }
                           }
                        },
                        {
                           "type":"JSON_PATH",
                           "value":{
                              "path":"person.name.firstName",
                              "operator":"EXISTS",
                              "mapping":{
                                 "sourceType":"text/x.schema-path",
                                 "source":"person.name.firstName",
                                 "destination":"person.name.firstName",
                                 "identity":false,
                                 "primaryIdentity":false,
                                 "functionVersion":0,
                                 "copyModeMapping":false,
                                 "sourceAttribute":"person.name.firstName",
                                 "destinationXdmPath":"person.name.firstName"
                              }
                           }
                        },
                        {
                           "type":"JSON_PATH",
                           "value":{
                              "path":"person.name.lastName",
                              "operator":"EXISTS",
                              "mapping":{
                                 "sourceType":"text/x.schema-path",
                                 "source":"person.name.lastName",
                                 "destination":"person.name.lastName",
                                 "identity":false,
                                 "primaryIdentity":false,
                                 "functionVersion":0,
                                 "copyModeMapping":false,
                                 "sourceAttribute":"person.name.lastName",
                                 "destinationXdmPath":"person.name.lastName"
                              }
                           }
                        },
                        {
                           "type":"JSON_PATH",
                           "value":{
                              "path":"personalEmail.address",
                              "operator":"EXISTS",
                              "mapping":{
                                 "sourceType":"text/x.schema-path",
                                 "source":"personalEmail.address",
                                 "destination":"personalEmail.address",
                                 "identity":false,
                                 "primaryIdentity":false,
                                 "functionVersion":0,
                                 "copyModeMapping":false,
                                 "sourceAttribute":"personalEmail.address",
                                 "destinationXdmPath":"personalEmail.address"
                              }
                           }
                        },
                        {
                           "type":"JSON_PATH",
                           "value":{
                              "path":"segmentMembership.status",
                              "operator":"EXISTS",
                              "mapping":{
                                 "sourceType":"text/x.schema-path",
                                 "source":"segmentMembership.status",
                                 "destination":"segmentMembership.status",
                                 "identity":false,
                                 "primaryIdentity":false,
                                 "functionVersion":0,
                                 "copyModeMapping":false,
                                 "sourceAttribute":"segmentMembership.status",
                                 "destinationXdmPath":"segmentMembership.status"
                              }
                           }
                        }
                     ],
                     "mandatoryFields":[
                        "Email",
                        "person.name.firstName",
                        "person.name.lastName"
                     ],
                     "primaryFields":[
                        {
                           "identityNamespace":"Email",
                           "fieldType":"IDENTITY"
                        }
                     ]
                  },
                  "segmentSelectors":{
                     "selectors":[
                        {
                           "type":"PLATFORM_SEGMENT",
                           "value":{
                              "id":"9f7d37fd-7039-4454-94ef-2b0cd6c3206a",
                              "name":"Interested in Mountain Biking",
                              "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                              "exportMode":"DAILY_FULL_EXPORT",
                              "schedule":{
                                 "frequency":"ONCE",
                                 "startDate":"2021-12-25",
                                 "startTime":"20:00"
                              },
                              "createTime":"1640289901",
                              "updateTime":"1640289901"
                           }
                        },
                        {
                           "type":"PLATFORM_SEGMENT",
                           "value":{
                              "id":"f52a3785-2e7c-40a7-8137-9be99af7794e",
                              "name":"Birth year 1970",
                              "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                              "exportMode":"DAILY_FULL_EXPORT",
                              "schedule":{
                                 "frequency":"DAILY",
                                 "startDate":"2021-12-23",
                                 "endDate":"2021-12-31",
                                 "startTime":"20:00"
                              },
                              "createTime":"1640289901",
                              "updateTime":"1640289901"
                           }
                        },
                        {
                           "type":"PLATFORM_SEGMENT",
                           "value":{
                              "id":"6caa79b9-39e0-4c37-892b-5061cdca2377",
                              "name":"Account Leads",
                              "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                              "exportMode":"FIRST_FULL_THEN_INCREMENTAL",
                              "schedule":{
                                 "frequency":"DAILY",
                                 "startDate":"2021-12-23",
                                 "endDate":"2021-12-31",
                                 "startTime":"20:00"
                              },
                              "createTime":"1640289901",
                              "updateTime":"1640289901"
                           }
                        },
                        {
                           "type":"PLATFORM_SEGMENT",
                           "value":{
                              "id":"4c41c318-9e8c-4a4f-b880-877cdd629fc7",
                              "name":"Batch export for autumn campaign",
                              "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                              "exportMode":"FIRST_FULL_THEN_INCREMENTAL",
                              "schedule":{
                                 "frequency":"EVERY_6_HOURS",
                                 "startDate":"2022-01-05",
                                 "endDate":"2022-12-30",
                                 "startTime":"20:00"
                              },
                              "createTime":"1640289901",
                              "updateTime":"1640289901"
                           }
                        }
                     ]
                  }
               }
            }
         ]
      }
   ]
```

## データフロー名と説明の更新 {#update-dataflow}

データフローの名前と説明を更新するには、フロー ID、バージョンおよび使用する新しい値を指定して、[!DNL Flow Service] API にPATCHリクエストを実行します。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCH リクエストを行う際に必要です。このヘッダーの値は、更新するデータフローの一意のバージョンです。 ETag の値は、データフローが正常に更新されるたびに更新されます。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストは、データフローの名前と説明を更新します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
            {
                "op": "replace",
                "path": "/name",
                "value": "2021/2022 winter campaign"
            },
            {
                "op": "replace",
                "path": "/description",
                "value": "ACME company holiday campaign for high fidelity customers and prospects"
            }
        ]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するフローの部分を定義します。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフローを有効または無効にする {#enable-disable-dataflow}

有効にすると、データフローはプロファイルを宛先に書き出します。 データフローはデフォルトで有効になっていますが、プロファイル書き出しを一時停止するには無効にします。

[!DNL Flow Service] API に宛先リクエストを実行し、フローを更新するステートを指定することで、既存のPOSTデータフローを有効または無効にできます。

**API 形式**

```http
POST /flows/{FLOW_ID}/action?op=enable or disable
```

**リクエスト**

次のリクエストは、データフローの状態を有効に更新します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc/action?op=enable' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

次のリクエストは、データフローの状態を無効に更新します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc/action?op=disable' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフローへのオーディエンスの追加 {#add-segment}

宛先データフローにオーディエンスを追加するには、[!DNL Flow Service] API にPATCHリクエストを実行し、その際にフロー ID、バージョンおよび追加するオーディエンスを指定します。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストでは、既存の宛先データフローに新しいオーディエンスが追加されます。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
   {
      "op":"add",
      "path":"/transformations/0/params/segmentSelectors/selectors/-",
      "value":{
         "type":"PLATFORM_SEGMENT",
         "value":{
            "id":"2d79d0d8-724f-49fc-a09d-d1dec338c93c",
            "name":"Winter 2021/2022 campaign",
            "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%SEGMENT_NAME%_%DATETIME(YYYYMMdd_HHmmss)%_custom-text",
            "exportMode":"DAILY_FULL_EXPORT",
            "schedule":{
               "startDate":"2022-01-05",
               "frequency":"DAILY",
               "triggerType": "AFTER_SEGMENT_EVAL",
               "endDate":"2022-03-10"
            }
         }
      }
   }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。データフローにオーディエンスを追加するには、`add` 操作を使用します。 |
| `path` | 更新するフローの部分を定義します。オーディエンスをデータフローに追加するときは、例で指定したパスを使用します。 |
| `value` | パラメーターの更新に使用する新しい値。 |
| `id` | 宛先データフローに追加するオーディエンスの ID を指定します。 |
| `name` | **（オプション）**. 宛先データフローに追加するオーディエンスの名前を指定します。 このフィールドは必須ではなく、名前を指定しなくてもオーディエンスを宛先データフローに正常に追加できます。 |
| `filenameTemplate` | *バッチ宛先* の場合のみ。 このフィールドは、Amazon S3、SFTP、Azure Blob などのバッチファイル書き出し宛先のデータフローにオーディエンスを追加する場合にのみ必要です。 <br> このフィールドは、宛先に書き出すファイルのファイル名形式を決定します。 <br> 以下のオプションを利用できます。<br> <ul><li>`%DESTINATION_NAME%`：必須。書き出されるファイルには、宛先名が含まれます。</li><li>`%SEGMENT_ID%`：必須。書き出されるファイルには、書き出されたオーディエンスの ID が含まれます。</li><li>`%SEGMENT_NAME%`: **（オプション）**。 書き出されるファイルには、書き出されたオーディエンスの名前が含まれます。</li><li>`DATETIME(YYYYMMdd_HHmmss)` または `%TIMESTAMP%`:**（オプション）**。 ファイルが Experience Platform で生成された時刻を含めるには、これら 2 つのオプションのいずれかを選択します。</li><li>`custom-text`: **（オプション）**。 ファイル名の末尾に追加したいカスタムテキストでこのプレースホルダーを置き換えます。</li></ul> <br> ファイル名の設定について詳しくは、バッチ宛先の有効化に関するチュートリアルの「[ファイル名の設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)」の節を参照してください。 |
| `exportMode` | *バッチ宛先* の場合のみ。 このフィールドは、Amazon S3、SFTP、Azure Blob などのバッチファイル書き出し宛先のデータフローにオーディエンスを追加する場合にのみ必要です。 <br> 必須。 `"DAILY_FULL_EXPORT"` または `"FIRST_FULL_THEN_INCREMENTAL"` を選択します。この 2 つのオプションについて詳しくは、バッチ宛先の有効化に関するチュートリアルの「[完全なファイルのエクスポート](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files)」および「[増分ファイルのエクスポート](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files)」を参照してください。 |
| `startDate` | オーディエンスが宛先へのプロファイルの書き出しを開始する日付を選択します。 |
| `frequency` | *バッチ宛先* の場合のみ。 このフィールドは、Amazon S3、SFTP、Azure Blob などのバッチファイル書き出し宛先のデータフローにオーディエンスを追加する場合にのみ必要です。 <br> 必須。<br> <ul><li>`"DAILY_FULL_EXPORT"` エクスポートモードの場合は、`ONCE` または `DAILY` を選択できます。</li><li>`"FIRST_FULL_THEN_INCREMENTAL"` エクスポートモードの場合は、`"DAILY"`、`"EVERY_3_HOURS"`、`"EVERY_6_HOURS"`、`"EVERY_8_HOURS"`、`"EVERY_12_HOURS"` を選択できます。</li></ul> |
| `triggerType` | *バッチ宛先* の場合のみ。 このフィールドは、`frequency` セレクターで `"DAILY_FULL_EXPORT"` モードを選択する場合にのみ必要です。 <br> 必須。<br> <ul><li>毎日の Platform バッチセグメント化ジョブが完了した直後にアクティベーションジョブを実行するには、「`"AFTER_SEGMENT_EVAL"`」を選択します。 これにより、アクティベーションジョブが実行されると、最新のプロファイルが確実に宛先に書き出されます。</li><li>`"SCHEDULED"` を選択して、特定の時間にアクティベーションジョブを実行します。 これにより、Experience Platformプロファイルデータは毎日同じ時刻に書き出されます。アクティベーションジョブが開始される前にバッチセグメント化ジョブが完了しているかどうかに応じて、書き出すプロファイルが最新ではない場合があります。 このオプションを選択する場合は、`startTime` を追加して、日次書き出しが発生する UTC の時間を示す必要もあります。</li></ul> |
| `endDate` | *バッチ宛先* の場合のみ。 このフィールドは、Amazon S3、SFTP、Azure Blob などのバッチファイル書き出し宛先のデータフローにオーディエンスを追加する場合にのみ必要です。 <br> `"exportMode":"DAILY_FULL_EXPORT"` と `"frequency":"ONCE"` を選択している場合は適用されません。 <br> オーディエンスメンバーが宛先への書き出しを停止する日付を設定します。 |
| `startTime` | *バッチ宛先* の場合のみ。 このフィールドは、Amazon S3、SFTP、Azure Blob などのバッチファイル書き出し宛先のデータフローにオーディエンスを追加する場合にのみ必要です。 <br> 必須。 オーディエンスのメンバーを含んだファイルを生成し、宛先に書き出す時間を選択します。 |

**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフローからのオーディエンスの削除 {#remove-segment}

既存の宛先データフローからオーディエンスを削除するには、[!DNL Flow Service] API に対してPATCHリクエストを行い、削除するオーディエンスのフロー ID、バージョンおよびインデックスセレクターを指定します。 インデックス作成は `0` から開始されます。 例えば、後述するリクエストのサンプルでは、データフローから 1 番目と 2 番目のオーディエンスを削除します。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストは、既存の宛先データフローから 2 つのオーディエンスを削除します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
{
   "op":"remove",
   "path":"/transformations/0/params/segmentSelectors/selectors/0",
   "value":{
      "type":"PLATFORM_SEGMENT",
      "value":{
      }
   }
},
{
   "op":"remove",
   "path":"/transformations/0/params/segmentSelectors/selectors/1",
   "value":{
      "type":"PLATFORM_SEGMENT",
      "value":{
      }
   }
}
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。データフローからオーディエンスを削除するには、`remove` 操作を使用します。 |
| `path` | オーディエンスセレクターのインデックスに基づいて、宛先データフローから削除する既存のオーディエンスを指定します。 データフロー内のオーディエンスの順序を取得するには、`/flows` エンドポイントへのGET呼び出しを実行し、`transformations.segmentSelectors` プロパティを調べます。 データフローの最初のオーディエンスを削除するには、`"path":"/transformations/0/params/segmentSelectors/selectors/0"` を使用します。 |


**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフロー内のオーディエンスのコンポーネントを更新 {#update-segment}

既存の宛先データフロー内のオーディエンスのコンポーネントを更新できます。 例えば、書き出し頻度を変更したり、ファイル名のテンプレートを編集したりできます。 これを行うには、更新するオーディエンスのフロー ID、バージョンおよびインデックスセレクターを指定したうえで、[!DNL Flow Service] API に対してPATCHリクエストを実行します。 インデックス作成は `0` から開始されます。 例えば、以下のリクエストは、データフローの 9 番目のオーディエンスを更新します。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

既存の宛先データフロー内のオーディエンスを更新する場合、最初にGET操作を実行して、更新するオーディエンスの詳細を取得する必要があります。 次に、更新するフィールドだけでなく、ペイロード内のすべてのオーディエンス情報を指定します。 次の例では、カスタムテキストがファイル名テンプレートの最後に追加され、書き出しスケジュール頻度が 6 時間から 12 時間に更新されます。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
   {
      "op":"replace",
      "path":"/transformations/0/params/segmentSelectors/selectors/8",
      "value":{
         "type":"PLATFORM_SEGMENT",
         "value":{
            "id":"4c41c318-9e8c-4a4f-b880-877cdd629fc7",
            "name":"Batch export for autumn campaign",
            "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%_custom-text",
            "exportMode":"FIRST_FULL_THEN_INCREMENTAL",
            "schedule":{
               "frequency":"EVERY_12_HOURS",
               "startDate":"2022-01-05",
               "endDate":"2022-01-30",
               "startTime":"20:00"
            },
            "createTime":"1640289901",
            "updateTime":"1640289901"
         }
      }
   }
]'
```

ペイロードのプロパティについては、[ データフローへのオーディエンスの追加 ](#add-segment) の節を参照してください。


**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

データフローで更新できるオーディエンスコンポーネントの他の例については、以下の例を参照してください。

## オーディエンスの書き出しモードを、スケジュール済みからオーディエンスの評価後に更新します {#update-export-mode}

+++ クリックすると、オーディエンスの書き出しが、毎日の指定された時間にアクティブ化されてから、Platform バッチセグメント化ジョブが完了した後に毎日のアクティブ化へと更新される例が表示されます。

オーディエンスは毎日 16:00 UTC に書き出されます。

```json
{
  "type": "PLATFORM_SEGMENT",
  "value": {
    "id": "b1e50e8e-a6e2-420d-99e8-a80deda2082f",
    "name": "12JAN22-AEP-NA-NTC-90D-MW",
    "filenameTemplate": "%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
    "exportMode": "DAILY_FULL_EXPORT"
    "schedule": {
      "frequency": "DAILY",
      "triggerType": "SCHEDULED",
      "startDate": "2022-01-13",
      "endDate": "2023-01-13",
      "startTime":"16:00"
    },
    "createTime": "1642041770",
    "updateTime": "1642615573"
  }
}
```

オーディエンスは、毎日のバッチセグメント化ジョブが完了した後に、毎日書き出されます。

```json
{
  "type": "PLATFORM_SEGMENT",
  "value": {
    "id": "b1e50e8e-a6e2-420d-99e8-a80deda2082f",
    "name": "12JAN22-AEP-NA-NTC-90D-MW",
    "filenameTemplate": "%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
    "exportMode": "DAILY_FULL_EXPORT"
    "schedule": {
      "frequency": "DAILY",
      "triggerType": "AFTER_SEGMENT_EVAL",
      "startDate": "2022-01-13",
      "endDate": "2023-01-13"
    },
    "createTime": "1642041770",
    "updateTime": "1642615573"
  }
}
```

+++

## ファイル名にフィールドを追加するように、ファイル名テンプレートを更新します {#update-filename-template}

+++ クリックすると、ファイル名にフィールドを追加するようにファイル名テンプレートが更新される例が表示されます

書き出されるファイルには、宛先名とExperience Platformオーディエンス ID が含まれます

```json
{
  "type": "PLATFORM_SEGMENT",
  "value": {
    "id": "b1e50e8e-a6e2-420d-99e8-a80deda2082f",
    "name": "12JAN22-AEP-NA-NTC-90D-MW",
    "filenameTemplate": "%DESTINATION_NAME%_%SEGMENT_ID%",
    "exportMode": "DAILY_FULL_EXPORT"
    "schedule": {
      "frequency": "DAILY",
      "triggerType": "SCHEDULED",
      "startDate": "2022-01-13",
      "endDate": "2023-01-13",
      "startTime":"16:00"
    },
    "createTime": "1642041770",
    "updateTime": "1642615573"
  }
}
```

書き出されるファイルには、宛先名、Experience Platformオーディエンス ID、Experience Platformがファイルを生成した日時、ファイルの末尾に追加されたカスタムテキストが含まれます。


```json
{
  "type": "PLATFORM_SEGMENT",
  "value": {
    "id": "b1e50e8e-a6e2-420d-99e8-a80deda2082f",
    "name": "12JAN22-AEP-NA-NTC-90D-MW",
    "filenameTemplate": "%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%_%this is custom text%",
    "exportMode": "DAILY_FULL_EXPORT"
    "schedule": {
      "frequency": "DAILY",
      "triggerType": "SCHEDULED",
      "startDate": "2022-01-13",
      "endDate": "2023-01-13",
      "startTime":"16:00"
    },
    "createTime": "1642041770",
    "updateTime": "1642615573"
  }
}
```

+++

## データフローへのプロファイル属性の追加 {#add-profile-attribute}

宛先データフローにプロファイル属性を追加するには、[!DNL Flow Service] API にPATCHリクエストを行い、追加するフロー ID、バージョンおよびプロファイル属性を指定します。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストでは、既存の宛先データフローに新しいプロファイル属性が追加されます。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
    {
    "op":"add",
    "path":"/transformations/0/params/profileSelectors/selectors/-",
    "value":{
        "type":"JSON_PATH",
        "value":{
            "path":"mobilePhone.status"
        }
    }
    }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。データフローにプロファイル属性を追加するには、`add` 操作を使用します。 |
| `path` | 更新するフローの部分を定義します。プロファイル属性をデータフローに追加するときは、例で指定したパスを使用します。 |
| `value.path` | データフローに追加するプロファイル属性の値。 |

**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフローからのプロファイル属性の削除 {#remove-profile-attribute}

既存の宛先データフローからプロファイル属性を削除するには、[!DNL Flow Service] API にPATCHリクエストを行い、削除するプロファイル属性のフロー ID、バージョンおよびインデックスセレクターを指定します。 インデックス作成は `0` から開始されます。 例えば、後述のサンプルリクエストでは、データフローから 5 番目のプロファイル属性を削除します。


**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストでは、既存の宛先データフローからプロファイル属性が削除されます。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
    {
    "op":"remove",
    "path":"/transformations/0/params/profileSelectors/selectors/4",
    "value":{
        "type":"JSON_PATH",
        "value":{
            "path":"mobilePhone.status"
        }
    }
    }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。データフローからオーディエンスを削除するには、`remove` 操作を使用します。 |
| `path` | オーディエンスセレクターのインデックスに基づいて、宛先データフローから削除する既存のプロファイル属性を指定します。 データフロー内のプロファイル属性の順序を取得するには、`/flows` エンドポイントへのGET呼び出しを実行し、`transformations.profileSelectors` プロパティを調べます。 データフローの最初のオーディエンスを削除するには、`"path":"transformations/0/params/segmentSelectors/selectors/0/"` を使用します。 |


**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "2edc08ac-4df5-4fe6-936f-81a19ce92f5c",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従っています。 エラー応答の解釈について詳しくは、Platform トラブルシューティングガイドの [API ステータスコード ](/help/landing/troubleshooting.md#api-status-codes) および [ リクエストヘッダーエラー ](/help/landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルに従うと、API を使用してオーディエンスやプロファイル属性を追加または削除するなど、宛先データフローの様々なコンポーネントを更新する方法 [!DNL Flow Service] 学ぶことができます。 宛先について詳しくは、[ 宛先の概要 ](../home.md) を参照してください。
