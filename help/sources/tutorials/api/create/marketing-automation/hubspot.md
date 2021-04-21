---
keywords: Experience Platform；ホーム；人気のあるトピック；ハブスポット；ハブスポット
solution: Experience Platform
title: Flow Service APIを使用したHubSpotソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してAdobe Experience PlatformをHubSpotに接続する方法を説明します。
exl-id: a3e64215-a82d-4aa7-8e6a-48c84c056201
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 24%

---

# [!DNL Flow Service] APIを使用して[!DNL HubSpot]ソース接続を作成する

>[!NOTE]
>
>[!DNL HubSpot]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Experience Platform]を[!DNL HubSpot]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL HubSpot]に正しく接続するために知っておく必要のある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL HubSpot]と接続するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientId` | [!DNL HubSpot]アプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | [!DNL HubSpot]アプリケーションに関連付けられているクライアントシークレット。 |
| `accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |
| `connectionSpec` | 接続を作成するために必要な一意の識別子。 [!DNL HubSpot]の接続指定IDは次のとおりです。`cc6a4487-9e91-433e-a3a3-9cf6626c1806` |

開始方法の詳細については、この[HubSpotドキュメント](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL HubSpot]アカウントごとに1つの接続のみが必要です。

**API 形式**

```https
POST /connections
```

**リクエスト**

[!DNL HubSpot]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL HubSpot]の接続指定IDは`cc6a4487-9e91-433e-a3a3-9cf6626c1806`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "connection for hubspot",
        "description": "connection for hubspot",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "accessToken": "{ACCESS_TOKEN}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "cc6a4487-9e91-433e-a3a3-9cf6626c1806",
            "version": "1.0"
        }
    }
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | [!DNL HubSpot]アプリケーションに関連付けられているクライアントID。 |
| `auth.params.clientSecret` | [!DNL HubSpot]アプリケーションに関連付けられているクライアントシークレット。 |
| `auth.params.accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `auth.params.refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

**応答**

正常に応答すると、新たに作成された接続が返されます。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL HubSpot]接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、Flow Service API ](../../explore/marketing-automation.md)を使用して[マーケティング自動化システムを調査する方法を学ぶ際に、次のチュートリアルで使用できます。
