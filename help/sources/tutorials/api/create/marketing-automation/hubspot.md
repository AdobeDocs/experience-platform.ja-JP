---
keywords: Experience Platform；ホーム；人気のあるトピック；hubspot;Hubspot
solution: Experience Platform
title: フローサービス API を使用した HubSpot ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを HubSpot に接続する方法を説明します。
exl-id: a3e64215-a82d-4aa7-8e6a-48c84c056201
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 10%

---

# [!DNL Flow Service] API を使用して [!DNL HubSpot] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL HubSpot] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL HubSpot] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL HubSpot] と接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `clientId` | [!DNL HubSpot] アプリケーションに関連付けられているクライアント ID。 |
| `clientSecret` | [!DNL HubSpot] アプリケーションに関連付けられているクライアントの秘密鍵。 |
| `accessToken` | OAuth 統合を最初に認証したときに取得されるアクセストークン。 |
| `refreshToken` | OAuth 統合を最初に認証したときに取得される更新トークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL HubSpot] の接続仕様 ID は次のとおりです。`cc6a4487-9e91-433e-a3a3-9cf6626c1806`. |

使い始める方法については、この [HubSpot のドキュメント ](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL HubSpot] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `auth.params.clientId` | [!DNL HubSpot] アプリケーションに関連付けられているクライアント ID。 |
| `auth.params.clientSecret` | [!DNL HubSpot] アプリケーションに関連付けられているクライアントの秘密鍵。 |
| `auth.params.accessToken` | OAuth 統合を最初に認証したときに取得されるアクセストークン。 |
| `auth.params.refreshToken` | OAuth 統合を最初に認証したときに取得される更新トークン。 |
| `connectionSpec.id` | [!DNL HubSpot] 接続仕様 ID:`cc6a4487-9e91-433e-a3a3-9cf6626c1806`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) が含まれます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL HubSpot] 接続を作成し、接続の一意の ID 値を取得しました。 次のチュートリアルでは、フローサービス API](../../explore/marketing-automation.md) を使用してマーケティング自動化システムを調べる方法を学ぶ際に、この接続 ID を使用できます。[
