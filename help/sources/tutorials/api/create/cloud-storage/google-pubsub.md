---
keywords: Experience Platform；ホーム；人気のあるトピック；Google PubSub;google pubsub
solution: Experience Platform
title: Flow Service APIを使用したGoogle PubSubソース接続の作成
topic: 概要
type: チュートリアル
description: Flow Service APIを使用して、Adobe Experience PlatformをGoogle PubSubアカウントに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: 0af90253f04377149986aedf2e9d3012ca06d4f8
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 36%

---


# Flow Service APIを使用した[!DNL Google PubSub]ソース接続の作成

>[!NOTE]
>
>[!DNL Google PubSub]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して、[!DNL Google PubSub]（以下「[!DNL PubSub]」と呼ばれる）をAdobe Experience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL PubSub]ソース接続を正しく作成するために知っておく必要がある追加情報を紹介します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL PubSub]に接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `projectId` | [!DNL PubSub]の認証に必要なプロジェクトID。 |
| `credentials` | [!DNL PubSub]の認証に必要な秘密鍵またはキーです。 |

これらの値について詳しくは、次の[PubSub authentication](https://cloud.google.com/pubsub/docs/authentication)ドキュメントを参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のデータフローの作成に使用できるため、[!DNL PubSub]アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL PubSub]接続を作成するには、POST要求の一部としてプロバイダーIDと接続指定IDを指定する必要があります。 プロバイダーIDは`521eee4d-8cbe-4906-bb48-fb6bd4450033`で、接続指定IDは`70116022-a743-464a-bbfe-e226a7f8210c`です。

**API 形式**

```http
POST /connections
```

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google PubSub connection",
        "description": "Google PubSub connection",
        "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
        "auth": {
            "specName": "Google PubSub authentication credentials",
            "params": {
                "projectId": "{PROJECT_ID}",
                "credentials": "{CREDENTIALS}"
            }
        },
        "connectionSpec": {
            "id": "70116022-a743-464a-bbfe-e226a7f8210c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.projectId` | [!DNL PubSub]の認証に必要なプロジェクトID。 |
| `auth.params.credentials` | [!DNL PubSub]の認証に必要な秘密鍵またはキーです。 |
| `connectionSpec.id` | [!DNL PubSub]接続仕様ID:`70116022-a743-464a-bbfe-e226a7f8210c`. |

**応答** 

正常に応答すると、新たに作成された[!DNL PubSub]接続の接続IDが返されます。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL PubSub]接続を作成し、一意の接続IDを取得します。 この接続IDを使用して[Flow Service API](../../collect/streaming.md)を使用してストリーミングデータを収集できます。
