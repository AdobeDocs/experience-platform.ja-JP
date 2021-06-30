---
keywords: Experience Platform；ホーム；人気のあるトピック；PayPalコネクタ；paypal;Paypal
solution: Experience Platform
title: フローサービスAPIを使用したPayPalベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してPayPalをAdobe Experience Platformに接続する方法を説明します。
exl-id: 5e6ca7b4-5e2f-4706-a339-ac159e2e0938
source-git-commit: 5cb853da21e41b38c88f25a4989a602dbcfceabc
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 7%

---

# [!DNL Flow Service] APIを使用して[!DNL PayPal]ベース接続を作成する

>[!NOTE]
>
>[!DNL PayPal]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して[!DNL PayPal]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一のPlatformインスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL PayPal]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL PayPal]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL PayPal]インスタンスのURL。 (デフォルト：api.sandbox.paypal.com)を参照してください。 |
| `clientId` | [!DNL PayPal]アプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | [!DNL PayPal]アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PayPal]の接続仕様IDは次のとおりです。`221c7626-58f6-4eec-8ee2-042b0226f03b` |

使い始めの詳細は、[このPayPalの文書](https://developer.paypal.com/docs/api/overview/#get-credentials)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL PayPal]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL PayPal]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Paypal connection",
        "description": "Paypal connection",
        "auth": {
        "specName": "Client-Id-Secret Based Authentication",
        "params": {
            "host": "{HOST}",
            "clientId": "{CLIENT_ID}",
            "clientSecret": "{CLIENT_SECRET}"
            }
        },
        "connectionSpec": {
            "id": "221c7626-58f6-4eec-8ee2-042b0226f03b",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | [!DNL PayPal]インスタンスのURL。 |
| `auth.params.clientId` | [!DNL PayPal]インスタンスに関連付けられているクライアントID。 |
| `auth.params.clientSecret` | [!DNL PayPal]インスタンスに関連付けられているクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL PayPal]接続仕様ID:`221c7626-58f6-4eec-8ee2-042b0226f03b`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL PayPal]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/payments.md)を使用して支払い申し込みを調べる方法を学ぶ際に使用できます。[
