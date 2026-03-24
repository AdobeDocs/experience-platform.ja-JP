---
solution: Experience Platform
title: Flow Service APIを使用した宛先接続の編集
type: Tutorial
description: Flow Service APIを使用して、宛先接続の様々なコンポーネントを編集する方法を説明します。
exl-id: d6d27d5a-e50c-4170-bb3a-c4cbf2b46653
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1604'
ht-degree: 24%

---

# Flow Service APIを使用した宛先接続の編集

このチュートリアルでは、宛先接続の様々なコンポーネントを編集する手順について説明します。 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して、認証資格情報の更新や場所の書き出しを行う方法を説明します。

>[!NOTE]
>
> このチュートリアルで説明する編集操作は、Experience Platform UIでもサポートされています。 詳しくは、[UIで宛先を編集する方法](/help/destinations/ui/edit-destination.md)に関するチュートリアルを参照してください。

## はじめに {#get-started}

このチュートリアルでは、有効なデータフローIDが必要です。 有効なデータフローIDがない場合は、このチュートリアルを試す前に、[宛先カタログ &#x200B;](../catalog/overview.md)から目的の宛先を選択し、[宛先に接続](../ui/connect-destination.md)および[&#x200B; データをアクティブ化](../ui/activation-overview.md)するための手順に従ってください。

>[!NOTE]
>
> このチュートリアルでは、*flow*&#x200B;と&#x200B;*dataflow*&#x200B;という用語が同じ意味で使用されています。 このチュートリアルのコンテキストでは、それらは同じ意味を持ちます。

このチュートリアルでは、[!DNL Adobe Experience Platform]の次のコンポーネントについて理解している必要もあります。

* [宛先](../home.md): [!DNL Destinations]は、[!DNL Adobe Experience Platform]からのデータをシームレスにアクティブ化できる宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [&#x200B; サンドボックス &#x200B;](../../sandboxes/home.md): Experience Platformは、1つのExperience Platform インスタンスを個別のバーチャル環境に分割して、デジタルエクスペリエンスアプリケーションの開発と進化に役立つバーチャルサンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してデータフローを正常に更新するために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

Experience Platform APIを呼び出すには、まず[認証チュートリアル &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。 認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service]に属するリソースを含むExperience Platformのすべてのリソースは、特定のバーチャルサンドボックスに分離されます。 Experience Platform APIへのすべてのリクエストには、操作を実行するサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>`x-sandbox-name` ヘッダーが指定されていない場合、リクエストは`prod` サンドボックスで解決されます。

ペイロード （`POST`、`PUT`、`PATCH`）を含むすべてのリクエストには、追加のメディアタイプヘッダーが必要です。

* `Content-Type: application/json`

## データフローの詳細の検索 {#look-up-dataflow-details}

宛先接続を編集する最初の手順は、フローIDを使用してデータフローの詳細を取得することです。 `/flows` エンドポイントに対して GET リクエストを実行することで、既存のデータフローの現在の詳細を表示できます。

>[!TIP]
>
>Experience Platform UIを使用して、宛先の目的のデータフローIDを取得できます。 **[!UICONTROL Destinations]** > **[!UICONTROL Browse]**&#x200B;に移動し、目的の宛先データフローを選択して、右側のパネルで宛先IDを見つけます。 宛先IDは、次の手順でフローIDとして使用する値です。
>
> ![Experience Platform UI](/help/destinations/assets/api/edit-destination/get-destination-id.png)を使用して宛先IDを取得

>[!BEGINSHADEBOX]

**API 形式**

```http
GET /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 取得する宛先データフローの一意の`id`値。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、フローIDに関する情報を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/flows/226fb2e1-db69-4760-b67e-9e671e05abfc' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、バージョン、一意の識別子（`id`）、およびその他の関連情報を含む、データフローの現在の詳細が返されます。 このチュートリアルに最も関連するのは、以下の応答で強調表示されているターゲット接続とベース接続IDです。 次の節では、これらのIDを使用して、宛先接続のさまざまなコンポーネントを更新します。

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

## ターゲット接続コンポーネント（ストレージの場所およびその他のコンポーネント）の編集 {#patch-target-connection}

ターゲット接続のコンポーネントは、宛先によって異なります。 例えば、[!DNL Amazon S3]の宛先の場合、ファイルが書き出されるバケットとパスを更新できます。 [!DNL Pinterest]宛先の場合は[!DNL Pinterest Advertiser ID]を更新でき、[!DNL Google Customer Match]の場合は[!DNL Pinterest Account ID]を更新できます。

ターゲット接続のコンポーネントを更新するには、ターゲット接続ID、バージョン、および使用する新しい値を指定しながら、`PATCH` エンドポイントに`/targetConnections/{TARGET_CONNECTION_ID}` リクエストを実行します。 必要な宛先への既存のデータフローを検査した場合、前の手順でターゲット接続IDを取得したことを忘れないでください。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、`PATCH` リクエストを行う際に必要です。 このヘッダーの値は、更新するターゲット接続の一意のバージョンです。 etag値は、データフロー、ターゲット接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> etag値の最新バージョンを取得するには、`/targetConnections/{TARGET_CONNECTION_ID}` エンドポイントに対してGET リクエストを実行します。`{TARGET_CONNECTION_ID}`は、更新する対象のコネクション IDです。
>
> `If-Match`要求を行う際は、以下の例のように、`PATCH` ヘッダーの値を二重引用符で囲んでください。

以下に、様々なタイプの宛先に対して、ターゲット接続仕様のパラメーターを更新する例をいくつか示します。 ただし、宛先のパラメーターを更新する一般的なルールは次のとおりです。

接続のデータフローIDを取得/目的のパラメーターの値を更新してターゲット接続を取得> `PATCH`。

>[!BEGINSHADEBOX]

**API 形式**

```http
PATCH /targetConnections/{TARGET_CONNECTION_ID}
```

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

次のリクエストは、`bucketName`宛先接続の`path`および[[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) パラメーターを更新します。

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

{style="table-layout:auto"}

**応答**

応答が成功すると、ターゲット接続IDと更新されたEtagが返されます。 ターゲット接続IDを指定しながら、[!DNL Flow Service] APIに対してGET リクエストを行うことで、更新を検証できます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Google Ad ManagerとGoogle Ad Manager 360]

**リクエスト**

次のリクエストは、[[!DNL Google Ad Manager]](/help/destinations/catalog/advertising/google-ad-manager.md)または[[!DNL Google Ad Manager 360] 宛先](/help/destinations/catalog/advertising/google-ad-manager-360-connection.md#destination-details)接続のパラメーターを更新して、新しい[**[!UICONTROL Append audience ID to audience name]**](/help/release-notes/2023/april-2023.md#destinations) フィールドを追加します。

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

{style="table-layout:auto"}

**応答**

応答が成功すると、ターゲット接続IDと更新されたタグが返されます。 ターゲット接続IDを指定しながら、[!DNL Flow Service] APIに対してGET リクエストを行うことで、更新を検証できます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Pinterest]

**リクエスト**

次のリクエストは、`advertiserId`宛先接続[[!DNL Pinterest] の](/help/destinations/catalog/advertising/pinterest.md#parameters) パラメーターを更新します。

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

{style="table-layout:auto"}

**応答**

応答が成功すると、ターゲット接続IDと更新されたタグが返されます。 ターゲット接続IDを指定しながら、[!DNL Flow Service] APIに対してGET リクエストを行うことで、更新を検証できます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!ENDTABS]

>[!ENDSHADEBOX]

## ベース接続コンポーネント（認証パラメーターおよびその他のコンポーネント）の編集 {#patch-base-connection}

宛先の資格情報を更新する場合は、ベース接続を編集します。 ベース接続のコンポーネントは、宛先によって異なります。 例えば、[!DNL Amazon S3]宛先の場合、アクセスキーと秘密鍵を[!DNL Amazon S3]の場所に更新できます。

ベース接続のコンポーネントを更新するには、ベース接続ID、バージョン、および使用する新しい値を指定しながら、`PATCH` エンドポイントに`/connections` リクエストを実行します。

パラメーター[の目的の宛先に対する既存のデータフローを検査した場合、](#look-up-dataflow-details)前の手順`baseConnection`でベース接続IDを取得したことを忘れないでください。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、`PATCH` リクエストを行う際に必要です。 このヘッダーの値は、更新するベース接続の一意のバージョンです。 etag値は、データフロー、ベース接続などのフローエンティティが正常に更新されるたびに更新されます。
>
> Etag値の最新バージョンを取得するには、`/connections/{BASE_CONNECTION_ID}` エンドポイントに対してGET リクエストを実行します。ここで、`{BASE_CONNECTION_ID}`は、更新するベース接続IDです。
>
> `If-Match`要求を行う際は、以下の例のように、`PATCH` ヘッダーの値を二重引用符で囲んでください。

以下に、様々なタイプの宛先に対するベース接続仕様のパラメーターの更新の例をいくつか示します。 ただし、宛先のパラメーターを更新する一般的なルールは次のとおりです。

接続のデータフローIDを取得/必要なパラメーターの値が更新されたベース接続でベース接続IDを取得> `PATCH`。

>[!BEGINSHADEBOX]

**API 形式**

```http
PATCH /connections/{BASE_CONNECTION_ID}
```

>[!BEGINTABS]

>[!TAB Amazon S3]

**リクエスト**

次のリクエストは、`accessId`宛先接続の`secretKey`および[[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) パラメーターを更新します。

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

{style="table-layout:auto"}

**応答**

正常な応答では、ベース接続 ID と更新された etag が返されます。ベース接続IDを指定しながら、[!DNL Flow Service] APIに対してGET リクエストを行うことで、更新を検証できます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!TAB Azure Blob]

**リクエスト**

次のリクエストは、[[!DNL Azure Blob] destination](/help/destinations/catalog/cloud-storage/azure-blob.md#authenticate)接続のパラメーターを更新して、Azure Blob インスタンスへの接続に必要な接続文字列を更新します。

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

{style="table-layout:auto"}

**応答**

正常な応答では、ベース接続 ID と更新された etag が返されます。ベース接続IDを指定しながら、[!DNL Flow Service] APIに対してGET リクエストを行うことで、更新を検証できます。

```json
{
    "id": "b2cb1407-3114-441c-87ea-2c1a3c84d0b0",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

>[!ENDTABS]

>[!ENDSHADEBOX]

## API エラー処理 {#api-error-handling}

このチュートリアルのAPI エンドポイントは、一般的なExperience Platform API エラーメッセージの原則に従っています。 エラー応答の解釈について詳しくは、Experience Platform トラブルシューティングガイドの[API ステータスコード &#x200B;](/help/landing/troubleshooting.md#api-status-codes)および[&#x200B; リクエストヘッダーエラー](/help/landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Flow Service] APIを使用して宛先接続の様々なコンポーネントを更新する方法について説明しました。 宛先について詳しくは、[宛先の概要](../home.md)を参照してください。
