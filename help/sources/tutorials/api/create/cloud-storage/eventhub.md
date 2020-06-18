---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してAzureイベントハブコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 855f543a1cef394d121502f03471a60b97eae256
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 2%

---


# Flow Service APIを使用してAzureイベントハブコネクタを作成する

>[!NOTE]
> Azureイベントハブコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集および一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをAzureイベントハブアカウントに接続する手順を順を追って説明します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

- [ソース](../../../../home.md): Experience Platformを使用すると、Platformサービスを使用して、様々なソースからデータを取り込み、データの構造、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してAzureイベントハブアカウントに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

Flow ServiceがAzureイベントハブアカウントに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 認証規則の名前。SASキー名とも呼ばれます。 |
| `sasKey` | 生成された共有アクセス署名です。 |
| `namespace` | アクセスしているイベントハブの名前空間。 |
| `connectionSpec.id` | Azureイベントハブ接続仕様ID: `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |

これらの値の詳細については、 [次の「イベントハブのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)」を参照してください。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

- Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるAzureイベントハブのアカウントに必要な接続は1つだけです。

**API形式**

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
        "name": "Azure Event Hubs connection",
        "description": "Connector for Azure Event Hubs",
        "auth": {
            "specName": "Basic Authentication for Event Hubs",
            "params": {
                "sasKeyName": "sasKeyName",
                "sasKey": "sasKey",
                "namespace": "namespace"
            }
        },
        "connectionSpec": {
            "id": "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.sasKeyName` | 認証規則の名前。SASキー名とも呼ばれます。 |
| `auth.params.sasKey` | 生成された共有アクセス署名です。 |
| `namespace` | アクセスしているイベントハブの名前空間。 |
| `connectionSpec.id` | Azureイベントハブ接続仕様ID: `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従って、APIを使用してAzureイベントハブ接続を作成し、応答本文の一部として一意のIDを取得しました。 この接続IDを使用して、Flow Service APIを使用してクラウドストレージを [調査できます](../../explore/cloud-storage.md)。