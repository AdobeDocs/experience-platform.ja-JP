---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 電子メールマーケティングの宛先の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 7ee83b5bf14ec802801cfbc17141c02ceeaccd82
workflow-type: tm+mt
source-wordcount: '1660'
ht-degree: 1%

---


# 電子メールマーケティングの宛先を作成し、アドビのリアルタイム顧客データプラットフォームでデータをアクティブにする

このチュートリアルでは、API呼び出しを使用してAdobe Experience Platformデータに接続する方法、 [電子メールマーケティングの宛先を作成する方法](../../rtcdp/destinations/email-marketing-destinations.md)、新しく作成した宛先へのデータフローを作成する方法、新しく作成した宛先へのデータをアクティブ化する方法を説明します。

このチュートリアルでは、Adobe Campaignのリンク先をすべての例で使用しますが、手順はすべての電子メールマーケティングのリンク先で同じです。

![概要 — 宛先を作成し、セグメントをアクティブ化する手順](../images/destinations/flow-api-destinations-steps-overview.png)

アドビのReal-time CDPのユーザーインターフェイスを使用して宛先を接続し、データをアクティブにする場合は、「宛先の [接続](../../rtcdp/destinations/connect-destination.md) 」および「宛先へのプロファイルとセグメントのアク [ティブ化](../../rtcdp/destinations/activate-destinations.md) 」のチュートリアルを参照してください。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
* [カタログサービス](../../catalog/home.md): カタログは、エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。
* [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Adobe Real-time CDPの電子メールマーケティング先に対してデータをアクティブ化するために知っておく必要がある追加情報について、以下の節で説明します。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、セグメントを接続およびアクティブ化する宛先のタイプに応じて、次の資格情報を準備する必要があります。

* 電子メールマーケティングプラットフォームへのAmazon S3接続の場合： `accessId`, `secretKey`
* 電子メールマーケティングプラットフォームへのSFTP接続の場合： `domain`、、 `port`、 `username`ま `password` たは `ssh key` （FTPの場所への接続方法に応じて異なります）

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必須ヘッダーと任意選択ヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのリソースは、特定の仮想サンドボックスに分離できます。 プラットフォームAPIへのリクエストでは、操作を実行するサンドボックスの名前とIDを指定できます。 これらはオプションのパラメーターです。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!N注意]
>エクスペリエンスプラットフォームのサンドボックスについて詳しくは、 [サンドボックスの概要に関するドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

<!--

### Definitions

Before starting this tutorial, familiarize yourself with the following terms which we'll use throughout the tutorial:

**Flow**: 

**Base Connection**: 

**Target Connection**: 

**Source Connection**: 

-->

### Swaggerドキュメント

Swaggerのこのチュートリアルでは、すべてのAPI呼び出しに関する付属のリファレンスドキュメントを参照できます。 https://platform.adobe.io/data/foundation/flowservice/swagger#/を参照してください。 このチュートリアルとSwaggerのドキュメントページを並行して使用することをお勧めします。

## 使用可能な宛先のリストの取得 {#get-the-list-of-available-destinations}

![宛先手順の概要手順1](../images/destinations/flow-api-destinations-step1.png)

最初の手順として、データをアクティブ化する電子メールマーケティングの宛先を決定する必要があります。 最初に、接続してセグメントをアクティブにできる、使用可能な宛先のリストをリクエストする呼び出しを実行します。 使用可能な宛先のリストを返すには、次のGETリクエストを `connectionSpecs` エンドポイントに実行します。

**API形式**

```http
GET /connectionSpecs
```

**リクエスト**

<!--

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-sandbox-id: {SANDBOX_ID}' \    
    -H 'Content-Type: application/json' \
```

-->

```
curl --location --request GET 'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
--header 'accept: application/json' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}'
```


**応答**

成功した応答には、使用可能な宛先とその一意の識別子(`id`)のリストが含まれます。 使用する宛先の値を保存します。この値は、以降の手順で必要になります。 例えば、セグメントをAdobe Campaignに接続して配信する場合は、応答内で次のスニペットを探します。

```json
{
    "id": "0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
  "name": "Adobe Campaign",
  ...
  ...
}
```

## エクスペリエンスプラットフォームデータに接続する {#connect-to-your-experience-platform-data}

![宛先手順の概要手順2](../images/destinations/flow-api-destinations-step2.png)

次に、Experience Platformデータに接続し、プロファイルデータを書き出して、目的のデータを書き出し先でアクティブ化できるようにする必要があります。 これは、次に説明する2つのサブステップで構成されます。

1. まず、ベース接続を設定して、エクスペリエンスプラットフォームでのデータへのアクセスを許可するための呼び出しを実行する必要があります。
2. 次に、ベース接続IDを使用して、別の呼び出しを行い、ソース接続を作成して、エクスペリエンスプラットフォームデータとの接続を確立します。


### エクスペリエンスプラットフォームでのデータへのアクセスを許可する

**API形式**

```http
POST /connections
```

**リクエスト**

<!--

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-sandbox-id: {SANDBOX_ID}' \ 
    -H 'Content-Type: application/json' \
    -d  '{
            
            "name": "Base connection to Experience Platform",
            "description": "This call establishes the connection to Experience Platform data",
            "connectionSpec": {
                "id": "{CONNECTION_SPEC}",
                "version": "1.0"
            }
           }'
```

-->

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

<!--

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-id: {SANDBOX_ID}' \ 
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
  "name": "Connecting to Unified Profile Service",
  "description": "Optional",
  "baseConnectionId": "{BASE_CONNECTION_ID}",
  "connectionSpec": {
    "id": "{CONNECTION_SPEC}",
    "version": "1.0"
  },
  "data": {
    "format": "CSV",
    "schema": null
  }
  }
```

-->

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
                "format": "CSV",
                "schema": null
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


## 電子メールのマーケティング先に接続 {#connect-to-email-marketing-destination}

![宛先手順の概要手順3](../images/destinations/flow-api-destinations-step3.png)

この手順では、目的の電子メールマーケティングの宛先への接続を設定します。 これは、次に説明する2つのサブステップで構成されます。

1. まず、ベースサービスプロバイダーを設定して、電子メール接続へのアクセスを許可する呼び出しを実行する必要があります。
2. 次に、ベース接続IDを使用して別の呼び出しを行い、ターゲット接続を作成します。この呼び出しでは、エクスポートされたデータが配信されるストレージアカウント内の場所と、エクスポートされるデータの形式を指定します。

### 電子メールマーケティングの宛先へのアクセスを許可する

**API形式**

```http
POST /connections
```

**リクエスト**

<!--

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-sandbox-id: {SANDBOX_ID}' \ 
    -H 'Content-Type: application/json' \
    -d  '{
            
            "name": "S3 Connection for Adobe Campaign",
            "description": "ACME company holiday campaign",
            "connectionSpec": {
                "id": "{CONNECTION_SPEC}",
                "version": "1.0"
            },
            "auth": {
                "specName": "{S3 or SFTP}",
                "params": {
                    "accessId": "{ACCESS_ID}",
                    "secretKey": "{SECRET_KEY}"
                }
            }
           }'
```

-->

```
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "S3 Connection for Adobe Campaign",
    "description": "your company's holiday campaign",
    "connectionSpec": {
        "id": "{_CONNECTION_SPEC_ID}",
        "version": "1.0"
    },
    "auth": {
        "specName": "{S3 or SFTP}",
        "params": {
            "accessId": "{ACCESS_ID}",
            "secretKey": "{SECRET_KEY}"
        }
    }
}'
```

* `{CONNECTION_SPEC_ID}`: 使用可能な宛先のリストの [取得で取得した接続仕様IDを使用します](#get-the-list-of-available-destinations)。
* `{S3 or SFTP}`: この宛先に対して必要な接続タイプを入力します。 リンク [先カタログで](../../rtcdp/destinations/destinations-catalog.md)、目的のリンク先までスクロールして、S3またはSFTPの接続タイプがサポートされているかどうかを確認します。
* `{ACCESS_ID}`: Amazon S3ストレージの場所のアクセスID。
* `{SECRET_KEY}`: Amazon S3ストレージの場所の秘密キー。

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

<!--

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/targetConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \    
    -H 'x-sandbox-id: {SANDBOX_ID}' \ 
    -H 'Content-Type: application/json' \
    -d  '{
   "baseConnectionId": "{BASE_CONNECTION_ID}",
   "name": "TargetConnection for Adobe Campaign",
   "data": {
       "format": "CSV",
       "schema": {
           "id": "1.0",
           "version": "1.0"
       },
    "connectionSpec": {
    "id": "{CONNECTION_SPEC_ID}",
    "version": "1.0"
   },
   "params": {
       "mode": "S3",
       "bucketName": "{BUCKETNAME}",
       "path": "{FILEPATH}"
    }
    }
```

-->

```
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Adobe Campaign",
    "description": "Connection to Adobe Campaign",
    "baseConnection": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "{CONNECTION_SPEC_ID}",
        "version": "1.0"
    },
    "data": {
        "format": "json",
        "schema": {
            "id": "1.0",
            "version": "1.0"
        }
    },
    "params": {
        "mode": "S3",
        "bucketName": "{BUCKETNAME}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
}'
```

* `{BASE_CONNECTION_ID}`: 上記の手順で取得したベース接続IDを使用します。
* `{CONNECTION_SPEC_ID}`: 使用可能な宛先のリストを [取得する手順で取得した接続仕様を使用します](#get-the-list-of-available-destinations)。
* `{BUCKETNAME}`: Amazon S3バケット。リアルタイムCDPがデータエクスポートをデポジットします。
* `{FILEPATH}`: Amazon S3バケットディレクトリ内の、リアルタイムCDPがデータエクスポートをデポジットするパスです。

**応答**

正常な応答を返すと、新たに作成されたターゲット接続と電子メールマーケティングの宛先との間の一意の識別子(`id`)が返されます。 この値は、後の手順で必要となるので保存します。

```json
{
    "id": "12ab90c7-519c-4291-bd20-d64186b62da8"
}
```

## データフローの作成

![宛先手順の概要：手順4](../images/destinations/flow-api-destinations-step4.png)

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
   
        "name": "Activate segments to Adobe Campaign",
        "description": "This operation creates a dataflow which we will later use to activate segments to Adobe Campaign",
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
                    "segmentSelectors": {
                        "selectors": []
                    },
                    "profileSelectors": {
                        "selectors": []
                    }
                }
            }
        ]
    }
```

* `{FLOW_SPEC_ID}`: 接続先の電子メールマーケティングの宛先のフローを使用します。 フロー仕様を取得するには、エンドポイントでGET操作を実行し `flowspecs` ます。 Swaggerのドキュメントはこちらを参照してください。 https://platform.adobe.io/data/foundation/flowservice/swagger#/Flow%20Specs%20API/getFlowSpecs 応答で、接続先の電子メールマーケティング `upsTo` 先の対応するIDを探してコピーします。 例えば、Adobe Campaignの場合は、パラメーターを探し `upsToCampaign` てコピー `id` します。
* `{SOURCE_CONNECTION_ID}`: 手順「エクスペリエンスプラットフォームへの [接続」で取得したソース接続IDを使用します](#connect-to-your-experience-platform-data)。
* `{TARGET_CONNECTION_ID}`: 手順「電子メールマーケティングの [宛先に接続」で取得したターゲット接続IDを使用します](#connect-to-email-marketing-destination)。

**応答**

正常な応答は、新しく作成されたデータフローのID(`id``etag`)と、 両方の値を書き留めておきます。 次の手順で行うように、セグメントをアクティブにします。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい送信先にデータをアクティブにする

![宛先手順の概要：手順5](../images/destinations/flow-api-destinations-step5.png)

すべての接続とデータフローを作成したら、電子メールマーケティングプラットフォームに対してプロファイルデータをアクティブ化できます。 この手順では、送信先に送信するセグメントとプロファイル属性を選択し、スケジュールを設定して送信先にデータを送信できます。

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
* `{SEGMENT_ID}`: この宛先にエクスポートするセグメントIDを指定します。 アクティブ化するセグメントのセグメントIDを取得するには、https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/にアクセスし、 `GET /segment/jobs` 操作を探します。
* `{PROFILE_ATTRIBUTE}`: 例えば、 `"person.lastName"`

**応答**

202 OK応答を探します。 応答本文は返されません。 リクエストが正しいことを検証するには、次の手順「データフローを検証する」を参照してください。

## データフローの検証

![宛先手順の概要手順6](../images/destinations/flow-api-destinations-step6.png)

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

このチュートリアルに従うと、Real-time CDPを優先メール・マーケティング先の1つに接続し、それぞれの宛先にデータ・フローを設定できます。 送信データは、電子メールキャンペーン、ターゲット広告、その他多くの使用例の送信先で使用できるようになりました。 詳しくは、次のページを参照してください。

* [宛先の概要](../../rtcdp/destinations/destinations-overview.md)
* [宛先カタログの概要](../../rtcdp/destinations/destinations-catalog.md)