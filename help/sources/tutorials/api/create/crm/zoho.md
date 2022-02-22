---
keywords: Experience Platform；ホーム；人気のトピック；Zoho CRM;Zoho crm;Zoho;zoho
solution: Experience Platform
title: フローサービス API を使用した Zoho CRM ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Zoho CRM に接続する方法を説明します。
exl-id: 33995927-8f5e-44c5-b809-4db8706bbd34
source-git-commit: 46b2fd6bc715bf1d8ccfeed576a2a2d193f92edd
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 8%

---

# の作成 [!DNL Zoho CRM] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Zoho CRM] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために必要な追加情報を示します。 [!DNL Zoho CRM] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL Zoho CRM]を使用する場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `endpoint` | のエンドポイント [!DNL Zoho CRM] リクエスト先のサーバー。 |
| `accountsUrl` | アカウント URL は、アクセスおよび更新トークンの生成に使用されます。 URL は、ドメイン固有である必要があります。 |
| `clientId` | に対応するクライアント ID [!DNL Zoho CRM] ユーザーアカウント。 |
| `clientSecret` | に対応するクライアントの秘密鍵 [!DNL Zoho CRM] ユーザーアカウント。 |
| `accessToken` | アクセストークンは、 [!DNL Zoho CRM] アカウント |
| `refreshToken` | 更新トークンは、アクセストークンの有効期限が切れた後に、新しいアクセストークンの生成に使用されるトークンです。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Zoho CRM] 次に該当： `929e4450-0237-4ed2-9404-b7e1e0a00309`. |

これらの資格情報について詳しくは、 [[!DNL Zoho CRM] 認証](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Zoho CRM] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

>[!TIP]
>
>アカウント URL ドメインは、適切なドメインの場所に対応している必要があります。 様々なドメインと対応するアカウント URL を次に示します。<ul><li>米国：https://accounts.zoho.com</li><li>オーストラリア：https://accounts.zoho.com.au</li><li>ヨーロッパ：https://accounts.zoho.eu</li><li>インド：https://accounts.zoho.in</li><li>中国：https://accounts.zoho.com.cn</li></ul>

次のリクエストは、 [!DNL Zoho CRM]:

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
| `name` | お客様の [!DNL Zoho CRM] ベース接続。 この名前を使用して、 [!DNL Zoho CRM] ベース接続。 |
| `description` | （オプション） [!DNL Zoho CRM] ベース接続。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.endpoint` | のエンドポイント [!DNL Zoho CRM] リクエスト先のサーバー。 |
| `auth.params.accountsUrl` | アカウント URL は、アクセスおよび更新トークンの生成に使用されます。 URL は、ドメイン固有である必要があります。 |
| `auth.params.clientId` | に対応するクライアント ID [!DNL Zoho CRM] ユーザーアカウント。 |
| `auth.params.clientSecret` | に対応するクライアントの秘密鍵 [!DNL Zoho CRM] ユーザーアカウント。 |
| `auth.params.accessToken` | アクセストークンは、 [!DNL Zoho CRM] アカウント |
| `auth.params.refreshToken` | 更新トークンは、アクセストークンの有効期限が切れた後に、新しいアクセストークンの生成に使用されるトークンです。 |
| `connectionSpec.id` | の接続仕様 ID [!DNL Zoho CRM]: `929e4450-0237-4ed2-9404-b7e1e0a00309`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) をクリックします。 この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Zoho CRM] を使用したベース接続 [!DNL Flow Service] API を介して取得され、接続の一意の ID 値を取得している。 この ID は、次のチュートリアルで、 [フローサービス API を使用した CRM システムの調査](../../explore/crm.md).
