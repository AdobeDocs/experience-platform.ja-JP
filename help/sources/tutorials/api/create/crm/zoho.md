---
keywords: Experience Platform;ホーム;人気のトピック;Zoho CRM;Zoho crm;Zoho;zoho
solution: Experience Platform
title: Flow Service API を使用した Zoho CRM ベース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service API を使用して Adobe Experience Platform を Zoho CRM に接続する方法を説明します。
exl-id: 33995927-8f5e-44c5-b809-4db8706bbd34
source-git-commit: 17055f76800deadacf435970a691cec79c9f1d17
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 92%

---

# [!DNL Flow Service] API を使用した [!DNL Zoho CRM] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Zoho CRM] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Zoho CRM] に正常に接続するために必要な追加情報を示しています。

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Zoho CRM] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| `endpoint` | リクエスト先の [!DNL Zoho CRM] サーバーのエンドポイント。 |
| `accountsUrl` | アカウント URL は、アクセスおよび更新トークンの生成に使用されます。URL は、ドメイン固有である必要があります。 |
| `clientId` | [!DNL Zoho CRM] ユーザーアカウントに対応するクライアント ID。 |
| `clientSecret` | [!DNL Zoho CRM] ユーザーアカウントに対応するクライアントの秘密鍵。 |
| `accessToken` | アクセストークンは、[!DNL Zoho CRM] アカウントへの安全かつ一時的なアクセスを許可します。 |
| `refreshToken` | 更新トークンは、アクセストークンの有効期限が切れた後に、新しいアクセストークンの生成に使用されるトークンです。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。[!DNL Zoho CRM] の接続仕様 ID は `929e4450-0237-4ed2-9404-b7e1e0a00309` です。 |

これらの資格情報について詳しくは、[[!DNL Zoho CRM] 認証](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html)に関するドキュメントを参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Zoho CRM] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

>[!TIP]
>
>アカウント URL のドメインは、適切なドメインの地域に対応している必要があります。様々なドメインと対応するアカウント URL を次に示します。<ul><li>米国：https://accounts.zoho.com</li><li>オーストラリア：https://accounts.zoho.com.au</li><li>ヨーロッパ：https://accounts.zoho.eu</li><li>インド：https://accounts.zoho.in</li><li>中国：https://accounts.zoho.com.cn</li></ul>

次のリクエストは、[!DNL Zoho CRM] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json'
    -d '{
        "name": "Zoho CRM base connection",
        "description": "Base Connection for Zoho CRM",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "endpoint": "{ENDPOINT}",
                "accountsUrl": "{ACCOUNTS_URL}",
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "accessToken": "{ACCESS_TOKEN}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "929e4450-0237-4ed2-9404-b7e1e0a00309",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | [!DNL Zoho CRM] ベース接続名。この名前を使用して、[!DNL Zoho CRM] ベース接続を検索できます。 |
| `description` | [!DNL Zoho CRM] ベース接続のオプション説明。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.endpoint` | リクエスト先の [!DNL Zoho CRM] サーバーのエンドポイント。 |
| `auth.params.accountsUrl` | アカウント URL は、アクセストークンと更新トークンの生成に使用されます。URL は、ドメイン固有である必要があります。 |
| `auth.params.clientId` | [!DNL Zoho CRM] ユーザーアカウントに対応するクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Zoho CRM] ユーザーアカウントに対応するクライアントの秘密鍵。 |
| `auth.params.accessToken` | アクセストークンは、[!DNL Zoho CRM] アカウントへの安全かつ一時的なアクセスを許可します。 |
| `auth.params.refreshToken` | 更新トークンは、アクセストークンの有効期限が切れた後に、新しいアクセストークンの生成に使用されるトークンです。 |
| `connectionSpec.id` | [!DNL Zoho CRM] の接続仕様 ID は `929e4450-0237-4ed2-9404-b7e1e0a00309` です。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Zoho] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/crm.md)
