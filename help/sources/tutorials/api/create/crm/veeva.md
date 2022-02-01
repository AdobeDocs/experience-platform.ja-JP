---
keywords: Experience Platform；ホーム；人気の高いトピック；veeva crm;Veeva CRM;Veeva;
solution: Experience Platform
title: フローサービス API を使用した Veeva CRM ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Veeva CRM に接続する方法を説明します。
exl-id: e1aea5a2-a247-43eb-8252-2e2ed96b82a1
source-git-commit: 25cc0c5a1e6dcf01b82956ea1022663445315a27
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 10%

---

# の作成 [!DNL Veeva CRM] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Veeva CRM] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために必要な追加情報を示します。 [!DNL Veeva CRM] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL Veeva CRM]を使用する場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | の URL [!DNL Veeva CRM] インスタンス。 |
| `username` | ユーザー名の値 [!DNL Veeva CRM] アカウント |
| `password` | パスワードの値 [!DNL Veeva CRM] アカウント |
| `securityToken` | のセキュリティトークン [!DNL Veeva CRM] インスタンス。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Veeva CRM] 次に該当： `fcad62f3-09b0-41d3-be11-449d5a621b69`. |

これらの値について詳しくは、 [[!DNL Veeva CRM] 文書](https://developer.veevacrm.com/api/#order-management-rest-api).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Veeva CRM] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Veeva CRM]:

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json'
    -d '{
        "name": "Veeva CRM base connection",
        "description": "Base Connection for Veeva CRM",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "environmentUrl": "{ENVIRONMENT_URL}",
                "username": "{USERNAME}",
                "password": "{PASSWORD}",
                "securityToken": "{SECURITY_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "fcad62f3-09b0-41d3-be11-449d5a621b69",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | お客様の [!DNL Veeva CRM] ベース接続。 この名前を使用して、 [!DNL Veeva CRM] ベース接続。 |
| `description` | （オプション） [!DNL Veeva CRM] ベース接続。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.environmentUrl` | の URL [!DNL Veeva CRM] インスタンス。 |
| `auth.params.username` | ユーザー名の値 [!DNL Veeva CRM] アカウント |
| `auth.params.password` | パスワードの値 [!DNL Veeva CRM] アカウント |
| `auth.params.securityToken` | のセキュリティトークン [!DNL Veeva CRM] インスタンス。 |
| `connectionSpec.id` | の接続仕様 ID [!DNL Veeva CRM]: `fcad62f3-09b0-41d3-be11-449d5a621b69`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) をクリックします。 この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Veeva CRM] を使用したベース接続 [!DNL Flow Service] API を介して取得され、接続の一意の ID 値を取得している。 この ID は、次のチュートリアルで、 [フローサービス API を使用した CRM システムの調査](../../explore/crm.md).
