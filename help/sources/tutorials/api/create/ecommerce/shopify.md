---
keywords: Experience Platform;home;popular topics;Shopify;shopify;ecommerce
solution: Experience Platform
title: フローサービスAPIを使用してShopifyコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、ShopifyをExperience Platformに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: b31b7dc04d32129ba5522e1b0d3e52a213347a40
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 20%

---


# APIを使用して [!DNL Shopify][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>
>コネクタ [!DNL Shopify] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [[!DNL Flow Service]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml) APIを使用して、に接続する手順を順を追って説明 [!DNL Shopify] し [!DNL Experience Platform]ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [[!DNL Sources]](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to [!DNL Shopify] using the [!DNL Flow Service] API.

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があ [!DNL Shopify]ります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | サー [!DNL Shopify] バーのエンドポイント。 |
| `accessToken` | ユーザーアカウントの [!DNL Shopify] アクセストークン。 |
| `connectionSpec` | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Shopify] は次のとおりです。 `4f63aa36-bd48-4e33-bb83-49fbcd11c708` |

使い始める前に、この [Shopify認証ドキュメントを参照してください](https://shopify.dev/concepts/about-apis/authentication)。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Shopify] アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL Shopify] 一意の接続指定IDをPOST要求の一部として指定する必要があります。 の接続指定ID [!DNL OData] はです `4f63aa36-bd48-4e33-bb83-49fbcd11c708`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Shopify source",
        "description": "Shopify source",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "host": "{HOST}",
                "accessToken": "{ACCCESS_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "4f63aa36-bd48-4e33-bb83-49fbcd11c708",
            "version": "1.0"
        }
    }
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | The endpoint of the [!DNL Shopify] server. |
| `auth.params.accessToken` | ユーザーアカウントの [!DNL Shopify] アクセストークン。 |
| `connectionSpec.id` | 接続 [!DNL Shopify] 指定ID: `4f63aa36-bd48-4e33-bb83-49fbcd11c708`. |

**応答** 

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "582f4f8d-71e9-4a5c-a164-9d2056318d6c",
    "etag": "\"d600d3ae-0000-0200-0000-5fa99a3d0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Shopify] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用したeコマース接続の [調査方法を学ぶために、次のチュートリアルで使用できます](../../explore/ecommerce.md)。