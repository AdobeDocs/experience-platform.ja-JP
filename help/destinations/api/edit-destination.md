---
solution: Experience Platform
title: フローサービス API を使用した宛先接続の編集
type: Tutorial
description: フローサービス API を使用して、宛先接続の様々なコンポーネントを編集する方法を説明します。
source-git-commit: 2afe330176c2b7734c38cf47be79960175060824
workflow-type: tm+mt
source-wordcount: '1580'
ht-degree: 33%

---

# フローサービス API を使用した宛先接続の編集

このチュートリアルでは、宛先接続の様々なコンポーネントを編集する手順を説明します。 認証資格情報を更新する方法、書き出し場所など、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

>[!NOTE]
>
> このチュートリアルで説明する編集操作は、現在、フローサービス API を通じてのみサポートされています。

## はじめに {#get-started}

このチュートリアルでは、有効なデータフロー ID が必要です。 有効なデータフロー ID がない場合は、 [宛先カタログ](../catalog/overview.md) そして、以下に示す手順に従います。 [宛先に接続](../ui/connect-destination.md) および [データをアクティブ化](../ui/activation-overview.md) このチュートリアルを試す前に

>[!NOTE]
>
> 用語 *流れ* および *データフロー* このチュートリアルでは、同じ意味で使用されます。 このチュートリアルのコンテキストでは、同じ意味を持ちます。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [宛先は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。](../home.md)[!DNL Destinations]宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Flow Service] API

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

Experience Platform内のすべてのリソース ( [!DNL Flow Service]は、特定の仮想サンドボックスに分離されています。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>この `x-sandbox-name` ヘッダーが指定されていない場合、リクエストは `prod` サンドボックス。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## データフローの詳細の検索 {#look-up-dataflow-details}

宛先接続を編集する最初の手順は、フロー ID を使用してデータフローの詳細を取得することです。 `/flows` エンドポイントに対して GET リクエストを実行することで、既存のデータフローの現在の詳細を表示できます。

>[!TIP]
>
>Experience PlatformUI を使用して、宛先の目的のデータフロー ID を取得できます。 に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]**&#x200B;をクリックし、目的の宛先データフローを選択し、右側のパネルで宛先 ID を見つけます。 宛先 ID は、次の手順でフロー ID として使用する値です。
>
> ![Experience PlatformUI を使用した宛先 ID の取得](/help/destinations/assets/api/edit-destination/get-destination-id.png)

>[!BEGINSHADEBOX]

**API 形式**

```http
GET /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 一意の `id` 取得する宛先データフローの値。 |

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

正常な応答は、バージョン、一意の識別子 (`id`)、およびその他の関連情報。 このチュートリアルで最も関連性の高いのは、以下の応答で強調表示されているターゲット接続とベース接続 ID です。 次の節でこれらの ID を使用して、宛先接続の様々なコンポーネントを更新します。

```json {line-numbers="true" start-line="1" highlight="27,38"}
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
            "shortened for brevity"
         ]
      }
   ]
```

>[!ENDSHADEBOX]

## ターゲット接続コンポーネント（ストレージの場所と他のコンポーネント）を編集 {#patch-target-connection}

ターゲット接続のコンポーネントは、宛先によって異なります。 例： [!DNL Amazon S3] の宛先を指定する場合、ファイルが書き出されるバケットとパスを更新できます。 の場合 [!DNL Pinterest] の宛先を更新するには、 [!DNL Pinterest Advertiser ID] および [!DNL Google Customer Match] 以下を更新します。 [!DNL Pinterest Account ID].

ターゲット接続のコンポーネントを更新するには、 `/targetConnections/{TARGET_CONNECTION_ID}` エンドポイントを使用して、ターゲット接続 ID、バージョンおよび使用する新しい値を指定します。 前の手順で、目的の宛先に対する既存のデータフローを調べた際に、ターゲット接続 ID を取得したことを忘れないでください。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCH リクエストを行う際に必要です。このヘッダーの値は、更新するターゲット接続の一意のバージョンです。 etag の値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> 最新バージョンの etag 値を取得するには、に対してGETリクエストを実行します。 `/targetConnections/{TARGET_CONNECTION_ID}` endpoint，ここで `{TARGET_CONNECTION_ID}` は、更新しようとしているターゲット接続 ID です。

次に、様々なタイプの宛先のターゲット接続仕様のパラメーターを更新する例をいくつか示します。 ただし、宛先のパラメーターを更新する一般的なルールは次のとおりです。

接続のデータフロー ID を取得し、ターゲット接続 ID を取得して、目的のパラメーターの更新された値を使用してターゲット接続をPATCHします。

>[!BEGINSHADEBOX]

**API 形式**

```http
PATCH /targetConnections/{TARGET_CONNECTION_ID}
```

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

次のリクエストは、 `bucketName` および `path` のパラメーター [[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) 宛先接続。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections/b2cb1407-3114-441c-87ea-2c1a3c84d0b0' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
  {
    "op": "replace",
    "path": "/params",
    "value": {
      "bucketName": "newBucketName",
      "path": "updatedPath"
    }
  }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するフローの部分を定義します。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、ターゲット接続 ID と更新された Etag を返します。 更新を検証するには、 [!DNL Flow Service] API を使用してターゲット接続 ID を指定します。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Google Ad Manager とGoogle Ad Manager 360]

**リクエスト**

次のリクエストは、 [[!DNL Google Ad Manager]](/help/destinations/catalog/advertising/google-ad-manager.md) または [[!DNL Google Ad Manager 360] 宛先](/help/destinations/catalog/advertising/google-ad-manager-360-connection.md#destination-details) 新しい [**[!UICONTROL セグメント名にセグメント ID を追加]**](/help/release-notes/2023/april-2023.md#destinations) フィールドに入力します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections/b2cb1407-3114-441c-87ea-2c1a3c84d0b0' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
  {
    "op": "add",
    "path": "/params/appendSegmentId",
    "value": true
  }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するフローの部分を定義します。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、ターゲット接続 ID と更新された etag を返します。 更新を検証するには、 [!DNL Flow Service] API を使用してターゲット接続 ID を指定します。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Pinterest]

**リクエスト**

次のリクエストは、 `advertiserId` のパラメーター [[!DNL Pinterest] 宛先接続](/help/destinations/catalog/advertising/pinterest.md#parameters).

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections/b2cb1407-3114-441c-87ea-2c1a3c84d0b0' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
  {
    "op": "replace",
    "path": "/params",
    "value": {
      "advertiser_id": "1234567890"
    }
  }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するフローの部分を定義します。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、ターゲット接続 ID と更新された etag を返します。 更新を検証するには、 [!DNL Flow Service] API を使用してターゲット接続 ID を指定します。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!ENDTABS]

>[!ENDSHADEBOX]

## 基本接続コンポーネント（認証パラメーターおよび他のコンポーネント）の編集 {#patch-base-connection}

宛先の資格情報を更新する場合は、ベース接続を編集します。 ベース接続のコンポーネントは、宛先によって異なります。 例： [!DNL Amazon S3] の宛先の場合、アクセスキーと秘密鍵を [!DNL Amazon S3] 場所。

ベース接続のコンポーネントを更新するには、 `/connections` エンドポイントを使用して、基本接続 ID、バージョンおよび使用する新しい値を指定します。

ベース接続 ID は [前の手順](#look-up-dataflow-details)（既存のデータフローを目的の宛先に対してパラメーターを調べた場合） `baseConnection`.

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCH リクエストを行う際に必要です。このヘッダーの値は、更新するベース接続の一意のバージョンです。 etag 値は、データフロー、ベース接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> 最新バージョンの Etag 値を取得するには、に対してGETリクエストを実行します。 `/connections/{BASE_CONNECTION_ID}` endpoint，ここで `{BASE_CONNECTION_ID}` は、更新しようとしているベース接続 ID です。

様々なタイプの宛先のベース接続仕様のパラメーターを更新する例を以下にいくつか示します。 ただし、宛先のパラメーターを更新する一般的なルールは次のとおりです。

接続のデータフロー ID を取得し、ベース接続 ID を取得して、必要なパラメーターの更新された値を使用してベース接続をPATCHします。

>[!BEGINSHADEBOX]

**API 形式**

```http
PATCH /connections/{BASE_CONNECTION_ID}
```

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

次のリクエストは、 `accessId` および `secretKey` のパラメーター [[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) 宛先接続。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections/b2cb1407-3114-441c-87ea-2c1a3c84d0b0' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
  {
    "op": "add",
    "path": "/auth/params",
    "value": {
      "accessId": "exampleAccessId",
      "secretKey": "exampleSecretKey"
    }
  }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するフローの部分を定義します。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答では、ベース接続 ID と更新された etag が返されます。更新を検証するには、 [!DNL Flow Service] API と同じです。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Azure Blob]

**リクエスト**

次のリクエストは、 [[!DNL Azure Blob] 宛先](/help/destinations/catalog/cloud-storage/azure-blob.md#authenticate) 接続を使用して Azure Blob インスタンスへの接続に必要な接続文字列を更新します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/targetConnections/b2cb1407-3114-441c-87ea-2c1a3c84d0b0' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
  {
    "op": "add",
    "path": "/auth/params",
    "value": {
      "connectionString": "updatedString"
    }
  }
]'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するフローの部分を定義します。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答では、ベース接続 ID と更新された etag が返されます。更新を検証するには、 [!DNL Flow Service] API と同じです。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!ENDTABS]

>[!ENDSHADEBOX]

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience PlatformAPI エラーメッセージの一般的な原則に従います。 参照： [API ステータスコード](/help/landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](/help/landing/troubleshooting.md#request-header-errors) エラー応答の解釈について詳しくは、『 Platform トラブルシューティングガイド』を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、 [!DNL Flow Service] API 宛先について詳しくは、 [宛先の概要](../home.md).
