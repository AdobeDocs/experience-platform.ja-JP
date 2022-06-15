---
keywords: Experience Platform；ホーム；人気の高いトピック；veeva crm;Veeva CRM;Veeva;
solution: Experience Platform
title: フローサービス API を使用した Veeva CRM ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Veeva CRM に接続する方法を説明します。
exl-id: e1aea5a2-a247-43eb-8252-2e2ed96b82a1
source-git-commit: ea20a850a5d83f648c699119913aa31e2ea16233
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 71%

---

# [!DNL Flow Service] API を使用した [!DNL Veeva CRM] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Veeva CRM] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Veeva CRM] に正常に接続するために必要な追加情報を示しています。

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Veeva CRM] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | の URL [!DNL Veeva CRM] インスタンス。 |
| `username` | ユーザー名の値 [!DNL Veeva CRM] アカウント |
| `password` | パスワードの値 [!DNL Veeva CRM] アカウント |
| `securityToken` | のセキュリティトークン [!DNL Veeva CRM] インスタンス。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。[!DNL Veeva CRM] の接続仕様 ID は `fcad62f3-09b0-41d3-be11-449d5a621b69` です。 |

これらの値について詳しくは、 [[!DNL Veeva CRM] 文書](https://developer.veevacrm.com/doc/Content/rest-api.htm).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Veeva CRM] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Veeva CRM] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `name` | [!DNL Veeva CRM] ベース接続名。この名前を使用して、[!DNL Veeva CRM] ベース接続を検索できます。 |
| `description` | [!DNL Veeva CRM] ベース接続のオプション説明。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.environmentUrl` | の URL [!DNL Veeva CRM] インスタンス。 |
| `auth.params.username` | ユーザー名の値 [!DNL Veeva CRM] アカウント |
| `auth.params.password` | パスワードの値 [!DNL Veeva CRM] アカウント |
| `auth.params.securityToken` | のセキュリティトークン [!DNL Veeva CRM] インスタンス。 |
| `connectionSpec.id` | [!DNL Veeva CRM] の接続仕様 ID は `fcad62f3-09b0-41d3-be11-449d5a621b69` です。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

## 次の手順

このチュートリアルに従って、 [!DNL Veeva CRM] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/crm.md)
