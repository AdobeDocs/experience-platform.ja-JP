---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用して汎用ODataコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 2%

---


# APIを使用して [!DNL Generic OData][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>コネクタ [!DNL Generic OData] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順を順を追って説明 [!DNL Generic OData] し [!DNL Experience Platform]ます。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Flow Service] APIを使用してODataに正常に接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

ODataと接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| url | サービスのルートURL [!DNL OData] です。 |
| connectionSpec.id | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL OData] は次のとおりです。 `8e6b41a8-d998-4545-ad7d-c6a9fff406c3` |

使い始める前に詳しくは、 [このODataドキュメントを参照してください](https://www.odata.org/getting-started/basic-tutorial/)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Flow Service]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL OData] アカウントごとに必要な接続は1つだけです。

**API形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL OData] 一意の接続指定IDをPOSTリクエストの一部として指定する必要があります。 の接続指定ID [!DNL OData] はです `8e6b41a8-d998-4545-ad7d-c6a9fff406c3`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Protocols",
        "description": "A test connection for a Protocols source",
        "auth": {
            "specName": "Anonymous Authentication",
        "params": {
            "url" :  "{URL}"
            }
        },
        "connectionSpec": {
            "id": "8e6b41a8-d998-4545-ad7d-c6a9fff406c3",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.url` | サー [!DNL OData] バーのホスト。 |
| `connectionSpec.id` | 接続 [!DNL OData] 指定ID: `8e6b41a8-d998-4545-ad7d-c6a9fff406c3`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "a5c6b647-e784-4b58-86b6-47e784ab580b",
    "etag": "\"7b01056a-0000-0200-0000-5e8a4f5b0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL OData] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してプロトコルアプリケーションを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/protocols.md)。