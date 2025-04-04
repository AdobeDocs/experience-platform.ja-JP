---
solution: Experience Platform
title: Flow Service API を使用した宛先接続の編集
type: Tutorial
description: Flow Service API を使用して宛先接続の様々なコンポーネントを編集する方法について説明します。
exl-id: d6d27d5a-e50c-4170-bb3a-c4cbf2b46653
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 26%

---

# Flow Service API を使用した宛先接続の編集

このチュートリアルでは、宛先接続の様々なコンポーネントの編集手順を説明します。 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、認証資格情報の更新、場所のエクスポートなどを行う方法を説明します。

>[!NOTE]
>
> このチュートリアルで説明する編集操作は、現在 Flow Service API でのみサポートされています。

## はじめに {#get-started}

このチュートリアルでは、有効なデータフロー ID が必要です。 有効なデータフロー ID がない場合は、このチュートリアルの内容を試す前に、[ 宛先カタログ ](../catalog/overview.md) から宛先を選択し、[ 宛先に接続 ](../ui/connect-destination.md) および [ データをアクティブ化 ](../ui/activation-overview.md) の手順に従ってください。

>[!NOTE]
>
> このチュートリアルでは *フロー* および *データフロー* という用語が同じ意味で使用されています。 このチュートリアルのコンテキストでは、同じ意味を持ちます。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ 宛先 ](../home.md):[!DNL Destinations] は、Adobe Experience Platformからのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [ サンドボックス ](../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してデータフローを正常に更新するために必要な追加情報を示しています。

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

Experience Platform API を呼び出すには、まず[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service] に属するリソースを含む、Experience Platformのすべてのリソースは、特定の仮想サンドボックスに分離されます。 Experience Platform API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>`x-sandbox-name` ヘッダーが指定されていない場合、リクエストは `prod` サンドボックスで解決されます。

ペイロード（`POST`、`PUT`、`PATCH`）を含むすべてのリクエストには、次のような追加のメディアタイプヘッダーが必要です。

* `Content-Type: application/json`

## データフローの詳細の検索 {#look-up-dataflow-details}

宛先接続を編集する最初の手順は、フロー ID を使用してデータフローの詳細を取得することです。 `/flows` エンドポイントに対して GET リクエストを実行することで、既存のデータフローの現在の詳細を表示できます。

>[!TIP]
>
>Experience Platform UI を使用して、宛先の目的のデータフロー ID を取得できます。 **[!UICONTROL 宛先]**/**[!UICONTROL 参照]** に移動し、目的の宛先データフローを選択して、右側のパネルで宛先 ID を見つけます。 宛先 ID は、次の手順でフロー ID として使用する値です。
>
> ![Experience Platform UI を使用した宛先 ID の取得 ](/help/destinations/assets/api/edit-destination/get-destination-id.png)

>[!BEGINSHADEBOX]

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

リクエストが成功した場合は、バージョン、一意の ID （`id`）およびその他の関連情報を含む、現在のデータフローの詳細が返されます。 このチュートリアルで最も関連するのは、以下の応答でハイライト表示されているターゲット接続 ID とベース接続 ID です。 次の節では、これらの ID を使用して、宛先接続の様々なコンポーネントを更新します。

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

## ターゲット接続コンポーネント （ストレージの場所およびその他のコンポーネント）の編集 {#patch-target-connection}

ターゲット接続のコンポーネントは、宛先によって異なります。 例えば、[!DNL Amazon S3] の宛先の場合、バケットとファイルの書き出し先のパスを更新できます。 [!DNL Pinterest] の宛先の場合は [!DNL Pinterest Advertiser ID] を更新でき、[!DNL Google Customer Match] の場合は [!DNL Pinterest Account ID] を更新できます。

ターゲット接続のコンポーネントを更新するには、`/targetConnections/{TARGET_CONNECTION_ID}` エンドポイントに `PATCH` リクエストを実行し、その際にターゲット接続 ID、バージョン、使用する新しい値を指定します。 前の手順で、目的の宛先に対する既存のデータフローを調べた際に、ターゲット接続 ID を取得しました。

>[!IMPORTANT]
>
>`PATCH` リクエストを行う場合、`If-Match` ヘッダーは必須です。 このヘッダーの値は、更新するターゲット接続の一意のバージョンです。 etag の値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> etag 値の最新バージョンを取得するには、`/targetConnections/{TARGET_CONNECTION_ID}` エンドポイントに対してGET リクエストを実行します。`{TARGET_CONNECTION_ID}` は、更新するターゲット接続 ID です。
>
> `PATCH` リクエストを行う場合は、以下の例のように、`If-Match` ヘッダーの値を必ず二重引用符で囲みます。

様々なタイプの宛先に対して、ターゲット接続仕様のパラメーターを更新する例を以下に示します。 ただし、宛先のパラメーターを更新するための一般的なルールは次のとおりです。

接続のデータフロー ID を取得/ ターゲット接続 ID を取得/目的のパラメーターの更新された値を使用してターゲット接続を `PATCH` 得します。

>[!BEGINSHADEBOX]

**API 形式**

```http
PATCH /targetConnections/{TARGET_CONNECTION_ID}
```

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

次のリクエストは、[[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) 宛先接続の `bucketName` および `path` パラメーターを更新します。

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

正常な応答では、ターゲット接続 ID と更新された Etag が返されます。 更新を検証するには、ターゲット接続 ID を指定する際に [!DNL Flow Service] API へGET リクエストを行います。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Google Ad Manager およびGoogle Ad Manager 360]

**リクエスト**

次のリクエストは、[[!DNL Google Ad Manager]](/help/destinations/catalog/advertising/google-ad-manager.md) 接続または [[!DNL Google Ad Manager 360]  宛先 ](/help/destinations/catalog/advertising/google-ad-manager-360-connection.md#destination-details) 接続のパラメーターを更新して、新しい [**[!UICONTROL  オーディエンス ID をオーディエンス名に追加 ]**](/help/release-notes/2023/april-2023.md#destinations) フィールドを追加します。

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

正常な応答は、ターゲット接続 ID と更新された etag を返します。 更新を検証するには、ターゲット接続 ID を指定する際に [!DNL Flow Service] API へGET リクエストを行います。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Pinterest]

**リクエスト**

次のリクエストは、[[!DNL Pinterest]  宛先接続 ](/help/destinations/catalog/advertising/pinterest.md#parameters) の `advertiserId` パラメーターを更新します。

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

正常な応答は、ターゲット接続 ID と更新された etag を返します。 更新を検証するには、ターゲット接続 ID を指定する際に [!DNL Flow Service] API へGET リクエストを行います。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!ENDTABS]

>[!ENDSHADEBOX]

## ベース接続コンポーネント（認証パラメーターおよびその他のコンポーネント）の編集 {#patch-base-connection}

宛先の資格情報を更新する場合は、ベース接続を編集します。 ベース接続のコンポーネントは、宛先によって異なります。 例えば、[!DNL Amazon S3] の宛先の場合、アクセスキーと秘密鍵を [!DNL Amazon S3] の場所に更新できます。

ベース接続のコンポーネントを更新するには、`/connections` エンドポイントに `PATCH` リクエストを実行し、その際にベース接続 ID、バージョン、使用する新しい値を指定します。

ベース接続 ID を取得したのは、[ 前の手順 ](#look-up-dataflow-details) で、既存のデータフローを目的の宛先に調査してパラメーター `baseConnection` を取得したときです。

>[!IMPORTANT]
>
>`PATCH` リクエストを行う場合、`If-Match` ヘッダーは必須です。 このヘッダーの値は、更新するベース接続の一意のバージョンです。 etag の値は、データフロー、ベース接続など、フローエンティティが正常に更新されるたびに更新されます。
>
> Etag 値の最新バージョンを取得するには、`/connections/{BASE_CONNECTION_ID}` エンドポイントに対してGET リクエストを実行します。`{BASE_CONNECTION_ID}` は、更新するベース接続 ID です。
>
> `PATCH` リクエストを行う場合は、以下の例のように、`If-Match` ヘッダーの値を必ず二重引用符で囲みます。

様々なタイプの宛先に対して、ベース接続仕様のパラメーターを更新する例を以下に示します。 ただし、宛先のパラメーターを更新するための一般的なルールは次のとおりです。

接続のデータフロー ID を取得/ ベース接続 ID を取得/目的のパラメーターの更新された値を使用してベース接続を `PATCH` します。

>[!BEGINSHADEBOX]

**API 形式**

```http
PATCH /connections/{BASE_CONNECTION_ID}
```

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

次のリクエストは、[[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) 宛先接続の `accessId` および `secretKey` パラメーターを更新します。

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

正常な応答では、ベース接続 ID と更新された etag が返されます。ベース接続 ID を指定した状態で [!DNL Flow Service] API に対してGET リクエストを実行すると、更新を確認することができます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Azure Blob]

**リクエスト**

次のリクエストは、[[!DNL Azure Blob] destination](/help/destinations/catalog/cloud-storage/azure-blob.md#authenticate) 接続のパラメーターを更新して、Azure Blob インスタンスへの接続に必要な接続文字列を更新します。

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

正常な応答では、ベース接続 ID と更新された etag が返されます。ベース接続 ID を指定した状態で [!DNL Flow Service] API に対してGET リクエストを実行すると、更新を確認することができます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!ENDTABS]

>[!ENDSHADEBOX]

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience Platform API の一般的なエラーメッセージの原則に従っています。 エラー応答の解釈について詳しくは、Experience Platform トラブルシューティングガイドの [API ステータスコード ](/help/landing/troubleshooting.md#api-status-codes) および [ リクエストヘッダーエラー ](/help/landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Flow Service] API を使用して宛先接続の様々なコンポーネントを更新する方法を学びました。 宛先について詳しくは、[ 宛先の概要 ](../home.md) を参照してください。
