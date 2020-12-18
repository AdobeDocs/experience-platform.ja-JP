---
keywords: Experience Platform;home;popular topics;Azure Data Explorer;data explorer;Data Explorer
solution: Experience Platform
title: Flow Service APIを使用してAzureData Explorerコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、AzureData Explorer(以下「Data Explorer」と呼ばれます)をExperience Platformに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 36620a229fc8e6e3fa4545bfc775a49bc89935bb
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 18%

---


# [!DNL Flow Service] APIを使用して[!DNL Azure Data Explorer]コネクタを作成する

>[!NOTE]
>
>[!DNL Azure Data Explorer]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Azure Data Explorer](以下「Data Explorer」と呼びます)を[!DNL Experience Platform]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL Data Explorer]に正しく接続するために知っておく必要のある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Data Explorer]と接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL Data Explorer]サーバーのエンドポイント。 |
| `database` | [!DNL Data Explorer]データベースの名前。 |
| `tenant` | [!DNL Data Explorer]データベースへの接続に使用する一意のテナントID。 |
| `servicePrincipalId` | [!DNL Data Explorer]データベースへの接続に使用する一意のサービスプリンシパルID。 |
| `servicePrincipalKey` | [!DNL Data Explorer]データベースへの接続に使用する一意のサービスプリンシパルキーです。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 [!DNL Data Explorer]の接続指定IDは`0479cc14-7651-4354-b233-7480606c2ac3`です。 |

開始方法の詳細については、[このData Explorerドキュメント](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](../../../../../tutorials/authentication.md)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、[!DNL Data Explorer]アカウントごとに1つのコネクタが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Data Explorer]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Data Explorer]の接続指定IDは`0479cc14-7651-4354-b233-7480606c2ac3`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Azure Data Explorer connection",
        "description": "A connection for Azure Data Explorer",
        "auth": {
            "specName": "Service Principal Based Authentication",
            "params": {
                    "endpoint": "{ENDPOINT}",
                    "database": "{DATABASE}",
                    "tenant": "{TENANT}",
                    "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
                    "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}"
                }
        },
        "connectionSpec": {
            "id": "0479cc14-7651-4354-b233-7480606c2ac3",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.endpoint` | [!DNL Data Explorer]サーバーのエンドポイント。 |
| `auth.params.database` | [!DNL Data Explorer]データベースの名前。 |
| `auth.params.tenant` | [!DNL Data Explorer]データベースへの接続に使用する一意のテナントID。 |
| `auth.params.servicePrincipalId` | [!DNL Data Explorer]データベースへの接続に使用する一意のサービスプリンシパルID。 |
| `auth.params.servicePrincipalKey` | [!DNL Data Explorer]データベースへの接続に使用する一意のサービスプリンシパルキーです。 |
| `connectionSpec.id` | [!DNL Data Explorer]接続仕様ID:`0479cc14-7651-4354-b233-7480606c2ac3`. |

**応答** 

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Data Explorer]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API ](../../explore/database-nosql.md)を使用して[データベースを調査する方法を学習する際に、次のチュートリアルで使用できます。
