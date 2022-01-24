---
keywords: Experience Platform；ホーム；人気の高いトピック；salesforce marketing cloud;SalesforceMarketing Cloud
solution: Experience Platform
title: フローサービス API を使用した SalesforceMarketing Cloudベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを SalesforceMarketing Cloudに接続する方法を説明します。
exl-id: fbf68d3a-f8b1-4618-bd56-160cc6e3346d
source-git-commit: 531d5619e0643b6195abaa53d1708e0368d45871
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 10%

---

# の作成 [!DNL Salesforce Marketing Cloud] を使用したベース接続 [!DNL Flow Service] API

>[!NOTE]
>
>この [!DNL Salesforce Marketing Cloud] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Salesforce Marketing Cloud] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]：Experience は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

次の節では、に正常に接続するために必要な追加情報を示します。 [!DNL Salesforce Marketing Cloud] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL Salesforce Marketing Cloud]に値を入力する場合は、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **注意：** を `host` の値を指定する場合、URL 全体ではなくサブドメインのみを指定する必要があります。 例えば、ホスト URL が `https://abcd-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/`を指定した場合は、 `abcd-ab12c3d4e5fg6hijk7lmnop8qrst` をホスト値として使用します。 |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Salesforce Marketing Cloud] 次に該当： `ea1c2a08-b722-11eb-8529-0242ac130003`. |

導入の詳細については、 [[!DNL Salesforce Marketing Cloud] 文書](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm).

## ベース接続を作成する

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Salesforce Marketing Cloud] 認証資格情報をリクエスト本文の一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Salesforce Marketing Cloud]:

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Salesforce Marketing Cloud base connection",
        "description": "Salesforce Marketing Cloud base connection",
        "auth": {
            "specName": "Client-Id-Secret Based Authentication",
            "params": {
                "host": "{HOST}"
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}"
            }
        },
        "connectionSpec": {
            "id": "ea1c2a08-b722-11eb-8529-0242ac130003",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | 次に関連付けられたクライアント ID: [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `connectionSpec.id` | この [!DNL Salesforce Marketing Cloud] 接続仕様 ID: `ea1c2a08-b722-11eb-8529-0242ac130003`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルに従って、 [!DNL Salesforce Marketing Cloud] を使用した接続 [!DNL Flow Service] API で、接続の一意の ID 値を取得している。 次のチュートリアルでは、この接続 ID を使用して、次の方法を学習できます [フローサービス API を使用したマーケティング自動化システムの調査](../../explore/marketing-automation.md).
