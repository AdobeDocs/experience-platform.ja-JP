---
keywords: Experience Platform；ホーム；人気のトピック；PayPal コネクタ；paypal;Paypal
solution: Experience Platform
title: Flow Service API を使用した PayPal ベース接続の作成
type: Tutorial
description: Flow Service API を使用して PayPal をAdobe Experience Platformに接続する方法を説明します。
exl-id: 5e6ca7b4-5e2f-4706-a339-ac159e2e0938
source-git-commit: 9ca4f19f7b59f075250bce7035303e11d3f3710f
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 60%

---

# [!DNL Flow Service] API を使用した [!DNL PayPal] ベース接続の作成

>[!WARNING]
>
>[!DNL PayPal] ソースは 2025 年 6 月末に非推奨（廃止予定）になります。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL PayPal] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md):[!DNL Experience Platform] には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL PayPal] ています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL PayPal] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL PayPal] インスタンスの URL。 （デフォルト：api.sandbox.paypal.com）。 |
| `clientId` | [!DNL PayPal] アプリケーションに関連付けられたクライアント ID。 |
| `clientSecret` | [!DNL PayPal] アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL PayPal] の接続仕様 ID は `221c7626-58f6-4eec-8ee2-042b0226f03b` です。 |

基本について詳しくは、[ この PayPal ドキュメント ](https://developer.paypal.com/docs/api/overview/#get-credentials) を参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL PayPal] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL PayPal] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `auth.params.host` | [!DNL PayPal] インスタンスの URL。 |
| `auth.params.clientId` | [!DNL PayPal] インスタンスに関連付けられたクライアント ID。 |
| `auth.params.clientSecret` | [!DNL PayPal] インスタンスに関連付けられたクライアントシークレット。 |
| `connectionSpec.id` | [!DNL PayPal] 接続仕様 ID: `221c7626-58f6-4eec-8ee2-042b0226f03b`。 |

**応答**

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PayPal] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、支払いデータを Platform に取り込むデータフローの作成](../../collect/payments.md)
