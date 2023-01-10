---
keywords: Experience Platform；ホーム；人気のトピック；hubspot;Hubspot
solution: Experience Platform
title: フローサービス API を使用した HubSpot ベース接続の作成
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを HubSpot に接続する方法を説明します。
exl-id: a3e64215-a82d-4aa7-8e6a-48c84c056201
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 60%

---

# [!DNL Flow Service] API を使用した [!DNL HubSpot] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL HubSpot] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL HubSpot] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL HubSpot]に値を入力する場合は、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL HubSpot] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL HubSpot] アプリケーション。 |
| `accessToken` | OAuth 統合を最初に認証したときに取得されたアクセストークン。 |
| `refreshToken` | OAuth 統合の初回認証時に取得された更新トークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL HubSpot] の接続仕様 ID は `cc6a4487-9e91-433e-a3a3-9cf6626c1806` です。 |

導入の詳細については、 [HubSpot ドキュメント](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL HubSpot] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL HubSpot] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "connection for HubSpot",
        "description": "connection for HubSpot",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "accessToken": "{ACCESS_TOKEN}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "cc6a4487-9e91-433e-a3a3-9cf6626c1806",
            "version": "1.0"
        }
    }
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | 次に関連付けられたクライアント ID: [!DNL HubSpot] アプリケーション。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL HubSpot] アプリケーション。 |
| `auth.params.accessToken` | OAuth 統合を最初に認証したときに取得されたアクセストークン。 |
| `auth.params.refreshToken` | OAuth 統合の初回認証時に取得された更新トークン。 |
| `connectionSpec.id` | この [!DNL HubSpot] 接続仕様 ID: `cc6a4487-9e91-433e-a3a3-9cf6626c1806`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL HubSpot] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service]  API を使用した、マーケティング自動化データを Platform に取り込むデータフローの作成](../../collect/marketing-automation.md)
