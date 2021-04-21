---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce;salesforce
solution: Experience Platform
title: フローサービスAPIを使用したSalesforceソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience PlatformをSalesforceアカウントに接続する方法を説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 26%

---

# [!DNL Flow Service] APIを使用して[!DNL Salesforce]ソース接続を作成する

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元管理するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、Flow Service APIを使用して[!DNL Platform]を[!DNL Salesforce]アカウントに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL Platform]を[!DNL Salesforce]アカウントに正しく接続するために知っておく必要がある追加情報について、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Salesforce]に接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `environmentUrl` | [!DNL Salesforce]ソースインスタンスのURL。 |
| `username` | [!DNL Salesforce]ユーザーアカウントのユーザー名です。 |
| `password` | [!DNL Salesforce]ユーザーアカウントのパスワードです。 |
| `securityToken` | [!DNL Salesforce]ユーザーアカウントのセキュリティトークン。 |

開始方法の詳細については、[このSalesforceドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm)を参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Salesforce]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Salesforce]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Salesforce]の接続指定IDは`cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Salesforce Connection",
        "description": "Connection for Salesforce account",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "username": "{USERNAME}",
                "password": "{PASSWORD}",
                "securityToken": "{SECURITY_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.username` | [!DNL Salesforce]アカウントに関連付けられているユーザー名。 |
| `auth.params.password` | [!DNL Salesforce]アカウントに関連付けられているパスワードです。 |
| `auth.params.securityToken` | [!DNL Salesforce]アカウントに関連付けられているセキュリティトークン。 |
| `connectionSpec.id` | 前の手順で取得した[!DNL Salesforce]アカウントの接続仕様`id`。 |

**応答**

正常に応答すると、新たに作成された接続が、一意の識別子(`id`)を含めて返されます。 このIDは、次の手順でCRMシステムを調査するために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Salesforce]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API](../../explore/crm.md)を使用して[CRMシステムを調べる方法を学習する際に、次のチュートリアルで使用できます。
