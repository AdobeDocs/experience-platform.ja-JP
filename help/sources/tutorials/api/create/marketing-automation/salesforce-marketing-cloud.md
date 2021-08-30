---
keywords: Experience Platform；ホーム；人気のあるトピック；salesforce marketing cloud;SalesforceMarketing Cloud
solution: Experience Platform
title: フローサービスAPIを使用したSalesforceMarketing Cloudベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをSalesforceMarketing Cloudに接続する方法を説明します。
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL Salesforce Marketing Cloud]ベース接続を作成する

>[!NOTE]
>
>[!DNL Salesforce Marketing Cloud]ソースはベータ版です。 ベータラベル付きのソースの使用について詳しくは、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Salesforce Marketing Cloud]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]：Experience は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

次の節では、[!DNL Flow Service] APIを使用して[!DNL Salesforce Marketing Cloud]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Salesforce Marketing Cloud]と接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 |
| `clientId` | [!DNL Salesforce Marketing Cloud]アプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | [!DNL Salesforce Marketing Cloud]アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Salesforce Marketing Cloud]の接続仕様IDは次のとおりです。`cea1c2a08-b722-11eb-8529-0242ac130003`. |

使い始める方法については、この[[!DNL Salesforce Marketing Cloud] ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエスト本文の一部として[!DNL Salesforce Marketing Cloud]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce Marketing Cloud]のベース接続を作成します。

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
            "id": "cea1c2a08-b722-11eb-8529-0242ac130003",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | [!DNL Salesforce Marketing Cloud]アプリケーションに関連付けられているクライアントID。 |
| `auth.params.clientSecret` | [!DNL Salesforce Marketing Cloud]アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL Salesforce Marketing Cloud]接続仕様ID:`cea1c2a08-b722-11eb-8529-0242ac130003`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Salesforce Marketing Cloud]接続を作成し、接続の一意のID値を取得しました。 次のチュートリアルでは、フローサービスAPI](../../explore/marketing-automation.md)を使用してマーケティング自動化システムを調べる方法を学ぶ際に、この接続IDを使用できます。[
