---
keywords: Experience Platform;home;popular topics;Oracle;oracle
solution: Experience Platform
title: Flow Service APIを使用してOracleコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、OracleをExperience Platformに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 21%

---


# APIを使用した [!DNL Oracle][!DNL Flow Service] コネクタの作成

>[!NOTE]
>
>コネクタ [!DNL Oracle] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順を順を追って説明 [!DNL Oracle] し [!DNL Experience Platform]ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to [!DNL Oracle] using the [!DNL Flow Service] API.

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | 接続に使用する接続文字列 [!DNL Oracle]。 接続文字 [!DNL Oracle] 列パターンは次のとおりです。 `Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Oracle] はです `d6b52d86-f0f8-475f-89d4-ce54c8527328`。 |

使い始めについて詳しくは、 [このOracleドキュメントを参照してください](https://docs.oracle.com/database/121/ODPNT/featConnecting.htm#ODPNT199)。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Oracle] アカウントごとに1つのコネクタが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL Oracle] 一意の接続指定IDをPOST要求の一部として指定する必要があります。 の接続指定ID [!DNL Oracle] はです `d6b52d86-f0f8-475f-89d4-ce54c8527328`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Oracle connection",
        "description": "A connection for Oracle",
        "auth": {
            "specName": "ConnectionString",
            "params": {
                    "connectionString": "Host={HOST};Port={PORT};Sid={SID};UserId={USERNAME};Password={PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "d6b52d86-f0f8-475f-89d4-ce54c8527328",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | データベースへの接続に使用する接続文字列 [!DNL Oracle] です。 接続文字 [!DNL Oracle] 列パターンは次のとおりです。 `Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | 接続 [!DNL Oracle] 指定ID: `d6b52d86-f0f8-475f-89d4-ce54c8527328`. |

**応答** 

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Oracle] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
