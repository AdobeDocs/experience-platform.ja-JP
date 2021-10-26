---
keywords: エクスペリエンス Platform、home、人気のある話題。Zoho CRM; Zoho crm;Zoho;zoho
solution: Experience Platform
title: フローサービス API を使用した Zoho CRM ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Adobe エクスペリエンスプラットフォームを Zoho CRM に接続する方法について説明します。
source-git-commit: 7a15090d8ed2c1016d7dc4d7d3d0656640c4785c
workflow-type: tm+mt
source-wordcount: '666'
ht-degree: 8%

---

# 版[!DNL Zoho CRM]API を使用したベース接続の作成 [!DNL Flow Service]

>[!NOTE]
>
>[!DNL Zoho CRM]ソースはベータ版です。[ ](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用について詳しくは、ソースの概要を参照してください。

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、API を使用するための基本的な接続を作成する手順について説明し [!DNL Zoho CRM] [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース ](../../../../home.md) : [!DNL Experience Platform] 多種多様なソースからのデータの ingested を可能にするとともに、サービスを使用した受信データを構造化、ラベル付け、拡張するための機能を提供し [!DNL Platform] ます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の各セクションでは、API を使用して正常に接続するために必要な追加情報を示し [!DNL Zoho CRM] [!DNL Flow Service] ます。

### 必要な資格情報の収集

に [!DNL Flow Service] 接続するには [!DNL Zoho CRM] 、次の接続プロパティの値を指定する必要があります。

| Chap | 説明 |
| --- | --- |
| `endpoint` | [!DNL Zoho CRM]要求を出しているサーバーのエンドポイントを指定します。 |
| `accountsUrl` | Accounts URL は、アクセスおよび更新トークンを生成するために使用されます。 URL は、ドメイン固有である必要があります。 |
| `clientId` | ユーザーアカウントに対応しているクライアント ID [!DNL Zoho CRM] 。 |
| `clientSecret` | このクライアントシークレットには、 [!DNL Zoho CRM] ユーザーアカウントが含まれています。 |
| `accessToken` | アクセストークンによって、アカウントに対するセキュリティで保護された安全なアクセスが認証され [!DNL Zoho CRM] ます。 |
| `refreshToken` | 更新トークンは、アクセストークンの有効期限が切れると、新しいアクセストークンを生成するために使用されるトークンです。 |
| `connectionSpec.id` | コネクション仕様は、ベースおよびソース接続の作成に関連付けられた認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Zoho CRM] は、次のとおりです `929e4450-0237-4ed2-9404-b7e1e0a00309` 。 |

これらの資格情報について詳しくは、認証に関するドキュメントを参照してください [[!DNL Zoho CRM]  ](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html) 。

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、そのエンドポイントに POST 要求を行います。この場合は、 `/connections` [!DNL Zoho CRM] 要求パラメーターの一部として認証資格情報を指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

>[!TIP]
>
>アカウント URL ドメインは、適切なドメインの場所に対応している必要があります。 次の例は、各ドメインとそれに対応するアカウント Url を示しています。<ul><li>日本: https://accounts.zoho.com</li><li>オーストラリア: https://accounts.zoho.com.au</li><li>ヨーロッパ: https://accounts.zoho.eu</li><li>インド: https://accounts.zoho.in</li><li>中国: https://accounts.zoho.com.cn</li></ul>

次の要求によって、次のような基本的な接続が作成され [!DNL Zoho CRM] ます。

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
| `name` | ベース接続の名前を指定 [!DNL Zoho CRM] します。 この名前を使用して、基本的な接続を検索することができ [!DNL Zoho CRM] ます。 |
| `description` | 基本的な接続に関する説明を指定します (オプション) [!DNL Zoho CRM] 。 |
| `auth.specName` | 接続に使用される認証の種類です。 |
| `auth.params.endpoint` | [!DNL Zoho CRM]要求を出しているサーバーのエンドポイントを指定します。 |
| `auth.params.accountsUrl` | Accounts URL は、アクセスおよび更新トークンを生成するために使用されます。 URL は、ドメイン固有である必要があります。 |
| `auth.params.clientId` | ユーザーアカウントに対応しているクライアント ID [!DNL Zoho CRM] 。 |
| `auth.params.clientSecret` | このクライアントシークレットには、 [!DNL Zoho CRM] ユーザーアカウントが含まれています。 |
| `auth.params.accessToken` | アクセストークンによって、アカウントに対するセキュリティで保護された安全なアクセスが認証され [!DNL Zoho CRM] ます。 |
| `auth.params.refreshToken` | 更新トークンは、アクセストークンの有効期限が切れると、新しいアクセストークンを生成するために使用されるトークンです。 |
| `connectionSpec.id` | の接続仕様 ID [!DNL Zoho CRM] `929e4450-0237-4ed2-9404-b7e1e0a00309` 。. |

**応答**

応答が成功した場合は、一意の識別子 () を含む、新しく作成されたベース接続の詳細が返され `id` ます。 この ID は、次の手順に従ってソース接続を作成する必要があります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルでは、 [!DNL Zoho CRM] API を使用した基本的な接続を作成 [!DNL Flow Service] し、接続の一意の ID 値を取得しました。 この ID は、 [ フローサービス API を使用して CRM システムを検索する方法について学習するために、次のチュートリアルで使用でき ](../../explore/crm.md) ます。
