---
keywords: Experience Platform；ホーム；人気の高いトピック；Salesforce;Salesforce
solution: Experience Platform
title: フローサービス API を使用した Salesforce ベース接続の作成
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Salesforce アカウントに接続する方法を説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 71%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Salesforce] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、接続を成功させるために知っておく必要がある追加情報を示します [!DNL Platform] から [!DNL Salesforce] アカウントを [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Salesforce] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | の URL [!DNL Salesforce] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Salesforce] ユーザーアカウント。 |
| `password` | のパスワード [!DNL Salesforce] ユーザーアカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Salesforce] ユーザーアカウント。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL AdWords] の接続仕様 ID は `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5` です。 |

使い始める方法について詳しくは、 [この Salesforce ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Salesforce] 認証資格情報をリクエストパラメーターの一部として使用します。


**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Salesforce Connection",
        "description": "Connection for Salesforce account",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "username": "{USERNAME}",
                "password": "{PASSWORD}",
                "securityToken": "{SECURITY_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.username` | ユーザー名 [!DNL Salesforce] アカウント |
| `auth.params.password` | ユーザーに関連付けられたパスワード [!DNL Salesforce] アカウント |
| `auth.params.securityToken` | に関連付けられたセキュリティトークン [!DNL Salesforce] アカウント |
| `connectionSpec.id` | この [!DNL Salesforce] 接続仕様 ID: `cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成された接続が応答として返されます。この ID は、次の手順で CRM システムを探索するために必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/crm.md)
