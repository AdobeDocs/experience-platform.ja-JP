---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してHP Verticaコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 2%

---


# APIを使用してHP [!DNL Vertica][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>HP [!DNL Vertica] コネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用してHPとの接続手順を順を追って説明 [!DNL Vertica] し [!DNL Experience Platform]ます。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

- [ソース](https://docs.adobe.com/content/help/en/experience-platform/source-connectors/home.html): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用して入力データの構造、マッピング、拡張を行うことができます。
- [サンドボックス](https://docs.adobe.com/content/help/en/experience-platform/sandbox/home.html): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

APIを [!DNL Vertica][!DNL Flow Service] 使用してHPに正常に接続するために知っておく必要がある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

HPと接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があり [!DNL Vertica]ます。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | HP [!DNL Vertica] インスタンスへの接続に使用する接続文字列。 HPの接続文字列パターン [!DNL Vertica] は `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` |
| `connectionSpec.id` | 接続を作成するために必要な識別子。 HPの固定接続仕様ID [!DNL Vertica] は次のとおりです。 `a8b6a1a4-5735-42b4-952c-85dce0ac38b5` |

接続文字列の取得の詳細については、 [このHP Verticaドキュメントを参照してください](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](https://docs.adobe.com/content/help/en/experience-platform/landing/troubleshooting.html#reading-example-api-calls) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース(ソースコネクタ [!DNL Experience Platform]を含む)は、特定の仮想サンドボックスに分離されています。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

- Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるため、HP [!DNL Vertica] アカウントごとに必要な接続は1つだけです。

**API形式**

```http
POST /connections
```

**リクエスト**

HP [!DNL Vertica] 接続を作成するには、一意の接続仕様IDをPOST要求の一部として指定する必要があります。 HPの接続仕様ID [!DNL Vertica] は `a8b6a1a4-5735-42b4-952c-85dce0ac38b5`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Connection for HP Vertica",
        "description": "Connection for HP Vertica",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "a8b6a1a4-5735-42b4-952c-85dce0ac38b5",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | HP [!DNL Vertica] アカウントに関連付けられている接続文字列。 HPの接続文字列パターン [!DNL Vertica] は次のとおりです。 `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | HP [!DNL Vertica] 接続仕様ID: `a8b6a1a4-5735-42b4-952c-85dce0ac38b5`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

## 次の手順

このチュートリアルに従うと、 [!DNL Vertica][!DNL Flow Service] APIを使用してHP接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
