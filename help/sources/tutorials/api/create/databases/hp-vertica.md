---
keywords: Experience Platform；ホーム；人気のあるトピック；Vertica;vertica
solution: Experience Platform
title: Flow Service APIを使用したHP Verticaソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してHP VerticaをAdobe Experience Platformに接続する方法を説明します。
exl-id: 37f831c1-7c82-462a-8338-a0bcaaf08cd1
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 27%

---

# [!DNL Flow Service] APIを使用してHP [!DNL Vertica]ソース接続を作成する

>[!NOTE]
>
>HP [!DNL Vertica]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、HP [!DNL Vertica]を[!DNL Experience Platform]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](https://docs.adobe.com/content/help/en/experience-platform/source-connectors/home.html): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用して受信データの構造、マッピング、拡張を行うことができます。
* [サンドボックス](https://docs.adobe.com/content/help/ja-JP/experience-platform/sandbox/home.html): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

次の節では、[!DNL Flow Service] APIを使用してHP [!DNL Vertica]に正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]がHP [!DNL Vertica]と接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | HP [!DNL Vertica]インスタンスへの接続に使用する接続文字列。 HP [!DNL Vertica]の接続文字列パターンは`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`です |
| `connectionSpec.id` | 接続を作成するために必要な識別子。 HP [!DNL Vertica]の固定接続仕様IDは次のとおりです。`a8b6a1a4-5735-42b4-952c-85dce0ac38b5` |

接続文字列の取得に関する詳細は、[このHP Verticaドキュメント](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm)を参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 HP [!DNL Vertica]アカウントごとに1つの接続のみが必要です。異なるデータを取り込むために複数のソースコネクタを作成するのに使用できます。

**API 形式**

```http
POST /connections
```

**リクエスト**

HP [!DNL Vertica]接続を作成するには、POST要求の一部として一意の接続仕様IDを指定する必要があります。 HP [!DNL Vertica]の接続仕様IDは`a8b6a1a4-5735-42b4-952c-85dce0ac38b5`です。

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
| `auth.params.connectionString` | HP [!DNL Vertica]アカウントに関連付けられている接続文字列。 HP [!DNL Vertica]の接続文字列パターンは次のとおりです。`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | HP [!DNL Vertica]接続仕様ID:`a8b6a1a4-5735-42b4-952c-85dce0ac38b5`. |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用してHP [!DNL Vertica]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API ](../../explore/database-nosql.md)を使用して[データベースを調査する方法を学習する際に、次のチュートリアルで使用できます。
