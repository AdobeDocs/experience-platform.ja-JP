---
keywords: Experience Platform；ホーム；人気のあるトピック；google adwords;Google AdWords;adwords
solution: Experience Platform
title: Flow Service APIを使用したGoogle AdWordsソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してAdobe Experience PlatformをGoogle AdWordsに接続する方法を説明します。
exl-id: 4658e392-1bd9-4e74-aa05-96109f9b62a0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 23%

---

# [!DNL Flow Service] APIを使用して[!DNL Google AdWords]ソース接続を作成する

>[!NOTE]
>
>[!DNL Google AdWords]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Experience Platform]を[!DNL Google AdWords]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して広告に正しく接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

[!DNL Flow Service]がAdWordsと接続するには、次の接続プロパティの値を指定する必要があります。

| **Credential** | **説明** |
| -------------- | --------------- |
| `clientCustomerId` | AdWordsアカウントのクライアント顧客ID。 |
| `developerToken` | マネージャーアカウントに関連付けられている開発者トークン。 |
| `refreshToken` | AdWordsへのアクセスを許可するために[!DNL Google]から取得した更新トークン。 |
| `clientId` | 更新トークンの取得に使用される[!DNL Google]アプリケーションのクライアントID。 |
| `clientSecret` | 更新トークンの取得に使用される[!DNL Google]アプリケーションのクライアントシークレット。 |
| `connectionSpec` | 接続を作成するために必要な一意の識別子。 [!DNL Google AdWords]の接続指定IDは次のとおりです。`d771e9c1-4f26-40dc-8617-ce58c4b53702` |

これらの値の詳細については、[Google AdWordsのドキュメント](https://developers.google.com/adwords/api/docs/guides/authentication)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Google AdWords]アカウントごとに1つの接続のみが必要です。

**API 形式**

```https
POST /connections
```

**リクエスト**

[!DNL Google AdWords]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Google AdWords]の接続指定IDは`d771e9c1-4f26-40dc-8617-ce58c4b53702`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "google-AdWords connection",
        "description": "Connection for google-AdWords",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "clientCustomerID": "{CLIENT_CUSTOMER_ID}",
                "developerToken": "{DEVELOPER_TOKEN}",
                "authenticationType": "{AUTHENTICATION_TYPE}"
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "d771e9c1-4f26-40dc-8617-ce58c4b53702",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.clientCustomerID` | [!DNL AdWords]アカウントのクライアント顧客ID。 |
| `auth.params.developerToken` | [!DNL AdWords]アカウントの開発者トークン。 |
| `auth.params.refreshToken` | [!DNL AdWords]アカウントの更新トークン。 |
| `auth.params.clientID` | [!DNL AdWords]アカウントのクライアントID。 |
| `auth.params.clientSecret` | [!DNL AdWords]アカウントのクライアントシークレット。 |
| `connectionSpec.id` | [!DNL Google AdWords]接続指定ID:`d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Google AdWords]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API](../../explore/advertising.md)を使用して[広告システムを調査する方法を学習する際に、次のチュートリアルで使用できます。
