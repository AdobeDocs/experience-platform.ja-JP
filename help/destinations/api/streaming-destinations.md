---
keywords: Experience Platform；ホーム；人気の高いトピック；APIチュートリアル；ストリーミング送信先API;プラットフォーム
solution: Experience Platform
title: Adobe Experience PlatformのFlow Service APIを使用して、ストリーミング送信先に接続し、データをアクティブにします。
description: このドキュメントでは、Adobe Experience PlatformAPIを使用したストリーミング宛先の作成について説明します
topic: チュートリアル
type: チュートリアル
translation-type: tm+mt
source-git-commit: 32cb198bcf2c142b50c4b7a60282f0c923be06b1
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 54%

---


# Flow Service APIを使用して、ストリーミング送信先に接続し、データをアクティブにします

>[!NOTE]
>
>プラットフォームの[!DNL Amazon Kinesis]と[!DNL Azure Event Hubs]の宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

このチュートリアルでは、API呼び出しを使用してAdobe Experience Platformデータに接続し、ストリーミングクラウドストレージ先([AmazonKinesis](../catalog/cloud-storage/amazon-kinesis.md)または[Azureイベントハブ](../catalog/cloud-storage/azure-event-hubs.md))に接続し、新しく作成した宛先にデータフローを作成し、新しく作成した宛先にデータをアクティブ化する方法を説明します。

このチュートリアルでは、すべての例で[!DNL Amazon Kinesis]の宛先を使用しますが、手順は[!DNL Azure Event Hubs]と同じです。

![概要 — ストリーミングの宛先を作成し、セグメントをアクティブにする手順](../assets/api/streaming-destination/overview.png)

Platformのユーザーインターフェイスを使用して宛先に接続し、データをアクティブにする場合は、[宛先に接続](../ui/connect-destination.md)および[宛先にプロファイルとセグメントをアクティブにするチュートリアルを参照してください。](../ui/activate-destinations.md)

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [[!DNL Catalog Service]](../../catalog/home.md): [!DNL Catalog] は、Experience Platform内のデータの場所と系列のレコードシステムです。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、プラットフォーム内のストリーミング宛てのデータをアクティブにするために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、接続してセグメントをアクティブ化する宛先の種類に応じて、次の資格情報を準備しておく必要があります。

* [!DNL Amazon Kinesis]接続の場合：`accessKeyId`、`secretKey`、`region`または`connectionUrl`
* [!DNL Azure Event Hubs]接続の場合：`sasKeyName`、`sasKey`、`namespace`

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーおよびオプションヘッダーの値の収集 {#gather-values}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

### Swagger のドキュメント  {#swagger-docs}

このチュートリアルに含まれるすべての API 呼び出しについての参照ドキュメンは、Swagger のホームページにあります。Adobe I/O](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)の[Flow Service APIに関するドキュメントを参照してください。 このチュートリアルと Swagger のドキュメントページを並行して使用することをお勧めします。

## 使用可能なストリーミング先のリストを取得{#get-the-list-of-available-streaming-destinations}

![宛先の指定手順の概要 - 手順 1](../assets/api/streaming-destination/step1.png)

最初の手順として、データをアクティブにするストリーミング先を決定する必要があります。 最初に、接続してセグメントをアクティブ化できる、使用可能な宛先のリストを要求する呼び出しを実行します。`connectionSpecs` エンドポイントに次の GET リクエストを実行すると、使用可能な宛先のリストが返されます。

**API 形式**

```http
GET /connectionSpecs
```

**リクエスト**

```shell
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
--header 'accept: application/json' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```


**応答** 

リクエストが成功した場合、使用可能な宛先のリストと、その一意の ID（`id`）が返されます。使用する宛先の値を保存します。この値は、以降の手順で必要になります。例えば、セグメントを[!DNL Amazon Kinesis]または[!DNL Azure Event Hubs]に接続して配信する場合、応答で次のスニペットを探します。

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

## Experience Platform データへの接続 {#connect-to-your-experience-platform-data}

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
--header 'x-gw-ims-org-id: {IMS_ORG}' \
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


* `{CONNECTION_SPEC_ID}`：統合プロファイルサービスの接続仕様 ID（`8a9c3494-9708-43d7-ae3f-cda01e5030e1`）を使用します。

**応答** 

リクエストが成功した場合、ベース接続の一意の ID（`id`）が返されます。この値は、次の手順でソース接続を作成する際に必要になるため保存します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### Experience Platform データへの接続   {#connect-to-platform-data}

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
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

* `{BASE_CONNECTION_ID}`：前述の手順で取得した ID を使用します。
* `{CONNECTION_SPEC_ID}`：統合プロファイルサービスの接続仕様 ID（`8a9c3494-9708-43d7-ae3f-cda01e5030e1`）を使用します。

**応答** 

リクエストが成功した場合は、新しく作成した、統合プロファイルサービスへのソース接続を表す一意の ID（`id`）が返されます。識別子が返された場合、Experience Platform データに正常に接続できています。この値は、後の手順で必要になるため保存します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```


## ストリーミング宛先に接続{#connect-to-streaming-destination}

![宛先の指定手順の概要 - 手順 3](../assets/api/streaming-destination/step3.png)

この手順では、目的のストリーミング宛先への接続を設定します。 そのためには、以下に示す 2 つの手順を実行します。

1. まず、ベース接続を設定して、ストリーミング宛先へのアクセスを許可する呼び出しを実行する必要があります。
2. 次に、ベース接続 ID を使用して、ターゲット接続を作成する別の呼び出しを実行します。これで、書き出されたデータが送信されるストレージアカウント内の場所と、書き出されるデータの形式が指定されます。

### ストリーミング先へのアクセスを許可する

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!IMPORTANT]
>
>次の例には、先頭に`//`が付いたコードコメントが含まれています。 これらのコメントは、異なるストリーミング先で異なる値を使用する必要がある場所を強調表示します。 スニペットを使用する前に、コメントを削除してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
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
* `{AUTHENTICATION_CREDENTIALS}`:ストリーミング先の名前を入力します。 `Aws Kinesis authentication credentials` または `Azure EventHub authentication credentials`。
* `{ACCESS_ID}`: *[!DNL Amazon Kinesis] 接続用。* AmazonKinesisストレージの場所のアクセスID。
* `{SECRET_KEY}`: *[!DNL Amazon Kinesis] 接続用。* AmazonKinesisストレージの場所の秘密キー。
* `{REGION}`: *[!DNL Amazon Kinesis] 接続用。* プラットフォームがデータをストリーミングする [!DNL Amazon Kinesis] アカウント内の領域。
* `{SAS_KEY_NAME}`: *[!DNL Azure Event Hubs] 接続用。* SASキー名を入力します。[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)で、SASキーを使用した[!DNL Azure Event Hubs]への認証について説明します。
* `{SAS_KEY}`: *[!DNL Azure Event Hubs] 接続用。* SASキーを入力します。[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)で、SASキーを使用した[!DNL Azure Event Hubs]への認証について説明します。
* `{EVENT_HUB_NAMESPACE}`: *[!DNL Azure Event Hubs] 接続用。* プラットフォームがデータをストリーミングする [!DNL Azure Event Hubs] 名前空間を入力します。詳しくは、[!DNL Microsoft]ドキュメントの[イベントハブ名前空間の作成](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace)を参照してください。

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
>次の例には、先頭に`//`が付いたコードコメントが含まれています。 これらのコメントは、異なるストリーミング先で異なる値を使用する必要がある場所を強調表示します。 スニペットを使用する前に、コメントを削除してください。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
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
* `{NAME_OF_DATA_STREAM}`: *[!DNL Amazon Kinesis] 接続用。* ア [!DNL Amazon Kinesis] カウント内の既存のデータストリームの名前を指定します。プラットフォームは、このストリームにデータをエクスポートします。
* `{REGION}`: *[!DNL Amazon Kinesis] 接続用。* プラットフォームがデータをストリーミングする、AmazonKinesisアカウント内の地域。
* `{EVENT_HUB_NAME}`: *[!DNL Azure Event Hubs] 接続用。* プラットフォームがデータをストリーミングする [!DNL Azure Event Hub] 名前を入力します。詳しくは、[!DNL Microsoft]ドキュメントの[イベントハブの作成](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub)を参照してください。

**応答** 

正常に応答すると、新たに作成されたターゲット接続からストリーミング宛先への一意の識別子(`id`)が返されます。 この値は、後の手順で必要になるため保存します。

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
-H 'x-gw-ims-org-id: {IMS_ORG}' \
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

* `{FLOW_SPEC_ID}`:プロファイルベースの宛先のフロー仕様IDはで `71471eba-b620-49e4-90fd-23f1fa0174d8`す。この値は呼び出しで使用します。
* `{SOURCE_CONNECTION_ID}`：手順「[Experience Platform データへの接続](#connect-to-your-experience-platform-data)」で取得したソース接続 ID を使用します。
* `{TARGET_CONNECTION_ID}`:「ストリーミング宛先への [接続」の手順で取得したターゲット接続IDを使用します](#connect-to-streaming-destination)。

**応答** 

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）と `etag` が返されます。両方の値をメモしておきます。これらの値は、次の手順でセグメントをアクティブ化する際に使用します。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい宛先に対してデータをアクティブ化にする  {#activate-data}

![宛先の指定手順の概要 - 手順 5](../assets/api/streaming-destination/step5.png)

すべての接続とデータフローを作成したら、プロファイルデータをストリーミングプラットフォームに対してアクティブ化できます。 この手順では、宛先に送信するセグメントとプロファイル属性を選択し、スケジュールを設定して宛先にデータを送信します。

新しい宛先に対してセグメントをアクティブ化するには、次の例のような JSON パッチ操作を実行する必要があります。1 回の呼び出しで、複数のセグメントとプロファイル属性をアクティブ化できます。JSON パッチについて詳しくは、[RFC 仕様](https://tools.ietf.org/html/rfc6902)を参照してください。

**API 形式**

```http
PATCH /flows
```

**リクエスト**

```shell
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

* `{DATAFLOW_ID}`：前述の手順で取得したデータフローを使用します。
* `{ETAG}`：前述の手順で取得した ETag を使用します。
* `{SEGMENT_ID}`：この宛先に書き出すセグメント ID を指定します。アクティブ化するセグメントのセグメントIDを取得するには、**https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/**&#x200B;に移動し、左側のナビゲーションメニューで「**[!UICONTROL Segmentation Service API]**」を選択し、「**[!UICONTROL セグメント定義]**」で「`GET /segment/definitions`」操作を探します。
* `{PROFILE_ATTRIBUTE}`:例えば、 `personalEmail.address` または  `person.lastName`

**応答** 

202 OK レスポンスを探します。レスポンスの本文は返されません。リクエストが正しいことを検証する方法については、次の手順「データフローの検証」を参照してください。

## データフローの検証

![宛先の指定手順の概要 - 手順 6](../assets/api/streaming-destination/step6.png)

このチュートリアルの最後の手順では、セグメントとプロファイル属性が実際にデータフローに正しくマッピングされたことを検証する必要があります。

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
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--header 'x-sandbox-name: prod' \
--header 'If-Match: "{ETAG}"' 
```

* `{DATAFLOW_ID}`：前述の手順で作成したデータフローを使用します。
* `{ETAG}`：前述の手順で取得した Etag を使用します。

**応答** 

返されたレスポンスの `transformations` パラメーターに、前述の手順で送信したセグメントとプロファイル属性が含まれています。レスポンス内のサンプル `transformations` パラメーターは次のようになります。

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
> 手順[新しい宛先](#activate-data)に対するプロファイル属性とデータをアクティブ化のセグメントに加えて、[!DNL AWS Kinesis]と[!DNL Azure Event Hubs]に書き出されたデータには、IDマップに関する情報も含まれます。 これは、書き出されたプロファイルのID（例：[ECID](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html)、モバイルID、Google ID、電子メールアドレスなど）を表します。 以下の例を参照してください。

```json
{
  "person": {
    "email": "yourstruly@adobe.con"
  },
  "segmentMembership": {
    "ups": {
      "72ddd79b-6b0a-4e97-a8d2-112ccd81bd02": {
        "lastQualificationTime": "2020-03-03T21:24:39Z",
        "status": "exited"
      },
      "7841ba61-23c1-4bb3-a495-00d695fe1e93": {
        "lastQualificationTime": "2020-03-04T23:37:33Z",
        "status": "existing"
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

## Postmanコレクションを使用したストリーミング宛先への接続{#collections}

このチュートリアルで説明するストリーミングの宛先に接続する場合は、[[!DNL Postman]](https://www.postman.com/)を使用して、より簡単に行うことができます。

[!DNL Postman] は、API呼び出しを行い、定義済みの呼び出しと環境のライブラリを管理するのに使用できるツールです。

このチュートリアルでは、次の[!DNL Postman]コレクションが添付されています。

* [!DNL AWS Kinesis] [!DNL Postman] collection
* [!DNL Azure Event Hubs] [!DNL Postman] collection

[ここ](../assets/api/streaming-destination/DestinationPostmanCollection.zip)をクリックして、コレクションのアーカイブをダウンロードします。

各コレクションには、[!DNL AWS Kinesis]用と[!DNL Azure Event Hub]用に必要なリクエストと環境変数が含まれます。

### Postmanコレクションの使用方法

添付された[!DNL Postman]コレクションを使用して宛先に正常に接続するには、次の手順に従います。

* [!DNL Postman]をダウンロードしてインストールします。
* [添付されたコレクションを](../assets/api/streaming-destination/DestinationPostmanCollection.zip) ダウンロードして解凍します。
* 対応するフォルダーからPostmanにコレクションを読み込みます。
* この記事の手順に従って、環境変数を入力します。
* この記事の説明に基づいて、Postmanから[!DNL API]リクエストを実行します。

## 次の手順

このチュートリアルに従うと、プラットフォームを優先ストリーミング先の1つに接続し、それぞれの宛先へのデータフローを設定できます。 これで、送信データを、顧客分析や実行する他の任意のデータ操作の送信先で使用できるようになりました。 詳しくは、以下のページを参照してください。

* [Destinations overview](../home.md)
* [宛先カタログの概要](../catalog/overview.md)
