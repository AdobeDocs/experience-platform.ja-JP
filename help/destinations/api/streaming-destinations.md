---
keywords: エクスペリエンス Platform、home、人気のある話題。API チュートリアル、その他の方法ストリーミング送信先 API。プラットフォーム
solution: Experience Platform
title: Adobe エクスペリエンスプラットフォームのフローサービス API を使用したストリーミング宛先への接続とデータのアクティブ化
description: このドキュメントでは、Adobe エクスペリエンスプラットフォーム API を使用したストリーミング宛先の作成について説明します。
topic-legacy: tutorial
type: Tutorial
exl-id: 3e8d2745-8b83-4332-9179-a84d8c0b4400
source-git-commit: 2b1cde9fc913be4d3bea71e7d56e0e5fe265a6be
workflow-type: tm+mt
source-wordcount: '2021'
ht-degree: 53%

---

# ストリーミングサービス API を使用したストリーミング宛先への接続とデータの有効化

>[!NOTE]
>
>[!DNL Amazon Kinesis] [!DNL Azure Event Hubs] プラットフォーム内のおよび宛先には、現在ベータ版が含まれています。ドキュメントと機能は変更される場合があります。

このチュートリアルでは、API 呼び出しを使用して Adobe エクスペリエンスプラットフォームデータに接続する方法、ストリーミングターゲット ( [ Amazon Kinesis または Azure Event hub) に対する接続を作成する方法、 ](../catalog/cloud-storage/amazon-kinesis.md) [ ](../catalog/cloud-storage/azure-event-hubs.md) 新しく作成された送信先にデータフローを作成する方法、新しく作成された出力先にデータをアクティブにする方法について説明します。

このチュートリアルでは、 [!DNL Amazon Kinesis] すべての例において移動先を使用しますが、その手順は同じ [!DNL Azure Event Hubs] です。

![概要: ストリーミング送信先を作成し、セグメントをアクティブ化するための手順](../assets/api/streaming-destination/overview.png)

プラットフォームのユーザーインターフェイスを使用して、宛先に接続し、データを有効にする場合は、「 [ 宛先に接続 ](../ui/connect-destination.md) し、セグメントの [ 書き出し先のストリーミングへの配信をアクティブにする」を参照してください ](../ui/activate-segment-streaming-destinations.md) 。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md): 標準化されたフレームワークで、経験のあるプラットフォームによってカスタマーエクスペリエンスデータが整理されます。
* [[!DNL Catalog Service]](../../catalog/home.md): [!DNL Catalog] エクスペリエンスプラットフォームにおいて、データの場所と系列の記録のシステムを指定します。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の各セクションでは、プラットフォームのストリーミングターゲットにデータをアクティブにするために必要な追加情報を記載しています。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、接続してセグメントをアクティブ化する宛先の種類に応じて、次の資格情報を準備しておく必要があります。

* 接続の場合 [!DNL Amazon Kinesis] : `accessKeyId` 、 `secretKey` 、 `region` または `connectionUrl`
* 接続の場合 [!DNL Azure Event Hubs] : `sasKeyName` 、 `sasKey` 、、 `namespace`

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーおよびオプションヘッダーの値の収集 {#gather-values}

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

### Swagger のドキュメント {#swagger-docs}

このチュートリアルに含まれるすべての API 呼び出しについての参照ドキュメンは、Swagger のホームページにあります。[Adobe i/o について詳しくは、Flow SERVICE API のマニュアルを参照してください ](https://www.adobe.io/experience-platform-apis/references/flow-service/) 。このチュートリアルと Swagger のドキュメントページを並行して使用することをお勧めします。

## 使用可能なストリーミング出力先のリストを取得します。 {#get-the-list-of-available-streaming-destinations}

![宛先の指定手順の概要 - 手順 1](../assets/api/streaming-destination/step1.png)

まず、どのストリーミング送信先にデータを有効にするかを決定する必要があります。 最初に、接続してセグメントをアクティブ化できる、使用可能な宛先のリストを要求する呼び出しを実行します。`connectionSpecs` エンドポイントに次の GET リクエストを実行すると、使用可能な宛先のリストが返されます。

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

リクエストが成功した場合、使用可能な宛先のリストと、その一意の ID（`id`）が返されます。使用する宛先の値を保存します。この値は、以降の手順で必要になります。例えば、セグメントを接続してまたはに送信する場合 [!DNL Amazon Kinesis] は [!DNL Azure Event Hubs] 、応答で次のスニペットを探します。

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


* `{CONNECTION_SPEC_ID}`: Profile Service 用の接続スペック ID を使用して `8a9c3494-9708-43d7-ae3f-cda01e5030e1` ください。

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
--header 'x-gw-ims-org-id: {IMS_ORG}' \
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
            "params" : {}
}'
```

* `{BASE_CONNECTION_ID}`：前述の手順で取得した ID を使用します。
* `{CONNECTION_SPEC_ID}`: Profile Service 用の接続スペック ID を使用して `8a9c3494-9708-43d7-ae3f-cda01e5030e1` ください。

**応答**

応答が成功した場合は、 `id` プロファイルサービスへの新しく作成されたソース接続の一意の識別子 () が返されます。 識別子が返された場合、Experience Platform データに正常に接続できています。この値は、後の手順で必要になるため保存します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```


## ストリーミング出力先への接続 {#connect-to-streaming-destination}

![宛先の指定手順の概要 - 手順 3](../assets/api/streaming-destination/step3.png)

この手順では、目的のストリーミング宛先への接続を設定します。 そのためには、以下に示す 2 つの手順を実行します。

1. 最初に、基本的な接続を設定して、ストリーミング宛先へのアクセスを許可するには、呼び出しを実行する必要があります。
2. 次に、ベース接続 ID を使用して、ターゲット接続を作成する別の呼び出しを実行します。これで、書き出されたデータが送信されるストレージアカウント内の場所と、書き出されるデータの形式が指定されます。

### ストリーミング宛先へのアクセスを許可します。

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!IMPORTANT]
>
>次の例には、というプレフィックスの付いたコードコメントが含まれてい `//` ます。 これらのコメントは、異なるストリーミング送信先に異なる値を使用する必要があることが強調されています。 スニペットを使用する前に、コメントを削除してください。

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
* `{AUTHENTICATION_CREDENTIALS}`: ストリーミング宛先の名前を入力 `Aws Kinesis authentication credentials` `Azure EventHub authentication credentials` します。
* `{ACCESS_ID}`: *接続の場合 [!DNL Amazon Kinesis] 。* Amazon Kinesis 格納場所のアクセス ID。
* `{SECRET_KEY}`: *接続の場合 [!DNL Amazon Kinesis] 。* Amazon Kinesis 格納場所の秘密キー
* `{REGION}`: *接続の場合 [!DNL Amazon Kinesis] 。* データをストリーミングするプラットフォームによって使用されるアカウント内の領域 [!DNL Amazon Kinesis] です。
* `{SAS_KEY_NAME}`: *接続の場合 [!DNL Azure Event Hubs] 。* SAS キーの名前を入力します。 Microsoft のマニュアルに記載されている SAS キーを使用した認証方法について説明 [!DNL Azure Event Hubs] [ ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) します。
* `{SAS_KEY}`: *接続の場合 [!DNL Azure Event Hubs] 。* SAS キーを入力します。 Microsoft のマニュアルに記載されている SAS キーを使用した認証方法について説明 [!DNL Azure Event Hubs] [ ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) します。
* `{EVENT_HUB_NAMESPACE}`: *接続の場合 [!DNL Azure Event Hubs] 。*[!DNL Azure Event Hubs]プラットフォームによってデータがストリーミングされる名前空間に入力します。詳しくは、マニュアルの「 [ Event hub 名前空間の作成」を参照してください ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) [!DNL Microsoft] 。

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
>次の例には、というプレフィックスの付いたコードコメントが含まれてい `//` ます。 これらのコメントは、異なるストリーミング送信先に異なる値を使用する必要があることが強調されています。 スニペットを使用する前に、コメントを削除してください。

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
* `{NAME_OF_DATA_STREAM}`: *接続の場合 [!DNL Amazon Kinesis] 。* アカウントに既存のデータストリームの名前を指定 [!DNL Amazon Kinesis] します。 プラットフォームによって、データがこのストリームに書き出されます。
* `{REGION}`: *接続の場合 [!DNL Amazon Kinesis] 。* プラットフォームがデータをストリーミングする Amazon Kinesis アカウント内の領域です。
* `{EVENT_HUB_NAME}`: *接続の場合 [!DNL Azure Event Hubs] 。*[!DNL Azure Event Hub]プラットフォームによってデータがストリーミングされる名前を入力します。詳しくは、マニュアルの「イベントハブの作成」を参照してください [ ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) [!DNL Microsoft] 。

**応答**

応答が成功した場合は、 `id` 新しく作成されたストリーミング宛先へのターゲット接続の一意の識別子 () が返されます。 この値は、後の手順で必要になるため保存します。

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

* `{FLOW_SPEC_ID}`: プロファイルに基づいた宛先のフロー仕様 ID が `71471eba-b620-49e4-90fd-23f1fa0174d8` です。 この値は呼び出しで使用します。
* `{SOURCE_CONNECTION_ID}`：手順「[Experience Platform データへの接続](#connect-to-your-experience-platform-data)」で取得したソース接続 ID を使用します。
* `{TARGET_CONNECTION_ID}`: ステップで取得したターゲット接続 ID を使用して、 [ ストリーミング宛先に接続 ](#connect-to-streaming-destination) します。

**応答** 

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）と `etag` が返されます。両方の値をメモしておきます。これらの値は、次の手順でセグメントをアクティブ化する際に使用します。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい宛先に対してデータをアクティブ化にする {#activate-data}

![宛先の指定手順の概要 - 手順 5](../assets/api/streaming-destination/step5.png)

すべての接続とデータフローが作成されたので、プロファイルデータをストリーミングプラットフォームにアクティブ化できるようになりました。 この手順では、宛先に送信するセグメントとプロファイル属性を選択し、スケジュールを設定して宛先にデータを送信します。

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
* `{SEGMENT_ID}`：この宛先に書き出すセグメント ID を指定します。アクティブ化するセグメントのセグメント Id を取得するには、https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/にアクセスし、 **** 左側の **[!UICONTROL ナビゲーションメニューで「セグメンテーションサービス api」を選択]** して、 `GET /segment/definitions` セグメント定義で操作を探し **** ます。
* `{PROFILE_ATTRIBUTE}`: 例えば、 `personalEmail.address` または `person.lastName`

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

**書き出したデータ**

>[!IMPORTANT]
>
> では、プロファイル属性と、ステップのセグメントによって、 [ データが新しい宛先にアクティブ化 ](#activate-data) されます。また、に書き出されたデータには、 [!DNL AWS Kinesis] [!DNL Azure Event Hubs] id マップについての情報も含まれています。 これは、書き出されたプロファイルの id を表します (例えば [ ](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html?lang=ja) 、d、mobile Id、Google id、電子メールアドレスなど)。 以下の例を参照してください。

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

## Postman コレクションを使用したストリーミング宛先への接続  {#collections}

より効率的に使用できるように、このチュートリアルで説明されているストリーミング出力先に接続するには、を使用 [[!DNL Postman] ](https://www.postman.com/) します。

[!DNL Postman] は、API を呼び出して、事前に定義された呼び出しと環境のライブラリを管理するために使用できるツールです。

このチュートリアルでは、次の [!DNL Postman] コレクションがアタッチされています。

* [!DNL AWS Kinesis] [!DNL Postman] collection
* [!DNL Azure Event Hubs][!DNL Postman]コレクション

[ ](../assets/api/streaming-destination/DestinationPostmanCollection.zip) コレクションアーカイブをダウンロードするには、ここをクリックします。

各コレクションには、必要な要求、環境変数、およびについての説明が含まれてい [!DNL AWS Kinesis] [!DNL Azure Event Hub] ます。

### Postman コレクションの使用方法

添付されているコレクションを使用して宛先に正常に接続するには [!DNL Postman] 、次の手順を実行します。

* ダウンロードしてインストールし [!DNL Postman] ます。
* [](../assets/api/streaming-destination/DestinationPostmanCollection.zip)添付されているコレクションをダウンロードして解凍します。
* 対応するフォルダーのコレクションを Postman に読み込みます。
* この記事に記載されている手順に従って、環境変数に情報を入力します。
* [!DNL API]この記事に記載された手順に従って、Postman からの要求を実行します。

## 次の手順

このチュートリアルに従うことで、プラットフォームを適切なストリーミング出力先に接続し、データフローをそれぞれの目的に合わせて設定することができました。 出力データは、カスタマー分析の対象として、またはその他の実行するデータ操作に使用することができるようになりました。 詳しくは、以下のページを参照してください。

* [Destinations overview](../home.md)
* [宛先カタログの概要](../catalog/overview.md)
