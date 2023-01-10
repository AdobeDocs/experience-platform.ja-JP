---
keywords: Experience Platform；ホーム；人気のトピック；PayPal コネクタ；paypal;Paypal
solution: Experience Platform
title: フローサービス API を使用した PayPal ベース接続の作成
type: Tutorial
description: フローサービス API を使用して PayPal をAdobe Experience Platformに接続する方法を説明します。
exl-id: 5e6ca7b4-5e2f-4706-a339-ac159e2e0938
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 58%

---

# [!DNL Flow Service] API を使用した [!DNL PayPal] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL PayPal] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL PayPal] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL PayPal] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | の URL [!DNL PayPal] インスタンス。 ( デフォルト：api.sandbox.paypal.com) を参照してください。 |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL PayPal] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL PayPal] アプリケーション。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL PayPal] 次に該当： `221c7626-58f6-4eec-8ee2-042b0226f03b` |

の導入について詳しくは、 [この PayPal ドキュメント](https://developer.paypal.com/docs/api/overview/#get-credentials).

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
| `auth.params.host` | の URL [!DNL PayPal] インスタンス。 |
| `auth.params.clientId` | 次に関連付けられたクライアント ID: [!DNL PayPal] インスタンス。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL PayPal] インスタンス。 |
| `connectionSpec.id` | この [!DNL PayPal] 接続仕様 ID: `221c7626-58f6-4eec-8ee2-042b0226f03b`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL PayPal] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/payments.md)
