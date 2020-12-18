---
keywords: Experience Platform;home;popular topics;Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Flow Service APIを使用してMicrosoft Dynamics Connectorを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、CRMデータを収集するためのMicrosoft Dynamics（以下「Dynamics」と呼ばれる）アカウントにプラットフォームを接続する手順を説明します。
translation-type: tm+mt
source-git-commit: 9092c3d672967d3f6f7bf7116c40466a42e6e7b1
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 20%

---


# [!DNL Flow Service] APIを使用して[!DNL Microsoft Dynamics]コネクタを作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Platform]を[!DNL Microsoft Dynamics] （以下「Dynamics」と呼びます）アカウントに接続してCRMデータを収集する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):デジタルエクスペリエンスアプリケーションの開発と発展に役立つ、単一の[!DNL xperience Platform] インスタンスを個別の仮想環境に分割する仮想サンドボックスを [!DNL Platform] 提供します。

[!DNL Flow Service] APIを使用して[!DNL Platform]をDynamicsアカウントに正常に接続するために知っておく必要がある追加情報について、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Dynamics]に接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUri` | [!DNL Dynamics]インスタンスのサービスURL。 |
| `username` | [!DNL Dynamics]ユーザーアカウントのユーザー名です。 |
| `password` | [!DNL Dynamics]アカウントのパスワード。 |

開始方法の詳細については、[このDynamicsドキュメント](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](../../../../../tutorials/authentication.md)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Dynamics]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Dynamics]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Dynamics]の接続指定IDは`38ad80fe-8b06-4938-94f4-d4ee80266b07`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Dynamics Connection",
        "description": "connection for Dynamics account",
        "auth": {
            "specName": "Basic Authentication for Dynamics-Online",
            "params": {
                "serviceUri": "{SERVICE_URI}",
                "username": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUri` | [!DNL Dynamics]インスタンスに関連付けられたサービスURI。 |
| `auth.params.username` | [!DNL Dynamics]アカウントに関連付けられているユーザー名。 |
| `auth.params.password` | [!DNL Dynamics]アカウントに関連付けられているパスワードです。 |
| `connectionSpec.id` | 前の手順で取得した[!DNL Dynamics]アカウントの接続仕様`id`。 |

**応答** 

正常に応答すると、新たに作成された接続が、一意の識別子(`id`)を含めて返されます。 このIDは、次の手順でCRMシステムを調査するために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Dynamics]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API](../../explore/crm.md)を使用して[CRMシステムを調べる方法を学習する際に、次のチュートリアルで使用できます。