---
keywords: Experience Platform；ホーム；人気の高いトピック；API チュートリアル；ストリーミング宛先 API;Platform
solution: Experience Platform
title: Adobe Experience Platformのフローサービス API を使用して、ストリーミング宛先に接続し、データをアクティブ化する
description: このドキュメントでは、Adobe Experience Platform API を使用したストリーミング先の作成について説明します
type: Tutorial
exl-id: 3e8d2745-8b83-4332-9179-a84d8c0b4400
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '2241'
ht-degree: 45%

---

# Flow Service API でストリーミング宛先に接続してデータを有効化する

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]**[アクセス制御権限](/help/access-control/home.md#permissions)が必要です。
>
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。
>
>[アクセス制御の概要](/help/access-control/ui/overview.md)を読むか、製品管理者に問い合わせて、必要な権限を取得してください。

このチュートリアルでは、API 呼び出しを使用してAdobe Experience Platformデータに接続する方法、ストリーミングクラウドストレージの宛先 ([Amazon Kinesis](../catalog/cloud-storage/amazon-kinesis.md) または [Azure イベントハブ](../catalog/cloud-storage/azure-event-hubs.md))、新しく作成した宛先へのデータフローを作成し、新しく作成した宛先へのデータをアクティブ化します。

このチュートリアルでは、 [!DNL Amazon Kinesis] の宛先はすべての例で同じですが、手順は [!DNL Azure Event Hubs].

![概要 — ストリーミング宛先の作成手順とオーディエンスのアクティブ化の手順](../assets/api/streaming-destination/overview.png)

Platform のユーザーインターフェイスを使用して宛先に接続し、データをアクティブ化する場合は、 [宛先の接続](../ui/connect-destination.md) および [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータのアクティブ化](../ui/activate-segment-streaming-destinations.md) チュートリアル

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
* [[!DNL Catalog Service]](../../catalog/home.md): [!DNL Catalog] は、Experience Platform内のデータの場所と系列のレコードのシステムです。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、Platform でストリーミング宛先に対してデータをアクティブ化する際に知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、接続してオーディエンスをアクティブ化する宛先の種類に応じて、次の資格情報を準備しておく必要があります。

* の場合 [!DNL Amazon Kinesis] 接続： `accessKeyId`, `secretKey`, `region` または `connectionUrl`
* の場合 [!DNL Azure Event Hubs] 接続： `sasKeyName`, `sasKey`, `namespace`

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーおよびオプションヘッダーの値の収集 {#gather-values}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

### Swagger のドキュメント {#swagger-docs}

このチュートリアルに含まれるすべての API 呼び出しについての参照ドキュメンは、Swagger のホームページにあります。詳しくは、 [Adobe I/Oに関するフローサービス API ドキュメント](https://www.adobe.io/experience-platform-apis/references/flow-service/). このチュートリアルと Swagger のドキュメントページを並行して使用することをお勧めします。

## 使用可能なストリーミング先のリストを取得する {#get-the-list-of-available-streaming-destinations}

![宛先手順の概要 - 手順 1](../assets/api/streaming-destination/step1.png)

最初の手順として、データをアクティブ化するストリーミング先を決定する必要があります。 まず、接続してオーディエンスをアクティブ化できる、使用可能な宛先のリストをリクエストする呼び出しを実行します。 `connectionSpecs` エンドポイントに次の GET リクエストを実行すると、使用可能な宛先のリストが返されます。

**API 形式**

```http
GET /connectionSpecs
```

**リクエスト**

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
--header 'accept: application/json' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```


**応答** 

リクエストが成功した場合、使用可能な宛先のリストと、その一意の ID（`id`）が返されます。使用する宛先の値を保存します。この値は、以降の手順で必要になります。例えば、オーディエンスを接続して配信する場合は、 [!DNL Amazon Kinesis] または [!DNL Azure Event Hubs]の場合、応答内で次のスニペットを探します。

```json
{
    "id": "86043421-563b-46ec-8e6c-e23184711bf6",
  "name": "Amazon Kinesis",
  ...
  ...
}

{
    "id": "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
  "name": "Azure Event Hubs",
  ...
  ...
}
```

## Experience Platform データへの接続  {#connect-to-your-experience-platform-data}

![宛先の指定手順の概要 - 手順 2](../assets/api/streaming-destination/step2.png)

次に、Experience Platform データに接続し、目的の宛先にプロファイルデータを書き出してアクティブ化できるようにする必要があります。そのためには、以下に示す 2 つの手順を実行します。

1. 最初に、Experience Platform のデータへのアクセスを認証する呼び出しを実行します。そのためには、ベース接続を設定します。
2. 次に、ベース接続 ID を使用して、ソース接続を作成する別の呼び出しを実行します。これで、Experience Platform データへの接続が確立されます。


### Experience Platform のデータへのアクセスを認証する

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
            "name": "Base connection to Experience Platform",
            "description": "This call establishes the connection to Experience Platform data",
            "connectionSpec": {
                "id": "{CONNECTION_SPEC_ID}",
                "version": "1.0"
            }
}'
```


* `{CONNECTION_SPEC_ID}`:プロファイルサービスの接続仕様 ID を使用します。 `8a9c3494-9708-43d7-ae3f-cda01e5030e1`.

**応答** 

リクエストが成功した場合、ベース接続の一意の ID（`id`）が返されます。この値は、次の手順でソース接続を作成する際に必要になるため保存します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### Experience Platform データへの接続  {#connect-to-platform-data}

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
            "name": "Connecting to Profile Service",
            "description": "Optional",
            "connectionSpec": {
                "id": "{CONNECTION_SPEC_ID}",
                "version": "1.0"
            },
            "baseConnectionId": "{BASE_CONNECTION_ID}",
            "data": {
                "format": "json"
            },
            "params": {}
}'
```

* `{BASE_CONNECTION_ID}`：前述の手順で取得した ID を使用します。
* `{CONNECTION_SPEC_ID}`:プロファイルサービスの接続仕様 ID を使用します。 `8a9c3494-9708-43d7-ae3f-cda01e5030e1`.

**応答**

正常な応答は、一意の識別子 (`id`) をクリックします。 識別子が返された場合、Experience Platform データに正常に接続できています。この値は、後の手順で必要になるため保存します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```


## ストリーミング先に接続 {#connect-to-streaming-destination}

![宛先手順の概要 - 手順 3](../assets/api/streaming-destination/step3.png)

この手順では、目的のストリーミング宛先への接続を設定します。 そのためには、以下に示す 2 つの手順を実行します。

1. まず、ベース接続を設定して、ストリーミング先へのアクセスを認証する呼び出しを実行する必要があります。
2. 次に、ベース接続 ID を使用して、ターゲット接続を作成する別の呼び出しを実行します。これで、書き出されたデータが送信されるストレージアカウント内の場所と、書き出されるデータの形式が指定されます。

### ストリーミング先へのアクセスを許可

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!IMPORTANT]
>
>以下の例には、というプレフィックスが付いたコードコメントが含まれています。 `//`. これらのコメントは、異なるストリーミング先に異なる値を使用する必要がある場所を強調表示します。 スニペットを使用する前にコメントを削除してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connection for Amazon Kinesis/ Azure Event Hubs",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "{_CONNECTION_SPEC_ID}",
        "version": "1.0"
    },
    "auth": {
        "specName": "{AUTHENTICATION_CREDENTIALS}",
        "params": { // use these values for Amazon Kinesis connections
            "accessKeyId": "{ACCESS_ID}",
            "secretKey": "{SECRET_KEY}",
            "region": "{REGION}"
        },
        "params": { // use these values for Azure Event Hubs connections
            "sasKeyName": "{SAS_KEY_NAME}",
            "sasKey": "{SAS_KEY}",
            "namespace": "{EVENT_HUB_NAMESPACE}"
        }        
    }
}'
```

* `{CONNECTION_SPEC_ID}`：手順「[使用可能な宛先のリストを取得する](#get-the-list-of-available-destinations)」で取得した接続仕様 ID を使用します。
* `{AUTHENTICATION_CREDENTIALS}`:ストリーミング先の名前を入力します。 `Aws Kinesis authentication credentials` または `Azure EventHub authentication credentials`.
* `{ACCESS_ID}`: *の場合 [!DNL Amazon Kinesis] 接続。* Amazon Kinesisストレージの場所のアクセス ID。
* `{SECRET_KEY}`: *の場合 [!DNL Amazon Kinesis] 接続。* Amazon Kinesisストレージの場所の秘密鍵。
* `{REGION}`: *の場合 [!DNL Amazon Kinesis] 接続。* の地域 [!DNL Amazon Kinesis] Platform がデータをストリーミングするアカウント。
* `{SAS_KEY_NAME}`: *の場合 [!DNL Azure Event Hubs] 接続。* SAS キー名を入力します。 認証の詳細 [!DNL Azure Event Hubs] に SAS キーを設定 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).
* `{SAS_KEY}`: *の場合 [!DNL Azure Event Hubs] 接続。* SAS キーを入力します。 認証の詳細 [!DNL Azure Event Hubs] に SAS キーを設定 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).
* `{EVENT_HUB_NAMESPACE}`: *の場合 [!DNL Azure Event Hubs] 接続。* 次の項目に入力： [!DNL Azure Event Hubs] namespace :Platform がデータをストリーミングする場所。 詳しくは、 [Event Hubs 名前空間の作成](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) 内 [!DNL Microsoft] ドキュメント。

**応答** 

リクエストが成功した場合、ベース接続の一意の ID（`id`）が返されます。この値は、次の手順でターゲット接続を作成する際に必要になるため保存します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### ストレージの場所とデータ形式を指定する

**API 形式**

```http
POST /targetConnections
```

**リクエスト**

>[!IMPORTANT]
>
>以下の例には、というプレフィックスが付いたコードコメントが含まれています。 `//`. これらのコメントは、異なるストリーミング先に異なる値を使用する必要がある場所を強調表示します。 スニペットを使用する前にコメントを削除してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Amazon Kinesis/ Azure Event Hubs target connection",
    "description": "Connection to Amazon Kinesis/ Azure Event Hubs",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "{CONNECTION_SPEC_ID}",
        "version": "1.0"
    },
    "data": {
        "format": "json"
    },
    "params": { // use these values for Amazon Kinesis connections
        "stream": "{NAME_OF_DATA_STREAM}", 
        "region": "{REGION}"
    },
    "params": { // use these values for Azure Event Hubs connections
        "eventHubName": "{EVENT_HUB_NAME}"
    }
}'
```

* `{BASE_CONNECTION_ID}`：前述の手順で取得したベース接続 ID を使用します。
* `{CONNECTION_SPEC_ID}`：手順「[使用可能な宛先のリストを取得する](#get-the-list-of-available-destinations)」で取得した接続仕様 ID を使用します。
* `{NAME_OF_DATA_STREAM}`: *の場合 [!DNL Amazon Kinesis] 接続。* 既存のデータストリームの名前を [!DNL Amazon Kinesis] アカウント Platform はこのストリームにデータを書き出します。
* `{REGION}`: *の場合 [!DNL Amazon Kinesis] 接続。* Amazon Kinesisアカウント内の、Platform がデータをストリーミングする地域です。
* `{EVENT_HUB_NAME}`: *の場合 [!DNL Azure Event Hubs] 接続。* 次の項目に入力： [!DNL Azure Event Hub] 名前 Platform がデータをストリーミングする場所。 詳しくは、 [イベントハブの作成](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) 内 [!DNL Microsoft] ドキュメント。

**応答**

正常な応答は、一意の識別子 (`id`) を使用します。 この値は、後の手順で必要になるため保存します。

```json
{
    "id": "12ab90c7-519c-4291-bd20-d64186b62da8"
}
```

## データフローの作成

![宛先の指定手順の概要 - 手順 4](../assets/api/streaming-destination/step4.png)

前述の手順で取得した ID を使用して、Experience Platform データと、データをアクティブ化する宛先との間でデータフローを作成できます。これは、パイプラインを構築する手順と考えることができます。データは、このパイプラインを通って Experience Platform と目的の宛先の間を流れます。

データフローを作成するには、以下のような POST リクエストを実行します。このとき、ペイロード内で以下に示す値を指定します。

次の POST リクエストを実行して、データフローを作成します。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
'https://platform.adobe.io/data/foundation/flowservice/flows' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'x-api-key: {API_KEY}' \
-H 'x-gw-ims-org-id: {ORG_ID}' \
-H 'x-sandbox-name: {SANDBOX_NAME}' \
-H 'Content-Type: application/json' \
-d  '{
  "name": "Azure Event Hubs",
  "description": "Azure Event Hubs",
  "flowSpec": {
    "id": "{FLOW_SPEC_ID}",
    "version": "1.0"
  },
  "sourceConnectionIds": [
    "{SOURCE_CONNECTION_ID}"
  ],
  "targetConnectionIds": [
    "{TARGET_CONNECTION_ID}"
  ],
  "transformations": [
    {
      "name": "GeneralTransform",
      "params": {
        "profileSelectors": {
          "selectors": [
            
          ]
        },
        "segmentSelectors": {
          "selectors": [
            
          ]
        }
      }
    }
  ]
}
```

* `{FLOW_SPEC_ID}`:プロファイルベースの宛先のフロー仕様 ID は、 `71471eba-b620-49e4-90fd-23f1fa0174d8`. 呼び出しでこの値を使用します。
* `{SOURCE_CONNECTION_ID}`：手順「[Experience Platform データへの接続](#connect-to-your-experience-platform-data)」で取得したソース接続 ID を使用します。
* `{TARGET_CONNECTION_ID}`:手順で取得したターゲット接続 ID を使用します [ストリーミング先に接続](#connect-to-streaming-destination).

**応答** 

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）と `etag` が返されます。両方の値をメモしておきます。次の手順でオーディエンスをアクティブ化する際に使用します。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい宛先に対してデータをアクティブ化にする {#activate-data}

![宛先の指定手順の概要 - 手順 5](../assets/api/streaming-destination/step5.png)

これで、すべての接続とデータフローを作成したので、ストリーミングプラットフォームに対してプロファイルデータをアクティブ化できます。 この手順では、宛先に送信するオーディエンスとプロファイル属性を選択し、スケジュールを設定して宛先にデータを送信します。

新しい宛先に対してオーディエンスをアクティブ化するには、次の例のような JSONPATCH操作を実行する必要があります。 1 回の呼び出しで、複数のオーディエンスおよびプロファイル属性をアクティブ化できます。 JSON パッチについて詳しくは、[RFC 仕様](https://tools.ietf.org/html/rfc6902)を参照してください。

**API 形式**

```http
PATCH /flows
```

**リクエスト**

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'Content-Type: application/json' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'If-Match: "{ETAG}"' \
--data-raw '[
  {
    "op": "add",
    "path": "/transformations/0/params/segmentSelectors/selectors/-",
    "value": {
      "type": "PLATFORM_SEGMENT",
      "value": {
        "name": "Name of the audience that you are activating",
        "description": "Description of the audience that you are activating",
        "id": "{SEGMENT_ID}"
      }
    }
  },
  {
    "op": "add",
    "path": "/transformations/0/params/profileSelectors/selectors/-",
    "value": {
      "type": "JSON_PATH",
      "value": {
        "operator": "EXISTS",
        "path": "{PROFILE_ATTRIBUTE}"
      }
    }
  }
]
```

| プロパティ | 説明 |
| --------- | ----------- |
| `{DATAFLOW_ID}` | URL 内で、前の手順で作成したデータフローの ID を使用します。 |
| `{ETAG}` | を取得 `{ETAG}` 前の手順の応答から、 [データフローの作成](#create-dataflow). 前の手順の応答形式は、エスケープ引用符で囲まれています。 リクエストのヘッダーでは、エスケープされていない値を使用する必要があります。 次の例を参照してください。 <br> <ul><li>応答の例： `"etag":""7400453a-0000-1a00-0000-62b1c7a90000""`</li><li>リクエストで使用する値： `"etag": "7400453a-0000-1a00-0000-62b1c7a90000"`</li></ul> <br>ETag の値は、データフローが正常に更新されるたびに更新されます。 |
| `{SEGMENT_ID}` | この宛先に書き出すオーディエンス ID を指定します。 アクティブ化するオーディエンスのオーディエンス ID の取得については、 [オーディエンス定義の取得](https://www.adobe.io/experience-platform-apis/references/segmentation/#operation/retrieveSegmentDefinitionById) (Experience PlatformAPI リファレンス ) |
| `{PROFILE_ATTRIBUTE}` | 例：`"person.lastName"` |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。オーディエンスをデータフローに追加するには、 `add` 操作。 |
| `path` | 更新するフローの部分を定義します。オーディエンスをデータフローに追加する場合は、例で指定したパスを使用します。 |
| `value` | パラメーターの更新に使用する新しい値。 |
| `id` | 宛先データフローに追加するオーディエンスの ID を指定します。 |
| `name` | *オプション*。宛先データフローに追加するオーディエンスの名前を指定します。 このフィールドは必須ではなく、名前を指定せずにオーディエンスを宛先データフローに正常に追加できます。 |

**応答** 

202 OK レスポンスを探します。レスポンスの本文は返されません。リクエストが正しいことを検証する方法については、次の手順「データフローの検証」を参照してください。

## データフローの検証

![宛先の指定手順の概要 - 手順 6](../assets/api/streaming-destination/step6.png)

このチュートリアルの最後の手順では、オーディエンスとプロファイル属性が実際にデータフローに正しくマッピングされていることを検証する必要があります。

検証するには、次の GET リクエストを実行します。

**API 形式**

```http
GET /flows
```

**リクエスト**

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'Content-Type: application/json' \
--header 'x-sandbox-name: prod' \
--header 'If-Match: "{ETAG}"' 
```

* `{DATAFLOW_ID}`：前述の手順で作成したデータフローを使用します。
* `{ETAG}`：前述の手順で取得した Etag を使用します。

**応答** 

返される応答は、 `transformations` パラメーターは、前の手順で送信したオーディエンスおよびプロファイル属性です。 レスポンス内のサンプル `transformations` パラメーターは次のようになります。

```json
"transformations": [
    {
        "name": "GeneralTransform",
        "params": {
            "profileSelectors": {
                        "selectors": [
                            {
                                "type": "JSON_PATH",
                                "value": {
                                    "path": "personalEmail.address",
                                    "operator": "EXISTS"
                                }
                            },
                            {
                                "type": "JSON_PATH",
                                "value": {
                                    "path": "person.lastname",
                                    "operator": "EXISTS"
                                }
                            }
                        ]
                    },
            "segmentSelectors": {
                "selectors": [
                    {
                        "type": "PLATFORM_SEGMENT",
                        "value": {
                            "name": "Men over 50",
                            "description": "",
                            "id": "72ddd79b-6b0a-4e97-a8d2-112ccd81bd02"
                        }
                    }
                ]
            }
        }
    }
],
```

**書き出されたデータ**

>[!IMPORTANT]
>
> 手順のプロファイル属性とオーディエンスに加えて、 [新しい宛先に対してデータをアクティブ化する](#activate-data)、で書き出されたデータ [!DNL AWS Kinesis] および [!DNL Azure Event Hubs] id マップに関する情報も含まれます。 これは、エクスポートされたプロファイルの ID を表します ( 例： [ECID](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html?lang=ja)、モバイル ID、Google ID、電子メールアドレスなど )。 以下の例を参照してください。

```json
{
  "person": {
    "email": "yourstruly@adobe.com"
  },
  "segmentMembership": {
    "ups": {
      "72ddd79b-6b0a-4e97-a8d2-112ccd81bd02": {
        "lastQualificationTime": "2020-03-03T21:24:39Z",
        "status": "exited"
      },
      "7841ba61-23c1-4bb3-a495-00d695fe1e93": {
        "lastQualificationTime": "2020-03-04T23:37:33Z",
        "status": "realized"
      }
    }
  },
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```

## 使用 [!DNL Postman] ストリーミング宛先に接続するためのコレクション  {#collections}

このチュートリアルで説明するストリーミング先により効率的に接続するには、 [[!DNL Postman]](https://www.postman.com/).

[!DNL Postman] は、API 呼び出しをおこなったり、事前に定義された呼び出しおよび環境のライブラリを管理したりするのに使用できるツールです。

この特定のチュートリアルでは、次の手順に従います [!DNL Postman] コレクションが添付されました：

* [!DNL AWS Kinesis] [!DNL Postman] collection
* [!DNL Azure Event Hubs] [!DNL Postman] collection

クリック [ここ](../assets/api/streaming-destination/DestinationPostmanCollection.zip) コレクションアーカイブをダウンロードします。

各コレクションには、 [!DNL AWS Kinesis]、および [!DNL Azure Event Hub]、それぞれ。

### の使用方法 [!DNL Postman] コレクション {#how-to-use-postman-collections}

接続されたを使用して宛先に正常に接続するには [!DNL Postman] コレクションを使用するには、次の手順に従います。

* ダウンロードとインストール [!DNL Postman];
* [ダウンロード](../assets/api/streaming-destination/DestinationPostmanCollection.zip) 添付されたコレクションを解凍します。
* 対応するフォルダーからにコレクションを読み込む [!DNL Postman];
* この記事の手順に従って、環境変数を入力します。
* を実行します。 [!DNL API] からのリクエスト [!DNL Postman]（この記事の手順に基づく）

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience PlatformAPI エラーメッセージの一般的な原則に従います。 参照： [API ステータスコード](/help/landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](/help/landing/troubleshooting.md#request-header-errors) エラー応答の解釈について詳しくは、『 Platform トラブルシューティングガイド』を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、Platform を目的のストリーミング宛先の 1 つに接続し、それぞれの宛先へのデータフローを設定しました。 これで、送信データを、顧客分析や他の実行が必要なデータ操作の宛先で使用できるようになりました。 詳しくは、以下のページを参照してください。

* [宛先の概要](../home.md)
* [宛先カタログの概要](../catalog/overview.md)
