---
keywords: Experience Platform；ホーム；人気のあるトピック；Shopify;shopify;eコマース
solution: Experience Platform
title: フローサービスAPIを使用したShopifyコネクタのベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してShopifyをAdobe Experience Platformに接続する方法を説明します。
exl-id: 36086c7f-813e-4fc5-9778-f9d55aba03b2
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 6%

---

# [!DNL Flow Service] APIを使用して[!DNL Shopify]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Shopify]（以下「[!DNL Shopify]」と呼びます）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [[!DNL Sources]](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [[!DNL Sandboxes]](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一のインスタンスを別々の仮想環境に分割 [!DNL Platform] し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Shopify]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Shopify]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Shopify]サーバーのエンドポイント。 |
| `accessToken` | [!DNL Shopify]ユーザーアカウントのアクセストークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Shopify]の接続仕様IDは次のとおりです。`4f63aa36-bd48-4e33-bb83-49fbcd11c708`. |

開始方法について詳しくは、[Shopify認証ドキュメント](https://shopify.dev/concepts/about-apis/authentication)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Shopify]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Shopify]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Shopify source",
        "description": "Shopify source",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "host": "{HOST}",
                "accessToken": "{ACCESS_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "4f63aa36-bd48-4e33-bb83-49fbcd11c708",
            "version": "1.0"
        }
    }
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | [!DNL Shopify]サーバーのエンドポイント。 |
| `auth.params.accessToken` | [!DNL Shopify]ユーザーアカウントのアクセストークン。 |
| `connectionSpec.id` | [!DNL Shopify]接続仕様ID:`4f63aa36-bd48-4e33-bb83-49fbcd11c708`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "582f4f8d-71e9-4a5c-a164-9d2056318d6c",
    "etag": "\"d600d3ae-0000-0200-0000-5fa99a3d0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Shopify]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/ecommerce.md)を使用してeコマース接続を調べる方法を学ぶ際に使用できます。[
