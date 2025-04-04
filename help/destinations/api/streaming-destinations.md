---
keywords: Experience Platform；ホーム；人気のトピック；API チュートリアル；ストリーミング宛先 API;Experience Platform
solution: Experience Platform
title: Adobe Experience Platformの Flow Service API を使用したストリーミング宛先への接続とデータのアクティブ化
description: このドキュメントでは、Adobe Experience Platform API を使用したストリーミング宛先の作成について説明します
type: Tutorial
exl-id: 3e8d2745-8b83-4332-9179-a84d8c0b4400
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2219'
ht-degree: 41%

---

# Flow Service API でストリーミング宛先に接続してデータを有効化する

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。
>
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。
>
>[アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

このチュートリアルでは、API 呼び出しを使用してAdobe Experience Platform データに接続する方法、ストリーミングクラウドストレージの宛先（[Amazon Kinesis](../catalog/cloud-storage/amazon-kinesis.md) または [Azure Event Hubs](../catalog/cloud-storage/azure-event-hubs.md)）への接続を作成する方法、新しく作成した宛先にデータフローを作成する方法、新しく作成した宛先にデータをアクティブ化する方法について説明します。

このチュートリアルでは、すべての例で [!DNL Amazon Kinesis] の宛先を使用しますが、手順は [!DNL Azure Event Hubs] で同じです。

![ 概要 – ストリーミング宛先の作成およびオーディエンスのアクティブ化の手順 ](../assets/api/streaming-destination/overview.png)

Experience Platformのユーザーインターフェイスを使用して宛先に接続し、データを有効化する場合は、[ 宛先の接続 ](../ui/connect-destination.md) および [ ストリーミングオーディエンス書き出しの宛先に対するオーディエンスデータの有効化 ](../ui/activate-segment-streaming-destinations.md) に関するチュートリアルを参照してください。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
* [[!DNL Catalog Service]](../../catalog/home.md):[!DNL Catalog] は、Experience Platform内のデータの場所と系列の記録システムです。
* [ サンドボックス ](../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、Experience Platformでストリーミング宛先に対してデータをアクティブ化するために必要な追加情報を示します。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、オーディエンスを接続してアクティブ化する宛先のタイプに応じて、次の資格情報を準備しておく必要があります。

* [!DNL Amazon Kinesis] 接続：`accessKeyId`、`secretKey`、`region` または `connectionUrl`
* [!DNL Azure Event Hubs] 接続：`sasKeyName`、`sasKey`、`namespace`

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーおよびオプションヘッダーの値の収集 {#gather-values}

Experience Platform API を呼び出すには、まず[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のリソースは、特定の仮想サンドボックスに分離することができます。Experience Platform API へのリクエストでは、操作を実行するサンドボックスの名前と ID を指定できます。 次に、オプションのパラメーターを示します。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

### Swagger のドキュメント {#swagger-docs}

このチュートリアルに含まれるすべての API 呼び出しについての参照ドキュメンは、Swagger のホームページにあります。詳しくは、Adobe I/Oにある [Flow Service API ドキュメント ](https://www.adobe.io/experience-platform-apis/references/flow-service/) を参照してください。 このチュートリアルと Swagger のドキュメントページを並行して使用することをお勧めします。

## 使用可能なストリーミング宛先のリストの取得 {#get-the-list-of-available-streaming-destinations}

![宛先手順の概要 - 手順 1](../assets/api/streaming-destination/step1.png)

最初の手順として、データを有効化するストリーミング宛先を決定する必要があります。 まず、呼び出しを実行して、オーディエンスを接続およびアクティブ化できる使用可能な宛先のリストをリクエストします。 `connectionSpecs` エンドポイントに次の GET リクエストを実行すると、使用可能な宛先のリストが返されます。

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

リクエストが成功した場合、使用可能な宛先のリストと、その一意の ID（`id`）が返されます。使用する宛先の値を保存します。この値は、以降の手順で必要になります。例えば、オーディエンスを接続して [!DNL Amazon Kinesis] または [!DNL Azure Event Hubs] に配信する場合、応答で次のスニペットを探します。

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


* `{CONNECTION_SPEC_ID}`: プロファイルサービスの接続仕様 ID （`8a9c3494-9708-43d7-ae3f-cda01e5030e1`）を使用します。

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
* `{CONNECTION_SPEC_ID}`: プロファイルサービスの接続仕様 ID （`8a9c3494-9708-43d7-ae3f-cda01e5030e1`）を使用します。

**応答**

リクエストが成功した場合は、新しく作成したプロファイルサービスへのソース接続を表す一意の ID （`id`）が返されます。 識別子が返された場合、Experience Platform データに正常に接続できています。この値は、後の手順で必要になるため保存します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```


## ストリーミング宛先への接続 {#connect-to-streaming-destination}

![宛先手順の概要 - 手順 3](../assets/api/streaming-destination/step3.png)

この手順では、目的のストリーミング宛先への接続を設定します。 そのためには、以下に示す 2 つの手順を実行します。

1. まず、ベース接続を設定して、ストリーミング宛先へのアクセスを認証する呼び出しを実行する必要があります。
2. 次に、ベース接続 ID を使用して、ターゲット接続を作成する別の呼び出しを実行します。これで、書き出されたデータが送信されるストレージアカウント内の場所と、書き出されるデータの形式が指定されます。

### ストリーミング宛先へのアクセスの認証

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!IMPORTANT]
>
>次の例には、先頭に `//` が付いたコードコメントが含まれています。 これらのコメントは、様々なストリーミング宛先に対して異なる値を使用する必要がある場所をハイライト表示します。 スニペットを使用する前に、コメントを削除してください。

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
* `{AUTHENTICATION_CREDENTIALS}`：ストリーミング宛先の名前（`Aws Kinesis authentication credentials` または `Azure EventHub authentication credentials`）を入力します。
* `{ACCESS_ID}`: *[!DNL Amazon Kinesis] 接続の場合。Amazon Kinesis ストレージの場所のアクセス ID を* します。
* `{SECRET_KEY}`: *[!DNL Amazon Kinesis] 接続の場合。Amazon Kinesis ストレージの場所の秘密鍵を* します。
* `{REGION}`: *[!DNL Amazon Kinesis] 接続の場合。* Experience Platformがデータをストリーミングする、[!DNL Amazon Kinesis] アカウントのリージョン。
* `{SAS_KEY_NAME}`: *[!DNL Azure Event Hubs] 接続の場合。* SAS キー名を入力します。 SAS キーを使用した [!DNL Azure Event Hubs] への認証については、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。
* `{SAS_KEY}`: *[!DNL Azure Event Hubs] 接続の場合。* SAS キーを入力します。 SAS キーを使用した [!DNL Azure Event Hubs] への認証については、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。
* `{EVENT_HUB_NAMESPACE}`: *[!DNL Azure Event Hubs] 接続の場合。* Experience Platformがデータをストリーミングする [!DNL Azure Event Hubs] 名前空間を入力します。 詳しくは、[!DNL Microsoft] ドキュメントの [Event Hubs 名前空間の作成 ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) を参照してください。

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
>次の例には、先頭に `//` が付いたコードコメントが含まれています。 これらのコメントは、様々なストリーミング宛先に対して異なる値を使用する必要がある場所をハイライト表示します。 スニペットを使用する前に、コメントを削除してください。

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
* `{NAME_OF_DATA_STREAM}`: *[!DNL Amazon Kinesis] 接続の場合。* [!DNL Amazon Kinesis] アカウントの既存のデータストリームの名前を指定します。 Experience Platformはこのストリームにデータを書き出します。
* `{REGION}`: *[!DNL Amazon Kinesis] 接続の場合。* Experience Platformがデータをストリーミングする、Amazon Kinesis アカウントのリージョン。
* `{EVENT_HUB_NAME}`: *[!DNL Azure Event Hubs] 接続の場合。* Experience Platformがデータをストリーミングする [!DNL Azure Event Hub] 名を入力します。 詳しくは、[!DNL Microsoft] ドキュメントの [ イベントハブの作成 ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) を参照してください。

**応答**

リクエストが成功した場合は、新しく作成したストリーミング宛先へのターゲット接続を表す一意の ID （`id`）が返されます。 この値は、後の手順で必要になるため保存します。

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

* `{FLOW_SPEC_ID}`：プロファイルベースの宛先のフロー仕様 ID は `71471eba-b620-49e4-90fd-23f1fa0174d8` です。 この値を呼び出しで使用します。
* `{SOURCE_CONNECTION_ID}`：手順「[Experience Platform データへの接続](#connect-to-your-experience-platform-data)」で取得したソース接続 ID を使用します。
* `{TARGET_CONNECTION_ID}`：手順 [ ストリーミング宛先への接続 ](#connect-to-streaming-destination) で取得したターゲット接続 ID を使用します。

**応答** 

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）と `etag` が返されます。両方の値をメモしておきます。オーディエンスをアクティブ化するには、次の手順で行います。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい宛先に対してデータをアクティブ化にする {#activate-data}

![宛先の指定手順の概要 - 手順 5](../assets/api/streaming-destination/step5.png)

これで、すべての接続とデータフローを作成したので、プロファイルデータをストリーミングプラットフォームに対してアクティブ化できます。 この手順では、宛先に送信するオーディエンスとプロファイル属性を選択し、スケジュールを設定して宛先にデータを送信できます。

新しい宛先に対してオーディエンスをアクティブ化するには、以下の例に示すような JSON PATCH操作を実行する必要があります。 1 回の呼び出しで、複数のオーディエンスとプロファイル属性をアクティブ化できます。 JSON パッチについて詳しくは、[RFC 仕様](https://tools.ietf.org/html/rfc6902)を参照してください。

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
| `{ETAG}` | 前の手順 [ データフローの作成 ](#create-dataflow) の応答から `{ETAG}` を取得します。 前の手順の応答形式には、引用符がエスケープされています。 リクエストのヘッダーには、エスケープされていない値を使用する必要があります。 以下の例を参照してください。<br> <ul><li>応答の例：`"etag":""7400453a-0000-1a00-0000-62b1c7a90000""`</li><li>リクエストで使用する値：`"etag": "7400453a-0000-1a00-0000-62b1c7a90000"`</li></ul> <br> etag 値は、データフローが正常に更新されるたびに更新されます。 |
| `{SEGMENT_ID}` | この宛先に書き出すオーディエンス ID を指定します。 アクティブ化するオーディエンスのオーディエンス ID を取得するには、Experience Platform API リファレンスの [ オーディエンス定義の取得 ](https://www.adobe.io/experience-platform-apis/references/segmentation/#operation/retrieveSegmentDefinitionById) を参照してください。 |
| `{PROFILE_ATTRIBUTE}` | 例：`"person.lastName"` |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。データフローにオーディエンスを追加するには、`add` 操作を使用します。 |
| `path` | 更新するフローの部分を定義します。オーディエンスをデータフローに追加するときは、例で指定したパスを使用します。 |
| `value` | パラメーターの更新に使用する新しい値。 |
| `id` | 宛先データフローに追加するオーディエンスの ID を指定します。 |
| `name` | *オプション*。宛先データフローに追加するオーディエンスの名前を指定します。 このフィールドは必須ではなく、名前を指定しなくてもオーディエンスを宛先データフローに正常に追加できます。 |

**応答** 

202 OK レスポンスを探します。レスポンスの本文は返されません。リクエストが正しいことを検証する方法については、次の手順「データフローの検証」を参照してください。

## データフローの検証

![宛先の指定手順の概要 - 手順 6](../assets/api/streaming-destination/step6.png)

このチュートリアルの最後の手順として、オーディエンスとプロファイル属性が正しくデータフローにマッピングされたことを検証する必要があります。

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

返される応答には、前の手順で送信したオーディエンスとプロファイル属性が `transformations` パラメーターに含まれている必要があります。 レスポンス内のサンプル `transformations` パラメーターは次のようになります。

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

**エクスポートされたデータ**

>[!IMPORTANT]
>
> [ 新しい宛先へのデータのアクティブ化 ](#activate-data) の手順のプロファイル属性とオーディエンスに加えて、[!DNL AWS Kinesis] および [!DNL Azure Event Hubs] の書き出されたデータには、ID マップに関する情報も含まれています。 書き出されたプロファイルの ID を表します（例：[ECID](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html)、モバイル ID、Google ID、メールアドレスなど）。 以下の例を参照してください。

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

## [!DNL Postman] コレクションを使用したストリーミング宛先への接続  {#collections}

このチュートリアルで説明しているストリーミング宛先に、より効率的に接続するには、[[!DNL Postman]](https://www.postman.com/) を使用できます。

[!DNL Postman] は、API 呼び出しを行い、事前定義済みの呼び出しと環境のライブラリを管理するために使用できるツールです。

この特定のチュートリアルでは、次の [!DNL Postman] コレクションが添付されています。

* [!DNL AWS Kinesis] [!DNL Postman] コレクション
* [!DNL Azure Event Hubs] [!DNL Postman] コレクション

コレクションアーカイブをダウンロードするには、[ ここ ](../assets/api/streaming-destination/DestinationPostmanCollection.zip) をクリックします。

各コレクションには、必要なリクエストと環境変数（[!DNL AWS Kinesis] と [!DNL Azure Event Hub]）がそれぞれ含まれます。

### [!DNL Postman] コレクションの使用方法 {#how-to-use-postman-collections}

添付された [!DNL Postman] コレクションを使用して宛先に正常に接続するには、次の手順に従います。

* [!DNL Postman] のダウンロードとインストール
* [ ダウンロード ](../assets/api/streaming-destination/DestinationPostmanCollection.zip) し、添付されているコレクションを解凍します。
* 対応するフォルダーから [!DNL Postman] にコレクションを読み込む。
* この記事の手順に従って、環境変数を入力します。
* この記事の説明に従って、[!DNL Postman] から [!DNL API] リクエストを実行します。

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience Platform API の一般的なエラーメッセージの原則に従っています。 エラー応答の解釈について詳しくは、Experience Platform トラブルシューティングガイドの [API ステータスコード ](/help/landing/troubleshooting.md#api-status-codes) および [ リクエストヘッダーエラー ](/help/landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、Experience Platformを優先ストリーミング宛先の 1 つに正常に接続し、それぞれの宛先へのデータフローを設定しました。 顧客の分析や必要に応じてその他のデータ操作のために、送信データを宛先で使用できるようになりました。 詳しくは、以下のページを参照してください。

* [宛先の概要](../home.md)
* [宛先カタログの概要](../catalog/overview.md)
