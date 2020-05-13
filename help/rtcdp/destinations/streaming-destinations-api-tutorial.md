---
keywords: Experience Platform;home;popular topics; API tutorials; streaming destinations API; Real-time CDP
solution: Experience Platform
title: ストリーミング送信先への接続とデータのアクティブ化
topic: tutorial
translation-type: tm+mt
source-git-commit: 47e03d3f58bd31b1aec45cbf268e3285dd5921ea
workflow-type: tm+mt
source-wordcount: '1861'
ht-degree: 1%

---


# APIを使用して、ストリーミング送信先に接続し、アドビのリアルタイム顧客データプラットフォームでデータをアクティブにします。

>[!NOTE]
>
>Adobe Real-time CDP [!DNL Amazon Kinesis][!DNL Azure Event Hubs] の宛先と宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

このチュートリアルでは、API呼び出しを使用してAdobe Experience Platformデータに接続し、ストリーミングクラウドストレージ先([Amazon Kinesis](/help/rtcdp/destinations/amazon-kinesis-destination.md) または [Azureイベントハブ](/help/rtcdp/destinations/azure-event-hubs-destination.md))に接続し、新しく作成した宛先にデータフローを作成し、新しく作成した宛先にデータをアクティブ化する方法を説明します。

このチュートリアルでは、すべての例で [!DNL Amazon Kinesis] 宛先を使用しますが、手順はで同じで [!DNL Azure Event Hubs]す。

![概要 — ストリーミングの宛先を作成し、セグメントをアクティブにする手順](/help/rtcdp/destinations/assets/flow-prelim.png)

アドビのReal-time CDPのユーザーインターフェイスを使用して宛先に接続し、データをアクティブにする場合は、「宛先の [接続](../../rtcdp/destinations/connect-destination.md) 」および「宛先へのプロファイルとセグメントのアク [ティブ化](../../rtcdp/destinations/activate-destinations.md) 」のチュートリアルを参照してください。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
* [カタログサービス](../../catalog/home.md): カタログは、エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。
* [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Adobe Real-time CDPのストリーミング宛先に対するデータをアクティブ化するために知っておく必要がある追加情報について、以下の節で説明します。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、セグメントを接続およびアクティブ化する宛先のタイプに応じて、次の資格情報を準備する必要があります。

* 接続 [!DNL Amazon Kinesis] の場合： `accessKeyId`、 `secretKey``region` または `connectionUrl`
* 接続 [!DNL Azure Event Hubs] の場合： `sasKeyName`, `sasKey``namespace`

### サンプルAPI呼び出しの読み取り {#reading-sample-api-calls}

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必須ヘッダーと任意選択ヘッダーの値の収集 {#gather-values}

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](/help/tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのリソースは、特定の仮想サンドボックスに分離できます。 プラットフォームAPIへのリクエストでは、操作を実行するサンドボックスの名前とIDを指定できます。 これらはオプションのパラメーターです。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!N注意]
>エクスペリエンスプラットフォームのサンドボックスについて詳しくは、 [サンドボックスの概要に関するドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

### Swaggerドキュメント {#swagger-docs}

Swaggerのこのチュートリアルでは、すべてのAPI呼び出しに関する付属のリファレンスドキュメントを参照できます。 https://platform.adobe.io/data/foundation/flowservice/swagger#/を参照してください。 このチュートリアルとSwaggerのドキュメントページを並行して使用することをお勧めします。

## 使用可能なストリーミング先のリストの取得 {#get-the-list-of-available-streaming-destinations}

![宛先手順の概要手順1](/help/rtcdp/destinations/assets/step1-create-streaming-destination-api.png)

最初の手順として、データをアクティブにするストリーミング先を決定する必要があります。 最初に、接続してセグメントをアクティブにできる、使用可能な宛先のリストをリクエストする呼び出しを実行します。 使用可能な宛先のリストを返すには、次のGETリクエストを `connectionSpecs` エンドポイントに実行します。

**API形式**

```http
GET /connectionSpecs
```

**リクエスト**

```
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
--header 'accept: application/json' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```


**応答**

成功した応答には、使用可能な宛先とその一意の識別子(`id`)のリストが含まれます。 使用する宛先の値を保存します。この値は、以降の手順で必要になります。 例えば、またはにセグメントを接続して配信する場合 [!DNL Amazon Kinesis] は、応答で次のスニペットを探し [!DNL Azure Event Hubs]ます。

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

## エクスペリエンスプラットフォームデータに接続する {#connect-to-your-experience-platform-data}

![宛先手順の概要手順2](/help/rtcdp/destinations/assets/step2-create-streaming-destination-api.png)

次に、Experience Platformデータに接続し、プロファイルデータを書き出して、目的のデータを書き出し先でアクティブ化できるようにする必要があります。 これは、次に説明する2つのサブステップで構成されます。

1. まず、ベース接続を設定して、エクスペリエンスプラットフォームでのデータへのアクセスを許可するための呼び出しを実行する必要があります。
2. 次に、ベース接続IDを使用して、別の呼び出しを行い、ソース接続を作成して、エクスペリエンスプラットフォームデータとの接続を確立します。


### エクスペリエンスプラットフォームでのデータへのアクセスを許可する

**API形式**

```http
POST /connections
```

**リクエスト**

```
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME} \
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


* `{CONNECTION_SPEC_ID}`: ユニファイドプロファイルングサービスの接続仕様IDを使用します — `8a9c3494-9708-43d7-ae3f-cda01e5030e1`。

**応答**

成功した応答には、ベース接続の固有な識別子(`id`)が含まれます。 この値は、次の手順でソース接続を作成する際に必要となる場合に保存します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### エクスペリエンスプラットフォームデータに接続する

**API形式**

```http
POST /sourceConnections
```

**リクエスト**

```
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
            "name": "Connecting to Unified Profile Service",
            "description": "Optional",
            "connectionSpec": {
                "id": "{CONNECTION_SPEC_ID}",
                "version": "1.0"
            },
            "baseConnectionId": "{BASE_CONNECTION_ID}",
            "data": {
                "format": "json"
            },
            "params" : {}
}'
```

* `{BASE_CONNECTION_ID}`: 前の手順で取得したIDを使用します。
* `{CONNECTION_SPEC_ID}`: ユニファイドプロファイルングサービスの接続仕様IDを使用します — `8a9c3494-9708-43d7-ae3f-cda01e5030e1`。

**応答**

正常な応答は、新しく作成されたUnified Connection Serviceへのソース接続の固有な識別子(`id`)を返します。 これにより、エクスペリエンスプラットフォームのデータに正常に接続できたことが確認できます。 この値は、後の手順で必要となる場合に格納します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```


## Connect to streaming destination {#connect-to-streaming-destination}

![宛先手順の概要手順3](/help/rtcdp/destinations/assets/step3-create-streaming-destination-api.png)

この手順では、目的のストリーミング宛先への接続を設定します。 これは、次に説明する2つのサブステップで構成されます。

1. まず、ベース接続を設定して、ストリーミング宛先へのアクセスを許可する呼び出しを実行する必要があります。
2. 次に、ベース接続IDを使用して別の呼び出しを行い、ターゲット接続を作成します。この呼び出しでは、エクスポートされたデータが配信されるストレージアカウント内の場所と、エクスポートされるデータの形式を指定します。

### ストリーミング先へのアクセスを許可する

**API形式**

```http
POST /connections
```

**リクエスト**

```
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connection for Amazon Kinesis/ Azure Event Hubs",
    "description": "your company's holiday campaign",
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

* `{CONNECTION_SPEC_ID}`: 使用可能な宛先のリストの [取得で取得した接続仕様IDを使用します](#get-the-list-of-available-destinations)。
* `{AUTHENTICATION_CREDENTIALS}`: ストリーミング先の名前を入力します。例： `Amazon Kinesis authentication credentials` または `Azure Event Hubs authentication credentials`。
* `{ACCESS_ID}`: *Amazon Kinesis接続の場合。* Amazon Kinesisストレージの場所のアクセスID。
* `{SECRET_KEY}`: *Amazon Kinesis接続の場合。* Amazon Kinesisストレージの場所の秘密キー。
* `{REGION}`: *Amazon Kinesis接続の場合。* Adobe Real-time CDPがデータをストリーミングするAmazon Kinesisアカウント内の領域。
* `{SAS_KEY_NAME}`: *Azureイベントハブ接続の場合。* SASキー名を入力します。 SASキーを使用し [!DNL Azure Event Hubs] たときの認証については、 [Microsoftのドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)。
* `{SAS_KEY}`: *Azureイベントハブ接続の場合。* SASキーを入力します。 SASキーを使用し [!DNL Azure Event Hubs] たときの認証については、 [Microsoftのドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)。
* `{EVENT_HUB_NAMESPACE}`: *Azureイベントハブ接続の場合。* Adobe Real-time CDPがデータをストリーミングするAzureイベントハブ名前空間に入力します。 詳細については、Microsoftのドキュメントの「イベントハブの [作成」名前空間](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) を参照してください。

**応答**

成功した応答には、ベース接続の固有な識別子(`id`)が含まれます。 この値は、次の手順でターゲット接続を作成する際に必要となる場合に格納します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### ストレージの場所とデータ形式を指定する

**API形式**

```http
POST /targetConnections
```

**リクエスト**

```
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Amazon Kinesis/ Azure Event Hubs target connection",
    "description": "Connection to Amazon Kinesis/ Azure Event Hubs",
    "baseConnection": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "{CONNECTION_SPEC_ID}",
        "version": "1.0"
    },
    "data": {
        "format": "json",
    },
    "params": { // use these values for Amazon Kinesis connections
        "stream": "{NAME_OF_DATA_STREAM}", 
        "region": "{REGION}"
    },
    "params": { // use these values for Azure Event Hubs connections
        "eventHubName": "{EVENT_HUB_NAME}",
        "namespace": "EVENT_HUB_NAMESPACE"
    }
}'
```

* `{BASE_CONNECTION_ID}`: 上記の手順で取得したベース接続IDを使用します。
* `{CONNECTION_SPEC_ID}`: 使用可能な宛先のリストを [取得する手順で取得した接続仕様を使用します](#get-the-list-of-available-destinations)。
* `{NAME_OF_DATA_STREAM}`: *Amazon Kinesis接続の場合。* Amazon Kinesisアカウント内の既存のデータストリームの名前を指定します。 Adobe Real-time CDPは、このストリームにデータをエクスポートします。
* `{REGION}`: *Amazon Kinesis接続の場合。* Adobe Real-time CDPがデータをストリーミングするAmazon Kinesisアカウント内の領域。
* `{EVENT_HUB_NAME}`: *Azureイベントハブ接続の場合。* Azureイベントハブ名に、Adobe Real-time CDPがデータをストリーミングする場所を入力します。 詳しくは、Microsoftのドキュメントの「イベントハブの [作成](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) 」を参照してください。
* `{EVENT_HUB_NAMESPACE}`: *Azureイベントハブ接続の場合。* Adobe Real-time CDPがデータをストリーミングするAzureイベントハブ名前空間に入力します。 詳細については、Microsoftのドキュメントの「イベントハブの [作成」名前空間](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) を参照してください。

**応答**

正常な応答を返すと、新たに作成されたターゲット接続からストリーミングの宛先への一意の識別子(`id`)が返されます。 この値は、後の手順で必要となるので保存します。

```json
{
    "id": "12ab90c7-519c-4291-bd20-d64186b62da8"
}
```

## データフローの作成

![宛先手順の概要：手順4](/help/rtcdp/destinations/assets/step4-create-streaming-destination-api.png)

前の手順で取得したIDを使用して、Experience Platformデータと、データをアクティブ化する先との間にデータフローを作成できるようになりました。 この手順は、後でデータが流れるパイプラインを構築し、エクスペリエンスプラットフォームと目的の宛先の間に行くことと考えてください。

データフローを作成するには、以下に示すように、ペイロード内で以下に示す値を指定しながら、POSTリクエストを実行します。

次のPOST要求を実行して、データフローを作成します。

**API形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
'https://platform.adobe.io/data/foundation/flowservice/flows' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'x-api-key: {API_KEY}' \
-H 'x-gw-ims-org-id: {IMS_ORG}' \
-H 'x-sandbox-name: {SANDBOX_NAME}' \
-H 'Content-Type: application/json' \
-d  '{
   
        "name": "Create dataflow to Amazon Kinesis/ Azure Event Hubs",
        "description": "This operation creates a dataflow to Amazon Kinesis/ Azure Event Hubs",
        "flowSpec": {
            "id": "{FLOW_SPEC_ID}",
            "version": "1.0"
        },
        "sourceConnectionIds": [
            "{SOURCE_CONNECTION_ID}"
        ],
        "targetConnectionIds": [
            "{TARGET_CONNECTION_ID}"
        ]
    }
```

* `{FLOW_SPEC_ID}`: 接続先のストリーミングのフローを使用します。 フロー仕様を取得するには、エンドポイントでGET操作を実行し `flowspecs` ます。 Swaggerのドキュメントはこちらを参照してください。 https://platform.adobe.io/data/foundation/flowservice/swagger#/Flow%20Specs%20API/getFlowSpecs 応答で、接続先のストリーミング先の対応するIDを探 `upsTo` してコピーします。
* `{SOURCE_CONNECTION_ID}`: 手順「エクスペリエンスプラットフォームへの [接続」で取得したソース接続IDを使用します](#connect-to-your-experience-platform-data)。
* `{TARGET_CONNECTION_ID}`: 「ストリーミング宛先への [接続」の手順で取得したターゲット接続IDを使用します](#connect-to-streaming-destination)。

**応答**

正常な応答は、新しく作成されたデータフローのID(`id``etag`)と、 両方の値を書き留めておきます。 次の手順で行うように、セグメントをアクティブにします。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい送信先にデータをアクティブにする

![宛先手順の概要：手順5](/help/rtcdp/destinations/assets/step5-create-streaming-destination-api.png)

すべての接続とデータフローを作成したら、プロファイルデータをストリーミングプラットフォームに対してアクティブ化できます。 この手順では、送信先に送信するセグメントとプロファイル属性を選択し、スケジュールを設定して送信先にデータを送信できます。

新しい宛先に対してセグメントをアクティブ化するには、次の例のようなJSON PATCH操作を実行する必要があります。 1回の呼び出しで複数のセグメントとプロファイル属性をアクティブ化できます。 JSON PATCHの詳細については、 [RFC仕様を参照してください](https://tools.ietf.org/html/rfc6902)。

**API形式**

```http
PATCH /flows
```

**リクエスト**

```
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
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
                "name": "Name of the segment that you are activating",
                "description": "Description of the segment that you are activating",
                "id": "{SEGMENT_ID}"
            }
        }
    },
        {
        "op": "add",
        "path": "/transformations/0/params/segmentSelectors/selectors/-",
        "value": {
            "type": "PLATFORM_SEGMENT",
            "value": {
                "name": "Name of the segment that you are activating",
                "description": "Description of the segment that you are activating",
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

* `{DATAFLOW_ID}`: 前の手順で取得したデータフローを使用します。
* `{ETAG}`: 前の手順で取得したetagを使用します。
* `{SEGMENT_ID}`: この宛先にエクスポートするセグメントIDを指定します。 アクティブ化するセグメントのセグメントIDを取得するには、https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/にアクセスし、左側のナビゲーションメニューで **Segmentation Service API** (Segmentation Service API `GET /segment/jobs` )を選択して、操作を探します。
* `{PROFILE_ATTRIBUTE}`: 例えば、 `"person.lastName"`

**応答**

202 OK応答を探します。 応答本文は返されません。 リクエストが正しいことを検証するには、次の手順「データフローを検証する」を参照してください。

## データフローの検証

![宛先手順の概要手順6](/help/rtcdp/destinations/assets/step6-create-streaming-destination-api.png)

このチュートリアルの最後の手順として、セグメントとプロファイル属性が実際にデータフローに正しくマッピングされていることを検証する必要があります。

これを検証するには、次のGETリクエストを実行します。

**API形式**

```http
GET /flows
```

**リクエスト**

```
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/flowservice/flows/{DATAFLOW_ID}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--header 'x-sandbox-name: prod' \
--header 'If-Match: "{ETAG}"' 
```

* `{DATAFLOW_ID}`: 前の手順のデータフローを使用します。
* `{ETAG}`: 前の手順のetagを使用します。

**応答**

返される応答には、前の手順で送信したセグメントとプロファイル属性を `transformations` パラメーターに含める必要があります。 応答内のサンプル `transformations` パラメーターは次のようになります。

```
"transformations": [
    {
        "name": "GeneralTransform",
        "params": {
            "profileSelectors": {
                "selectors": []
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

## 次の手順

このチュートリアルに従うと、Real-time CDPを優先ストリーミング先の1つに接続し、それぞれの宛先に対するデータ・フローを設定できます。 これで、送信データを、顧客分析や実行する他の任意のデータ操作の送信先で使用できるようになりました。 詳しくは、次のページを参照してください。

* [宛先の概要](../../rtcdp/destinations/destinations-overview.md)
* [宛先カタログの概要](../../rtcdp/destinations/destinations-catalog.md)