---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 電子メールマーケティングの宛先の作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 7ee83b5bf14ec802801cfbc17141c02ceeaccd82

---


# アドビのリアルタイム顧客データプラットフォームで電子メールのマーケティング先を作成し、データをアクティブ化する

このチュートリアルでは、API呼び出しを使用して、Adobe Experience Platformデータに接続し、電子メールマーケティングの宛先を作成し、新しく作成した宛先にデータフローを作成し [](../../rtcdp/destinations/email-marketing-destinations.md)、新しく作成した宛先にデータをアクティブ化する方法を説明します。

このチュートリアルでは、Adobe Campaignの宛先をすべての例で使用しますが、手順はすべての電子メールマーケティングの宛先で同じです。

![概要 — 宛先の作成とセグメントのアクティブ化の手順](../images/destinations/flow-api-destinations-steps-overview.png)

アドビのリアルタイムCDPのユーザーインターフェイスを使用して宛先を接続し、データをアクティブにする場合は、「宛先の接続 [」と「宛先のプロファイルとセグメントを宛先のチュートリ](../../rtcdp/destinations/connect-destination.md) アルにアクティブ化する [](../../rtcdp/destinations/activate-destinations.md) 」を参照してください。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
* [Catalog Service](../../catalog/home.md):カタログは、エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

以下の節では、Adobe Real-time CDPで電子メールマーケティングの宛先に対するデータをアクティブ化するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

このチュートリアルの手順を完了するには、セグメントを接続およびアクティブ化する宛先のタイプに応じて、次の資格情報を準備しておく必要があります。

* 電子メールマーケティングプラットフォームへのAmazon S3接続の場合： `accessId``secretKey`
* 電子メールマーケティングプラットフォームへのSFTP接続の場合： `domain`、、、 `port`ま `username``password``ssh key` たは（FTPの場所への接続方法に応じて）

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必須およびオプションのヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのリソースは、特定の仮想サンドボックスに分離できます。 プラットフォームAPIへのリクエストでは、操作を実行するサンドボックスの名前とIDを指定できます。 これらはオプションのパラメータです。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!N注意]
>エクスペリエンスプラットフォームのサンドボックスについて詳しくは、サンドボックスの概要に関するドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

<!--

### Definitions

Before starting this tutorial, familiarize yourself with the following terms which we'll use throughout the tutorial:

**Flow**: 

**Base Connection**: 

**Target Connection**: 

**Source Connection**: 

-->

### Swaggerのドキュメント

Swaggerのこのチュートリアルでは、すべてのAPI呼び出しに関する付属のリファレンスドキュメントを参照できます。 https://platform.adobe.io/data/foundation/flowservice/swagger#/を参照してください。 このチュートリアルとSwaggerのドキュメントページを並行して使用することをお勧めします。

## 使用可能な宛先のリストを取得する {#get-the-list-of-available-destinations}

![宛先手順の概要手順1](../images/destinations/flow-api-destinations-step1.png)

最初の手順として、データをアクティブにする電子メールマーケティングの宛先を決定する必要があります。 最初に、セグメントを接続してアクティブ化できる、使用可能な宛先のリストをリクエストする呼び出しを実行します。 使用可能な宛先のリストを返すには、次のGET `connectionSpecs` リクエストをエンドポイントに実行します。

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

成功した応答には、使用可能な宛先のリストと、その一意の識別子(`id`)が含まれます。 使用する宛先の値を保存します。この値は、以降の手順で必要になります。 例えば、セグメントをAdobe Campaignに接続して配信する場合、応答内で次のスニペットを探します。

```json
{
    "id": "0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
  "name": "Adobe Campaign",
  ...
  ...
}
```

## エクスペリエンスプラットフォームデータへの接続 {#connect-to-your-experience-platform-data}

![宛先手順の概要手順2](../images/destinations/flow-api-destinations-step2.png)

次に、Experience Platformデータに接続し、プロファイルデータを書き出して、希望の宛先でアクティブ化できるようにする必要があります。 これは、以下に説明する2つのサブステップで構成されます。

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


* `{CONNECTION_SPEC_ID}`:統合接続サービスの接続仕様IDを使用します — `8a9c3494-9708-43d7-ae3f-cda01e5030e1`。

**応答**

成功した応答には、ベース接続の一意の識別子(`id`)が含まれます。 この値は、次の手順でソース接続を作成する際に必要な値として格納します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### エクスペリエンスプラットフォームデータへの接続

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

* `{BASE_CONNECTION_ID}`:前の手順で取得したIDを使用します。
* `{CONNECTION_SPEC_ID}`:統合接続サービスの接続仕様IDを使用します — `8a9c3494-9708-43d7-ae3f-cda01e5030e1`。

**応答**

正常な応答は、新しく作成されたソース接続の`id`Unified Identifier()を、統合プロファイルサービスに返します。 これにより、エクスペリエンスプラットフォームデータに正常に接続したことが確認されます。 この値は、後の手順で必要な場合に保存します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```


## 電子メールマーケティングの宛先に接続 {#connect-to-email-marketing-destination}

![宛先手順の概要手順3](../images/destinations/flow-api-destinations-step3.png)

この手順では、目的の電子メールマーケティングの宛先への接続を設定します。 これは、以下に説明する2つのサブステップで構成されます。

1. 最初に、ベース接続を設定して、電子メールサービスプロバイダーへのアクセスを許可する呼び出しを実行する必要があります。
2. 次に、ベース接続IDを使用して、別の呼び出しを行い、ターゲット接続を作成します。この呼び出しでは、書き出されたデータが配信されるストレージアカウント内の場所と、書き出されるデータの形式を指定します。

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

* `{CONNECTION_SPEC_ID}`:手順「使用可能な宛先のリストを取得」で取得した接続 [仕様IDを使用します](#get-the-list-of-available-destinations)。
* `{S3 or SFTP}`:この宛先の接続タイプを入力します。 リンク先カ [タログで](../../rtcdp/destinations/destinations-catalog.md)、目的のリンク先までスクロールして、S3またはSFTPの接続タイプがサポートされているかどうかを確認します。
* `{ACCESS_ID}`:Amazon S3ストレージの場所のアクセスID。
* `{SECRET_KEY}`:Amazon S3ストレージの秘密鍵。

**応答**

成功した応答には、ベース接続の一意の識別子(`id`)が含まれます。 この値は、次の手順で接続を作成する際に必要となるとおりに保存してください。ターゲット接続の作成

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### ストレージの場所とデータ形式の指定

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

* `{BASE_CONNECTION_ID}`:上記の手順で取得したベース接続IDを使用します。
* `{CONNECTION_SPEC_ID}`:手順「使用可能な宛先のリストを取得」で取得し [た接続仕様を使用します](#get-the-list-of-available-destinations)。
* `{BUCKETNAME}`:Amazon S3バケット。リアルタイムCDPがデータエクスポートをデポジットします。
* `{FILEPATH}`:Amazon S3バケットディレクトリ内の、リアルタイムCDPがデータエクスポートをデポジットするパス。

**応答**

成功した応答は、電子メールマーケティングの宛先に対して新しく`id`作成されたターゲット接続の固有な識別子()を返します。 この値は、後の手順で必要な場合に保存します。

```json
{
    "id": "12ab90c7-519c-4291-bd20-d64186b62da8"
}
```

## データフローの作成

![宛先手順の概要手順4](../images/destinations/flow-api-destinations-step4.png)

前の手順で取得したIDを使用して、Experience Platformデータと、データをアクティブ化する宛先との間にデータフローを作成できるようになりました。 この手順は、後でデータが流れるパイプラインを構築し、エクスペリエンスプラットフォームと目的の宛先の間で行うことを考えてください。

データフローを作成するには、以下に示すように、ペイロード内で以下に示す値を指定しながら、POSTリクエストを実行します。

次のPOSTリクエストを実行して、データフローを作成します。

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

* `{FLOW_SPEC_ID}`:接続先の電子メールマーケティングのフローを使用します。 フロー仕様を取得するには、エンドポイントでGET操作を実行 `flowspecs` します。 Swaggerのドキュメントは、こちらを参照してください。https://platform.adobe.io/data/foundation/flowservice/swagger#/Flow%20Specs%20API/getFlowSpecs. 応答で、接続先の電 `upsTo` 子メールマーケティングの宛先の対応するIDを探し、コピーします。 例えば、Adobe Campaignの場合は、パラメータ `upsToCampaign` ーを探してコピー `id` します。
* `{SOURCE_CONNECTION_ID}`:手順「エクスペリエンスプラットフォームへの接続」で取得した [ソース接続IDを使用します](#connect-to-your-experience-platform-data)。
* `{TARGET_CONNECTION_ID}`:手順「電子メールターゲットへの接続」で取得した [マーケティング先への接続IDを使用します](#connect-to-email-marketing-destination)。

**応答**

成功した応答は、新しく作成されたデ`id``etag`ータフローのID()と、 両方の値を控えておきます。 次の手順で行うのと同じように、セグメントをアクティブにします。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい宛先にデータをアクティブにする

![宛先手順の概要手順5](../images/destinations/flow-api-destinations-step5.png)

すべての接続とデータフローを作成したら、電子メールマーケティングプラットフォームに対してプロファイルデータをアクティブ化できます。 この手順では、送信先に送信するセグメントとプロファイル属性を選択し、スケジュールして送信先にデータを送信できます。

新しい宛先に対してセグメントをアクティブ化するには、次の例のようなJSON PATCH操作を実行する必要があります。 1回の呼び出しで複数のセグメントとプロファイル属性をアクティブ化できます。 JSONパッチの詳細については、 [RFC仕様を参照してください](https://tools.ietf.org/html/rfc6902)。

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

* `{DATAFLOW_ID}`:前の手順で取得したデータフローを使用します。
* `{ETAG}`:前の手順で取得したetagを使用します。
* `{SEGMENT_ID}`:この宛先にエクスポートするセグメントIDを指定します。 アクティブにするセグメントのセグメントIDを取得するには、https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/にアクセスし、操作を探し `GET /segment/jobs` ます。
* `{PROFILE_ATTRIBUTE}`:例えば、 `"person.lastName"`

**応答**

202 OK応答を探します。 応答本文は返されません。 リクエストが正しかったことを検証するには、次の手順「データフローの検証」を参照してください。

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

* `{DATAFLOW_ID}`:前の手順のデータフローを使用します。
* `{ETAG}`:前の手順のetagを使用します。

**応答**

返される応答には、前の手順で送信 `transformations` したセグメントとプロファイル属性がパラメーターに含まれる必要があります。 応答内のサ `transformations` ンプルパラメータは次のようになります。

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

このチュートリアルに従うと、Real-time CDPを優先する電子メールマーケティングの宛先の1つに接続し、それぞれの宛先にデータフローを設定できます。 送信データを電子メールキャンペーン、ターゲット広告、その他多くの使用例の送信先で使用できるようになりました。 詳しくは、次のページを参照してください。

* [宛先の概要](../../rtcdp/destinations/destinations-overview.md)
* [宛先カタログの概要](../../rtcdp/destinations/destinations-catalog.md)