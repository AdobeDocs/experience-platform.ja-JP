---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Flow Service APIを使用してMicrosoft Dynamics Source Connectionを作成する
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用して、プラットフォームをMicrosoft Dynamicsアカウントに接続する方法を説明します。
exl-id: 423c6047-f183-4d92-8d2f-cc8cc26647ef
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 27%

---

# [!DNL Flow Service] APIを使用して[!DNL Microsoft Dynamics]ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、Flow Service APIを使用してプラットフォームを[!DNL Microsoft Dynamics]（以下「Dynamics」と呼ばれる）アカウントに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してPlatformをDynamicsアカウントに正常に接続するために必要な追加情報について、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Dynamics]に接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUri` | [!DNL Dynamics]インスタンスのサービスURL。 |
| `username` | [!DNL Dynamics]ユーザーアカウントのユーザー名です。 |
| `password` | [!DNL Dynamics]アカウントのパスワード。 |
| `servicePrincipalId` | [!DNL Dynamics]アカウントのクライアントID。 このIDは、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパル秘密キー。 この秘密鍵証明書は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

開始方法の詳細については、[this [!DNL Dynamics] ドキュメント](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のデータフローの作成に使用できるため、[!DNL Dynamics]アカウントごとに必要な接続は1つだけです。

### 基本認証を使用した[!DNL Dynamics]接続の作成

基本的な認証を使用して[!DNL Dynamics]POSTを作成するには、[!DNL Flow Service] APIに接続リクエストを行い、接続の`serviceUri`、`username`、および`password`に値を指定します。

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
        "name": "Dynamics connection",
        "description": "Dynamics connection using basic auth",
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

### サービスプリンシパルキーベースの認証を使用して[!DNL Dynamics]接続を作成する

サービスプリンシパルキーベースの認証を使用して[!DNL Dynamics]POSTを作成するには、接続の`serviceUri`、`servicePrincipalId`、および`servicePrincipalKey`に値を提供しながら、[!DNL Flow Service] APIに接続リクエストを行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Dynamics connection",
        "description": "Dynamics connection using key-based authentication",
        "auth": {
            "specName": "Service Principal Key Based Authentication",
            "params": {
                "serviceUri": "{SERVICE_URI}",
                "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
                "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}"
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
| `auth.params.servicePrincipalId` | [!DNL Dynamics]アカウントのクライアントID。 このIDは、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `auth.params.servicePrincipalKey` | サービスプリンシパル秘密キー。 この秘密鍵証明書は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

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
