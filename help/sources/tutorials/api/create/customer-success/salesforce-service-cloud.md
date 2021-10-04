---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce Service Cloud;Salesforce サービスクラウド
solution: Experience Platform
title: フローサービス API を使用した Salesforce サービスクラウドソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Salesforce Service Cloud に接続する方法を説明します。
exl-id: ed133bca-8e88-4c85-ae52-c3269b6bf3c9
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 11%

---

# [!DNL Flow Service] API を使用して [!DNL Salesforce Service Cloud] ソース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Salesforce Service Cloud] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Salesforce Service Cloud] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Salesforce Service Cloud] と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `username` | [!DNL Salesforce Service Cloud] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Salesforce Service Cloud] アカウントのパスワード。 |
| `securityToken` | [!DNL Salesforce Service Cloud] アカウントのセキュリティトークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Salesforce Service Cloud] の接続仕様 ID は次のとおりです。`b66ab34-8619-49cb-96d1-39b37ede86ea`. |

使い始める方法について詳しくは、[ この Salesforce Service Cloud ドキュメント ](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Salesforce Service Cloud] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce Service Cloud] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Base connection for salesforce service cloud",
        "description": "Base connection for salesforce service cloud",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "username": "{USERNAME}",
                "password": "{PASSWORD}",
                "securityToken": "{SECURITY_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "b66ab34-8619-49cb-96d1-39b37ede86ea",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.username` | [!DNL Salesforce Service Cloud] アカウントに関連付けられているユーザー名。 |
| `auth.params.password` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたパスワード。 |
| `auth.params.securityToken` | [!DNL Salesforce Service Cloud] アカウントに関連付けられたセキュリティトークン。 |
| `connectionSpec.id` | [!DNL Salesforce Service Cloud] 接続仕様 ID:`b66ab34-8619-49cb-96d1-39b37ede86ea` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) が含まれます。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce Service Cloud] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID は、次のチュートリアルでフローサービス API](../../explore/customer-success.md) を使用して顧客の成功システムを調べる方法を学ぶ際に使用できます。[
