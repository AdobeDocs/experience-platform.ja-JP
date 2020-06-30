---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してGoogle AdWordsコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 11431ffcfc2204931fe3e863bfadc7878a40b49c
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 2%

---


# APIを使用して [!DNL Google AdWords][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>コネクタ [!DNL Google AdWords] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順を順を追って説明 [!DNL Experience Platform] し [!DNL Google AdWords]ます。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Flow Service] APIを使用して広告に正しく接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

AdWordsと接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があります。

| **Credential** | **説明** |
| -------------- | --------------- |
| 顧客ID | AdWordsアカウントのクライアント顧客ID。 |
| 開発者トークン | マネージャーアカウントに関連付けられている開発者トークン。 |
| 更新トークン | AdWordsへのアクセスを許可す [!DNL Google] るために取得した更新トークン。 |
| クライアントID | 更新トークンの取得に使用される [!DNL Google] アプリケーションのクライアントID。 |
| クライアントシークレット | 更新トークンの取得に使用される [!DNL Google] アプリケーションのクライアントシークレット。 |
| 接続指定ID | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Google AdWords] は次のとおりです。 `d771e9c1-4f26-40dc-8617-ce58c4b53702` |

これらの値の詳細については、この [Google AdWordsドキュメントを参照してください](https://developers.google.com/adwords/api/docs/guides/authentication)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Google AdWords] アカウントごとに必要な接続は1つだけです。

**API形式**

```https
POST /connections
```

**リクエスト**

次の要求は、ペイロードで提供されるプロパティによって設定された、新しいAdWords接続を作成します。


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
| `auth.params.clientCustomerID` | アカウントのクライアント顧客ID [!DNL AdWords] 。 |
| `auth.params.developerToken` | アカウントの開発者トー [!DNL AdWords] クン。 |
| `auth.params.refreshToken` | アカウントの更新トークン [!DNL AdWords] 。 |
| `auth.params.clientID` | アカウントのクライアントID [!DNL AdWords] 。 |
| `auth.params.clientSecret` | アカウントのクライアントシークレット [!DNL AdWords] 。 |
| `connectionSpec.id` | 接続 [!DNL Google AdWords] 指定ID: `d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Google AdWords] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用して広告システムを [調査する方法を学ぶ際に、次のチュートリアルで使用できます](../../explore/advertising.md)。
