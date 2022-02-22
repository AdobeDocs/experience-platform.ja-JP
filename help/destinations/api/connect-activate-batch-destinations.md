---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: フローサービス API を使用してバッチ宛先に接続し、データをアクティブ化する
description: フローサービス API を使用してバッチクラウドストレージまたは電子メールマーケティングの宛先をExperience Platformで作成し、データをアクティブ化する手順を説明します
topic-legacy: tutorial
type: Tutorial
exl-id: 41fd295d-7cda-4ab1-a65e-b47e6c485562
source-git-commit: a8a8b3b9e4fdae11be95d2fa80abc0f356eff345
workflow-type: tm+mt
source-wordcount: '3083'
ht-degree: 20%

---

# フローサービス API を使用してバッチ宛先に接続し、データをアクティブ化する

このチュートリアルでは、フローサービス API を使用してバッチを作成する方法を示します [クラウドストレージ](../catalog/cloud-storage/overview.md) または [電子メールマーケティングの宛先](../catalog/email-marketing/overview.md)、新しく作成した宛先にデータフローを作成し、CSV ファイルを使用して新しく作成した宛先にデータを書き出します。

このチュートリアルでは、 [!DNL Adobe Campaign] の宛先に関する情報はすべての例で同じですが、手順はすべてのバッチクラウドストレージと電子メールマーケティングの宛先で同じです。

![概要 - 宛先の作成手順とセグメントのアクティブ化の手順](../assets/api/email-marketing/overview.png)

Platform ユーザーインターフェイスを使用して宛先に接続し、データをアクティブ化する場合は、 [宛先の接続](../ui/connect-destination.md) および [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../ui/activate-batch-profile-destinations.md) チュートリアル

## はじめに {#get-started}

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
* [[!DNL Segmentation Service]](../../segmentation/api/overview.md): [!DNL Adobe Experience Platform Segmentation Service] では、セグメントを作成し、 [!DNL Adobe Experience Platform] から [!DNL Real-time Customer Profile] データ。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、単一を分割する仮想サンドボックスを提供します [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立てます。

以下の節では、Platform でバッチ宛先に対してデータをアクティブ化する際に知っておく必要がある追加情報を示します。

### 必要な資格情報の収集 {#gather-required-credentials}

このチュートリアルの手順を完了するには、接続してセグメントをアクティブ化する宛先のタイプに応じて、次の資格情報を準備しておく必要があります。

* の場合 [!DNL Amazon S3] 接続： `accessId`, `secretKey`
* の場合 [!DNL Amazon S3] 接続 [!DNL Adobe Campaign]: `accessId`, `secretKey`
* SFTP 接続の場合： `domain`, `port`, `username`, `password` または `sshKey` （FTP の場所への接続方法に応じて）
* の場合 [!DNL Azure Blob] 接続： `connectionString`

>[!NOTE]
>
>資格情報 `accessId`, `secretKey` 対象 [!DNL Amazon S3] 接続と `accessId`, `secretKey` 対象 [!DNL Amazon S3] 接続 [!DNL Adobe Campaign] が同一である。

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーおよびオプションヘッダーの値の収集 {#gather-values-headers}

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{IMS_ORG}`

のリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離できます。 へのリクエスト内 [!DNL Platform] API を使用して、操作を実行するサンドボックスの名前と ID を指定できます。 次に、オプションのパラメーターを示します。

* x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

### API リファレンスドキュメント {#api-reference-documentation}

このチュートリアルに含まれるすべての API 操作に関する参照用ドキュメントを参照できます。 詳しくは、 [Adobe I/Oに関するフローサービス API ドキュメント](https://www.adobe.io/experience-platform-apis/references/flow-service/). このチュートリアルと API リファレンスドキュメントを並行して使用することをお勧めします。

## 使用可能な宛先のリストを取得する {#get-the-list-of-available-destinations}

![宛先の指定手順の概要 - 手順 1](../assets/api/batch-destination/step1.png)

最初の手順として、データをアクティブ化する宛先を決定する必要があります。 最初に、接続してセグメントをアクティブ化できる、使用可能な宛先のリストを要求する呼び出しを実行します。`connectionSpecs` エンドポイントに次の GET リクエストを実行すると、使用可能な宛先のリストが返されます。

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

リクエストが成功した場合、使用可能な宛先のリストと、その一意の ID（`id`）が返されます。使用する宛先の値を保存します。この値は、以降の手順で必要になります。例えば、に接続してセグメントを配信する場合は、 [!DNL Adobe Campaign]の場合、応答内で次のスニペットを探します。

```json
{
    "id": "0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
  "name": "Adobe Campaign",
  ...
  ...
}
```

参照用に、次の表に、一般的に使用されるバッチ宛先の接続仕様 ID を示します。

| 宛先 | 接続仕様 ID |
---------|----------|
| [!DNL Adobe Campaign] | `0b23e41a-cb4a-4321-a78f-3b654f5d7d97` |
| [!DNL Amazon S3] | `4890fc95-5a1f-4983-94bb-e060c08e3f81` |
| [!DNL Azure Blob] | `e258278b-a4cf-43ac-b158-4fa0ca0d948b` |
| [!DNL Oracle Eloqua] | `c1e44b6b-e7c8-404b-9031-58f0ef760604` |
| [!DNL Oracle Responsys] | `a5e28ddf-e265-426e-83a1-9d03a3a6822b` |
| [!DNL Salesforce Marketing Cloud] | `f599a5b3-60a7-4951-950a-cc4115c7ea27` |
| SFTP | `64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0` |

{style=&quot;table-layout:auto&quot;}

## に接続 [!DNL Experience Platform] データ {#connect-to-your-experience-platform-data}

![宛先の指定手順の概要 - 手順 2](../assets/api/batch-destination/step2.png)

次に、 [!DNL Experience Platform] データを書き出し、目的の宛先でプロファイルデータをアクティブ化できます。 そのためには、以下に示す 2 つの手順を実行します。

1. 最初に、でのデータへのアクセスを認証する呼び出しを実行する必要があります。 [!DNL Experience Platform]（ベース接続を設定して）
2. 次に、ベース接続 ID を使用して、 *ソース接続*( [!DNL Experience Platform] データ。

### のデータへのアクセスを認証 [!DNL Experience Platform]

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

| プロパティ | 説明 |
| --------- | ----------- |
| `name` | Experience Platformへのベース接続の名前を指定 [!DNL Profile Store]. |
| `description` | オプションで、ベース接続の説明を指定できます。 |
| `connectionSpec.id` | の接続仕様 ID を使用します。 [Experience Platformプロファイルストア](/help/profile/home.md#profile-data-store) - `8a9c3494-9708-43d7-ae3f-cda01e5030e1`. |

{style=&quot;table-layout:auto&quot;}

**応答**

リクエストが成功した場合、ベース接続の一意の ID（`id`）が返されます。この値は、次の手順でソース接続を作成する際に必要になるため保存します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### に接続 [!DNL Experience Platform] データ {#connect-to-platform-data}

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
            "name": "Connecting to Profile Store",
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

| プロパティ | 説明 |
| --------- | ----------- |
| `name` | Experience Platformへのソース接続の名前を指定 [!DNL Profile Store]. |
| `description` | 必要に応じて、ソース接続の説明を入力できます。 |
| `connectionSpec.id` | の接続仕様 ID を使用します。 [Experience Platformプロファイルストア](/help/profile/home.md#profile-data-store) - `8a9c3494-9708-43d7-ae3f-cda01e5030e1`. |
| `baseConnectionId` | 前の手順で取得したベース接続 ID を使用します。 |
| `data.format` | `CSV` は、現在、サポートされている唯一のファイル書き出し形式です。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、一意の識別子 (`id`) をクリックします。 [!DNL Profile Store]. これにより、 [!DNL Experience Platform] データ。 この値は、後の手順で必要になるため保存します。

```json
{
    "id": "ed48ae9b-c774-4b6e-88ae-9bc7748b6e97"
}
```

## バッチ保存先に接続 {#connect-to-batch-destination}

![宛先の指定手順の概要 - 手順 3](../assets/api/batch-destination/step3.png)

この手順では、目的のバッチクラウドストレージまたは電子メールマーケティングの宛先への接続を設定します。 そのためには、以下に示す 2 つの手順を実行します。

1. まず、ベース接続を設定して、宛先プラットフォームへのアクセスを認証する呼び出しを実行する必要があります。
2. 次に、ベース接続 ID を使用して、 *ターゲット接続*：書き出されたデータファイルが配信されるストレージアカウント内の場所と、書き出されるデータの形式を指定します。

### バッチ保存先へのアクセスを許可 {#authorize-access-to-batch-destination}

**API 形式**

```http
POST /connections
```

**リクエスト**

以下のリクエストは、 [!DNL Adobe Campaign] 宛先。 ファイルの書き出し先のストレージの場所に応じて ([!DNL Amazon S3], SFTP, [!DNL Azure Blob])、適切な `auth` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "S3 Connection for Adobe Campaign",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
        "version": "1.0"
    },
    "auth": {
        "specName": "S3",
        "params": {
            "accessId": "{ACCESS_ID}",
            "secretKey": "{SECRET_KEY}"
        }
    }
    "auth": {
        "specName": "SFTP with Password",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "password": "{PASSWORD}"
        }
    }
    "auth": {
        "specName": "SFTP with SSH Key",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "sshKey": "{SSH_KEY}"
        }
    }        
    "auth": {
        "specName": "Azure Blob",
        "params": {
            "connectionString": "{AZURE_BLOB_CONNECTION_STRING}"
        }
    }    
}'
```

サポートされている他のバッチクラウドストレージおよび電子メールマーケティングの宛先に接続するためのリクエスト例を以下に示します。

+++ 接続リクエストの例 [!DNL Amazon S3] 宛先

以下のリクエストは、 [!DNL Amazon S3] 宛先。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connect to Amazon S3",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "4890fc95-5a1f-4983-94bb-e060c08e3f81",
        "version": "1.0"
    },
    "auth": {
        "specName": "Access Key",
        "params": {
            "s3AccessKey": "{AMAZON_S3_ACCESS_KEY}",
            "s3SecretKey": "{AMAZON_S3_SECRET_KEY}"
        }
    }
}'
```

+++

+++ 接続リクエストの例 [!DNL Azure Blob] 宛先

以下のリクエストは、 [!DNL Azure Blob] 宛先。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connect to Azure Blob",
    "description": "Summer advertising campaign",
    "connectionSpec": {
        "id": "e258278b-a4cf-43ac-b158-4fa0ca0d948b",
        "version": "1.0"
    },
    "auth": {
        "specName": "ConnectionString",
        "params": {
            "connectionString": "{AZURE_BLOB_CONNECTION_STRING}"
        }
    }
}'
```

+++

+++ 接続リクエストの例 [!DNL Oracle Eloqua] 宛先

以下のリクエストは、 [!DNL Oracle Eloqua] 宛先。 ファイルの書き出し先のストレージの場所に応じて、適切な `auth` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connect to Eloqua destination",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "c1e44b6b-e7c8-404b-9031-58f0ef760604",
        "version": "1.0"
    },
    "auth": {
        "specName": "SFTP with Password",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "password": "{PASSWORD}"
        }
    }
    "auth": {
        "specName": "SFTP with SSH Key",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "sshKey": "{SSH_KEY}"
        }
    }    
}'
```

+++

+++ 接続リクエストの例 [!DNL Oracle Responsys] 宛先

以下のリクエストは、 [!DNL Oracle Responsys] 宛先。 ファイルの書き出し先のストレージの場所に応じて、適切な `auth` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connect to Responsys destination",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "a5e28ddf-e265-426e-83a1-9d03a3a6822b",
        "version": "1.0"
    },
    "auth": {
        "specName": "SFTP with Password",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "password": "{PASSWORD}"
        }
    }
    "auth": {
        "specName": "SFTP with SSH Key",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "sshKey": "{SSH_KEY}"
        }
    }    
}'
```

+++

+++ 接続リクエストの例 [!DNL Salesforce Marketing Cloud] 宛先

以下のリクエストは、 [!DNL Salesforce Marketing Cloud] 宛先。 ファイルの書き出し先のストレージの場所に応じて、適切な `auth` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connect to Salesforce Marketing Cloud",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "f599a5b3-60a7-4951-950a-cc4115c7ea27",
        "version": "1.0"
    },
    "auth": {
        "specName": "SFTP with Password",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "password": "{PASSWORD}"
        }
    }
    "auth": {
        "specName": "SFTP with SSH Key",
        "params": {
            "domain": "{DOMAIN}",
            "host": "{HOST}",
            "username": "{USERNAME}",
            "sshKey": "{SSH_KEY}"
        }
    }    
}'
```

+++

+++ パスワード宛先を使用した SFTP への接続のリクエスト例

以下のリクエストは、SFTP の宛先へのベース接続を確立します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/connections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Connect to SFTP with password",
    "description": "summer advertising campaign",
    "connectionSpec": {
        "id": "64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0",
        "version": "1.0"
    },
    "auth": {
        "specName": "Basic Authentication for sftp",
        "params": {
            "host": "{HOST}",
            "username": "{USERNAME}",
            "password": "{PASSWORD}"
        }
    }
}'
```

+++

| プロパティ | 説明 |
| --------- | ----------- |
| `name` | バッチ保存先へのベース接続の名前を指定します。 |
| `description` | オプションで、ベース接続の説明を指定できます。 |
| `connectionSpec.id` | 目的のバッチ保存先に接続仕様 ID を使用します。 この ID は手順で取得済みです [使用可能な宛先のリストを取得する](#get-the-list-of-available-destinations). |
| `auth.specname` | 宛先の認証形式を示します。 宛先の specName を調べるには、 [接続仕様エンドポイントへのGET呼び出し](https://developer.adobe.com/experience-platform-apis/references/flow-service/#operation/retrieveConnectionSpec)：目的の宛先の接続仕様を指定します。 パラメーターを探します。 `authSpec.name` を返します。 <br> 例えば、Adobe Campaignの宛先の場合は、任意の `S3`, `SFTP with Password`または `SFTP with SSH Key`. |
| `params` | 接続先に応じて、異なる必須認証パラメーターを指定する必要があります。 Amazon S3 接続の場合、Amazon S3 ストレージの場所にアクセス ID と秘密鍵を指定する必要があります。 <br> 宛先に必要なパラメーターを調べるには、 [接続仕様エンドポイントへのGET呼び出し](https://developer.adobe.com/experience-platform-apis/references/flow-service/#operation/retrieveConnectionSpec)：目的の宛先の接続仕様を指定します。 パラメーターを探します。 `authSpec.spec.required` を返します。 |

{style=&quot;table-layout:auto&quot;}

**応答**

リクエストが成功した場合、ベース接続の一意の ID（`id`）が返されます。この値は、次の手順でターゲット接続を作成する際に必要になるため保存します。

```json
{
    "id": "1ed86558-59b5-42f7-9865-5859b552f7f4"
}
```

### ストレージの場所とデータ形式を指定する {#specify-storage-location-data-format}

[!DNL Adobe Experience Platform] 一括電子メールマーケティングおよびクラウドストレージの宛先のデータを [!DNL CSV] ファイル。 この手順では、ファイルが書き出されるストレージの場所のパスを指定できます。

>[!IMPORTANT]
> 
>[!DNL Adobe Experience Platform] は、1 ファイルあたり 500 万件のレコード（行）でエクスポートファイルを自動的に分割します。 各行は 1 つのプロファイルを表します。
>
>次のように、分割ファイル名には、ファイルが大きなエクスポートの一部であることを示す数字が付加されます。 `filename.csv`, `filename_2.csv`, `filename_3.csv`.

**API 形式**

```http
POST /targetConnections
```

**リクエスト**

以下のリクエストは、 [!DNL Adobe Campaign] の宛先：書き出されたファイルが格納場所に配置される場所を決定します。 ファイルの書き出し先のストレージの場所に応じて、適切な `params` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Adobe Campaign",
    "description": "Connection to Adobe Campaign",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "0b23e41a-cb4a-4321-a78f-3b654f5d7d97",
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
        "bucketName": "{BUCKET_NAME}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
    "params": {
        "mode": "AZURE_BLOB",
        "container": "{CONTAINER}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
    "params": {
        "mode": "FTP",
        "remotePath": "{REMOTE_PATH}",
        "format": "CSV"
    }        
}'
```

次のリクエストの例を参照して、サポートされている他のバッチクラウドストレージおよび電子メールマーケティングの宛先の保存場所を設定します。

+++ のストレージの場所を設定するリクエストの例 [!DNL Amazon S3] 宛先

以下のリクエストは、 [!DNL Amazon S3] の宛先：書き出されたファイルが格納場所に配置される場所を決定します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Amazon S3",
    "description": "Connection to Amazon S3",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "4890fc95-5a1f-4983-94bb-e060c08e3f81",
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
        "mode": "Cloud Storage",
        "bucketName": "{BUCKET_NAME}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
}'
```

+++

+++ のストレージの場所を設定するリクエストの例 [!DNL Azure Blob] 宛先

以下のリクエストは、 [!DNL Azure Blob] の宛先：書き出されたファイルが格納場所に配置される場所を決定します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Azure Blob",
    "description": "Connection to Azure Blob",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "e258278b-a4cf-43ac-b158-4fa0ca0d948b",
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
        "mode": "Cloud Storage",
        "container": "{CONTAINER}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
}'
```

+++

+++ のストレージの場所を設定するリクエストの例 [!DNL Oracle Eloqua] 宛先

以下のリクエストは、 [!DNL Oracle Eloqua] の宛先：書き出されたファイルが格納場所に配置される場所を決定します。 ファイルの書き出し先のストレージの場所に応じて、適切な `params` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Oracle Eloqua",
    "description": "Connection to Oracle Eloqua",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "c1e44b6b-e7c8-404b-9031-58f0ef760604",
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
        "bucketName": "{BUCKET_NAME}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
    "params": {
        "mode": "FTP",
        "remotePath": "{REMOTE_PATH}",
        "format": "CSV"
    }        
}'
```

+++

+++ のストレージの場所を設定するリクエストの例 [!DNL Oracle Responsys] 宛先

以下のリクエストは、 [!DNL Oracle Responsys] の宛先：書き出されたファイルが格納場所に配置される場所を決定します。 ファイルの書き出し先のストレージの場所に応じて、適切な `params` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Oracle Responsys",
    "description": "Connection to Oracle Responsys",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "a5e28ddf-e265-426e-83a1-9d03a3a6822b",
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
        "bucketName": "{BUCKET_NAME}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
    "params": {
        "mode": "FTP",
        "remotePath": "{REMOTE_PATH}",
        "format": "CSV"
    }        
}'
```

+++

+++ のストレージの場所を設定するリクエストの例 [!DNL Salesforce Marketing Cloud] 宛先

以下のリクエストは、 [!DNL Salesforce Marketing Cloud] の宛先：書き出されたファイルが格納場所に配置される場所を決定します。 ファイルの書き出し先のストレージの場所に応じて、適切な `params` を指定し、その他を削除します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for Salesforce Marketing Cloud",
    "description": "Connection to Salesforce Marketing Cloud",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "f599a5b3-60a7-4951-950a-cc4115c7ea27",
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
        "bucketName": "{BUCKET_NAME}",
        "path": "{FILEPATH}",
        "format": "CSV"
    }
    "params": {
        "mode": "FTP",
        "remotePath": "{REMOTE_PATH}",
        "format": "CSV"
    }        
}'
```

+++

+++ SFTP の宛先のストレージの場所を設定するリクエストの例

以下のリクエストは、SFTP の宛先へのターゲット接続を確立し、書き出されたファイルが格納先となる場所を決定します。

```shell
curl --location --request POST 'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "TargetConnection for SFTP",
    "description": "Connection to SFTP",
    "baseConnectionId": "{BASE_CONNECTION_ID}",
    "connectionSpec": {
        "id": "64ef4b8b-a6e0-41b5-9677-3805d1ee5dd0",
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
        "mode": "Cloud Storage",
        "remotePath": "{REMOTE_PATH}",
    }
}'
```

+++


| プロパティ | 説明 |
| --------- | ----------- |
| `name` | バッチ保存先へのターゲット接続の名前を指定します。 |
| `description` | オプションで、ターゲット接続の説明を入力できます。 |
| `baseConnectionId` | 上記の手順で作成したベース接続の ID を使用します。 |
| `connectionSpec.id` | 目的のバッチ保存先に接続仕様 ID を使用します。 この ID は手順で取得済みです [使用可能な宛先のリストを取得する](#get-the-list-of-available-destinations). |
| `params` | 接続先の宛先に応じて、ストレージの場所に異なる必須パラメーターを指定する必要があります。 Amazon S3 接続の場合、Amazon S3 ストレージの場所にアクセス ID と秘密鍵を指定する必要があります。 <br> 宛先に必要なパラメーターを調べるには、 [接続仕様エンドポイントへのGET呼び出し](https://developer.adobe.com/experience-platform-apis/references/flow-service/#operation/retrieveConnectionSpec)：目的の宛先の接続仕様を指定します。 パラメーターを探します。 `targetSpec.spec.required` を返します。 |
| `params.mode` | 宛先でサポートされているモードに応じて、ここで異なる値を指定する必要があります。 宛先に必要なパラメーターを調べるには、 [接続仕様エンドポイントへのGET呼び出し](https://developer.adobe.com/experience-platform-apis/references/flow-service/#operation/retrieveConnectionSpec)：目的の宛先の接続仕様を指定します。 パラメーターを探します。 `targetSpec.spec.properties.mode.enum` を選択し、目的のモードを選択します。 |
| `params.bucketName` | S3 接続の場合、ファイルの書き出し先のバケットの名前を指定します。 |
| `params.path` | S3 接続の場合は、ファイルの書き出し先となるストレージの場所にファイルパスを指定します。 |
| `params.format` | `CSV` は、現在、サポートされている唯一のファイル書き出しタイプです。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、一意の識別子 (`id`) を使用します。 この値は、後の手順で必要になるため保存します。

```json
{
    "id": "12ab90c7-519c-4291-bd20-d64186b62da8"
}
```

## データフローの作成 {#create-dataflow}

![宛先の指定手順の概要 - 手順 4](../assets/api/batch-destination/step4.png)

前の手順で取得したフロー仕様、ソース接続、ターゲット接続 ID を使用して、 [!DNL Experience Platform] データと、データファイルを書き出す宛先。 この手順は、後でデータが相互に流れるパイプラインを構築すると考えます [!DNL Experience Platform] および目的の宛先。

データフローを作成するには、以下に示すようにPOSTリクエストを実行し、ペイロード内で以下に示す値を指定します。

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

| プロパティ | 説明 |
| --------- | ----------- |
| `name` | 作成するデータフローの名前を指定します。 |
| `description` | オプションで、データフローの説明を指定できます。 |
| `flowSpec.Id` | 接続先のバッチ保存先にフロー仕様 ID を使用します。 フロー仕様 ID を取得するには、 `flowspecs` エンドポイント ( [フロー仕様 API リファレンスドキュメント](https://www.adobe.io/experience-platform-apis/references/flow-service/#operation/retrieveFlowSpec). 応答で、 `upsTo` と、接続先のバッチ保存先の対応する ID をコピーします。 例えば、Adobe Campaign の場合は、`upsToCampaign` を探して、`id` パラメーターをコピーします。 |
| `sourceConnectionIds` | 手順で取得したソース接続 ID を使用します [Experience Platformデータに接続](#connect-to-your-experience-platform-data). |
| `targetConnectionIds` | 手順で取得したターゲット接続 ID を使用します [バッチ保存先に接続](#connect-to-batch-destination). |
| `transformations` | 次の手順では、アクティブ化するセグメントとプロファイル属性をこのセクションに入力します。 |

参照用に、次の表に、一般的に使用されるバッチ保存先のフロー仕様 ID を示します。

| 宛先 | フロー仕様 ID |
---------|----------|
| すべてのクラウドストレージの宛先 ([!DNL Amazon S3], SFTP, [!DNL Azure Blob]) および [!DNL Oracle Eloqua] | `71471eba-b620-49e4-90fd-23f1fa0174d8` |
| [!DNL Oracle Responsys] | `51d675ce-e270-408d-91fc-22717bdf2148` |
| [!DNL Salesforce Marketing Cloud] | `493b2bd6-26e4-4167-ab3b-5e910bba44f0` |

**応答** 

リクエストが成功した場合は、新しく作成したデータフローの ID（`id`）と `etag` が返されます。次の手順でセグメントをアクティブ化し、データファイルを書き出すために、両方の値が必要になるのでメモしておきます。

```json
{
    "id": "8256cfb4-17e6-432c-a469-6aedafb16cd5",
    "etag": "8256cfb4-17e6-432c-a469-6aedafb16cd5"
}
```


## 新しい宛先に対してデータをアクティブ化にする {#activate-data}

![宛先の指定手順の概要 - 手順 5](../assets/api/batch-destination/step5.png)

すべての接続とデータフローを作成したら、宛先プラットフォームに対してプロファイルデータをアクティブ化できます。 この手順では、宛先に書き出すセグメントとプロファイル属性を選択します。

また、書き出すファイルのファイル命名形式や、どの属性を使用するかを指定することもできます [重複排除キー](../ui/activate-batch-profile-destinations.md#mandatory-keys) または [必須属性](../ui/activate-batch-profile-destinations.md#mandatory-attributes). この手順では、宛先にデータを送信するスケジュールも指定できます。

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
                "id": "{SEGMENT_ID}",
                "filenameTemplate": "%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                "exportMode": "DAILY_FULL_EXPORT",
                "schedule": {
                    "frequency": "ONCE",
                    "startDate": "2021-12-20",
                    "startTime": "17:00"
                } 
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
                "id": "{SEGMENT_ID}",
                "filenameTemplate": "%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                "exportMode": "DAILY_FULL_EXPORT",
                "schedule": {
                    "frequency": "ONCE",
                    "startDate": "2021-12-20",
                    "startTime": "17:00"
                },   
            }
        }
    },
{
        "op": "add",
        "path": "/transformations/0/params/profileSelectors/selectors/-",
        "value": {
            "type": "JSON_PATH",
            "value": {
                "path": "{PROFILE_ATTRIBUTE}"
            }
        }
    }
]
```

| プロパティ | 説明 |
| --------- | ----------- |
| `{DATAFLOW_ID}` | URL 内で、前の手順で作成したデータフローの ID を使用します。 |
| `{ETAG}` | 前の手順で取得した ETag を使用します。 |
| `{SEGMENT_ID}` | この宛先に書き出すセグメント ID を指定します。 アクティブ化するセグメントのセグメント ID の取得方法については、 [セグメント定義の取得](https://www.adobe.io/experience-platform-apis/references/segmentation/#operation/retrieveSegmentDefinitionById) (Experience PlatformAPI リファレンス ) |
| `{PROFILE_ATTRIBUTE}` | 例：`"person.lastName"`。 |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。セグメントをデータフローに追加するには、 `add` 操作。 |
| `path` | 更新するフローの部分を定義します。 セグメントをデータフローに追加する場合は、例で指定したパスを使用します。 |
| `value` | パラメーターの更新に使用する新しい値。 |
| `id` | 宛先データフローに追加するセグメントの ID を指定します。 |
| `name` | *オプション*. 宛先データフローに追加するセグメントの名前を指定します。 このフィールドは必須ではなく、名前を指定せずにセグメントを宛先データフローに正常に追加できることに注意してください。 |
| `filenameTemplate` | このフィールドは、宛先に書き出すファイルのファイル名の形式を決定します。 <br> 次のオプションを使用できます。 <br> <ul><li>`%DESTINATION_NAME%`: 必須. 書き出されるファイルには、宛先名が含まれます。</li><li>`%SEGMENT_ID%`: 必須. 書き出されたファイルには、書き出されたセグメントの ID が含まれます。</li><li>`%SEGMENT_NAME%`: オプション. 書き出されたファイルには、書き出されたセグメントの名前が含まれます。</li><li>`DATETIME(YYYYMMdd_HHmmss)` または `%TIMESTAMP%`:オプション。 ファイルがExperience Platformで生成された時刻を含めるには、次の 2 つのオプションのいずれかを選択します。</li><li>`custom-text`: オプション. ファイル名の末尾に追加するカスタムテキストで、このプレースホルダーを置き換えます。</li></ul> <br> ファイル名の設定について詳しくは、 [ファイル名の設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names) の節を参照してください。 |
| `exportMode` | 必須. `"DAILY_FULL_EXPORT"` または `"FIRST_FULL_THEN_INCREMENTAL"` を選択します。この 2 つのオプションについて詳しくは、 [完全なファイルをエクスポート](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) および [増分ファイルの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files) （バッチ保存先のアクティベーションに関するチュートリアル）。 |
| `startDate` | セグメントが宛先へのプロファイルの書き出しを開始する日付を選択します。 |
| `frequency` | 必須. <br> <ul><li>の `"DAILY_FULL_EXPORT"` エクスポートモード： `ONCE` または `DAILY`.</li><li>の `"FIRST_FULL_THEN_INCREMENTAL"` エクスポートモード： `"DAILY"`, `"EVERY_3_HOURS"`, `"EVERY_6_HOURS"`, `"EVERY_8_HOURS"`, `"EVERY_12_HOURS"`.</li></ul> |
| `endDate` | 選択時には適用されません `"exportMode":"DAILY_FULL_EXPORT"` および `"frequency":"ONCE"`. <br> セグメントメンバーが宛先への書き出しを停止する日付を設定します。 |
| `startTime` | 必須. セグメントのメンバーを含むファイルを生成し、宛先に書き出す時間を選択します。 |

{style=&quot;table-layout:auto&quot;}

**応答**

202 受け入れられた応答を探します。 レスポンスの本文は返されません。リクエストが正しいことを検証するには、次の手順を参照してください。 [データフローの検証](#validate-dataflow).

## データフローの検証 {#validate-dataflow}

![宛先の指定手順の概要 - 手順 6](../assets/api/batch-destination/step6.png)

このチュートリアルの最後の手順では、セグメントとプロファイル属性が実際にデータフローに正しくマッピングされていることを検証する必要があります。

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

* `{DATAFLOW_ID}`:前の手順のデータフローを使用します。
* `{ETAG}`：前述の手順で取得した Etag を使用します。

**応答** 

返されたレスポンスの `transformations` パラメーターに、前述の手順で送信したセグメントとプロファイル属性が含まれています。レスポンス内のサンプル `transformations` パラメーターは次のようになります。

```json
"transformations":[
   {
      "name":"GeneralTransform",
      "params":{
         "profileSelectors":{
            "selectors":[
               {
                  "type":"JSON_PATH",
                  "value":{
                     "path":"homeAddress.countryCode",
                     "operator":"EXISTS",
                     "mapping":{
                        "sourceType":"text/x.schema-path",
                        "source":"homeAddress.countryCode",
                        "destination":"homeAddress.countryCode",
                        "identity":false,
                        "primaryIdentity":false,
                        "functionVersion":0,
                        "copyModeMapping":false,
                        "sourceAttribute":"homeAddress.countryCode",
                        "destinationXdmPath":"homeAddress.countryCode"
                     }
                  }
               },
               {
                  "type":"JSON_PATH",
                  "value":{
                     "path":"person.name.firstName",
                     "operator":"EXISTS",
                     "mapping":{
                        "sourceType":"text/x.schema-path",
                        "source":"person.name.firstName",
                        "destination":"person.name.firstName",
                        "identity":false,
                        "primaryIdentity":false,
                        "functionVersion":0,
                        "copyModeMapping":false,
                        "sourceAttribute":"person.name.firstName",
                        "destinationXdmPath":"person.name.firstName"
                     }
                  }
               },
               {
                  "type":"JSON_PATH",
                  "value":{
                     "path":"person.name.lastName",
                     "operator":"EXISTS",
                     "mapping":{
                        "sourceType":"text/x.schema-path",
                        "source":"person.name.lastName",
                        "destination":"person.name.lastName",
                        "identity":false,
                        "primaryIdentity":false,
                        "functionVersion":0,
                        "copyModeMapping":false,
                        "sourceAttribute":"person.name.lastName",
                        "destinationXdmPath":"person.name.lastName"
                     }
                  }
               },
               {
                  "type":"JSON_PATH",
                  "value":{
                     "path":"personalEmail.address",
                     "operator":"EXISTS",
                     "mapping":{
                        "sourceType":"text/x.schema-path",
                        "source":"personalEmail.address",
                        "destination":"personalEmail.address",
                        "identity":false,
                        "primaryIdentity":false,
                        "functionVersion":0,
                        "copyModeMapping":false,
                        "sourceAttribute":"personalEmail.address",
                        "destinationXdmPath":"personalEmail.address"
                     }
                  }
               },
               {
                  "type":"JSON_PATH",
                  "value":{
                     "path":"segmentMembership.status",
                     "operator":"EXISTS",
                     "mapping":{
                        "sourceType":"text/x.schema-path",
                        "source":"segmentMembership.status",
                        "destination":"segmentMembership.status",
                        "identity":false,
                        "primaryIdentity":false,
                        "functionVersion":0,
                        "copyModeMapping":false,
                        "sourceAttribute":"segmentMembership.status",
                        "destinationXdmPath":"segmentMembership.status"
                     }
                  }
               }
            ],
            "mandatoryFields":[
               "person.name.firstName",
               "person.name.lastName"
            ],
            "primaryFields":[
               {
                  "fieldType":"ATTRIBUTE",
                  "attributePath":"personalEmail.address"
               }
            ]
         },
         "segmentSelectors":{
            "selectors":[
               {
                  "type":"PLATFORM_SEGMENT",
                  "value":{
                     "id":"9f7d37fd-7039-4454-94ef-2b0cd6c3206a",
                     "name":"Interested in Mountain Biking",
                     "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                     "exportMode":"DAILY_FULL_EXPORT",
                     "schedule":{
                        "frequency":"ONCE",
                        "startDate":"2021-12-20",
                        "startTime":"17:00"
                     },
                     "createTime":"1640016962",
                     "updateTime":"1642534355"
                  }
               },
               {
                  "type":"PLATFORM_SEGMENT",
                  "value":{
                     "id":"25768be6-ebd5-45cc-8913-12fb3f348613",
                     "name":"Loyalty Segment",
                     "filenameTemplate":"%DESTINATION_NAME%_%SEGMENT_ID%_%DATETIME(YYYYMMdd_HHmmss)%",
                     "exportMode":"FIRST_FULL_THEN_INCREMENTAL",
                     "schedule":{
                        "frequency":"EVERY_6_HOURS",
                        "startDate":"2021-12-22",
                        "endDate":"2021-12-31",
                        "startTime":"17:00"
                     },
                     "createTime":"1640016962",
                     "updateTime":"1642534355"
                  }
               }
            ]
         }
      }
   }
]
```

## 次の手順

このチュートリアルに従うと、Platform は、目的のバッチクラウドストレージまたは電子メールマーケティングの宛先の 1 つに接続し、データファイルをエクスポートするための各宛先へのデータフローを設定しました。 これで、発信データを電子メールキャンペーン、ターゲット広告、ほかの多くの使用事例の宛先で使用することができます。フローサービス API を使用した既存のデータフローの編集方法など、詳しくは、次のページを参照してください。

* [宛先の概要](../home.md)
* [宛先カタログの概要](../catalog/overview.md)
* [フローサービス API を使用した宛先データフローの更新](../api/update-destination-dataflows.md)
