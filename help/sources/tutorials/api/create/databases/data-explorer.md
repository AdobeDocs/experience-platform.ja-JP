---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してAzure Data Explorerコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '605'
ht-degree: 1%

---


# APIを使用した [!DNL Azure Data Explorer][!DNL Flow Service] コネクタの作成

>[!NOTE]
>コネクタ [!DNL Azure Data Explorer] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順(以下「データエクスプローラ」と呼ば [!DNL Azure Data Explorer] れる)を順を追って説明 [!DNL Experience Platform]します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Data Explorer][!DNL Flow Service] APIを使用してに正常に接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があ [!DNL Data Explorer]ります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | サー [!DNL Data Explorer] バーのエンドポイント。 |
| `database` | The name of the [!DNL Data Explorer] database. |
| `tenant` | データベースへの接続に使用する一意のテナントID [!DNL Data Explorer] です。 |
| `servicePrincipalId` | データベースへの接続に使用する一意のサービスプリンシパルID [!DNL Data Explorer] です。 |
| `servicePrincipalKey` | データベースへの接続に使用する一意のサービスプリンシパルキー [!DNL Data Explorer] です。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Data Explorer] はです `0479cc14-7651-4354-b233-7480606c2ac3`。 |

使い始める前に、 [このData Explorerドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのE[!DNL xperience Platform] API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Flow Service]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Data Explorer] アカウントごとに1つのコネクタが必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL Data Explorer] 一意の接続指定IDをPOSTリクエストの一部として指定する必要があります。 の接続指定ID [!DNL Data Explorer] はです `0479cc14-7651-4354-b233-7480606c2ac3`。

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
| `auth.params.endpoint` | サー [!DNL Data Explorer] バーのエンドポイント。 |
| `auth.params.database` | The name of the [!DNL Data Explorer] database. |
| `auth.params.tenant` | データベースへの接続に使用する一意のテナントID [!DNL Data Explorer] です。 |
| `auth.params.servicePrincipalId` | データベースへの接続に使用する一意のサービスプリンシパルID [!DNL Data Explorer] です。 |
| `auth.params.servicePrincipalKey` | データベースへの接続に使用する一意のサービスプリンシパルキー [!DNL Data Explorer] です。 |
| `connectionSpec.id` | 接続 [!DNL Data Explorer] 仕様ID: `0479cc14-7651-4354-b233-7480606c2ac3`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Data Explorer] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
