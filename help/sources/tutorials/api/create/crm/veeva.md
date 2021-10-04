---
keywords: Experience Platform；ホーム；人気のあるトピック；veeva crm;Veeva CRM;Veeva;
solution: Experience Platform
title: フローサービス API を使用した Veeva CRM ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Veeva CRM に接続する方法を説明します。
exl-id: e1aea5a2-a247-43eb-8252-2e2ed96b82a1
source-git-commit: 89a0e2544a17fe10e6dfd7611b5223ca4fc55501
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 10%

---

# （ベータ版）[!DNL Flow Service] API を使用して [!DNL Veeva CRM] ベース接続を作成する

>[!NOTE]
>
>[!DNL Veeva CRM] ソースはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Veeva CRM] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Veeva CRM] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Veeva CRM] と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | [!DNL Veeva CRM] インスタンスの URL。 |
| `username` | [!DNL Veeva CRM] アカウントのユーザー名の値。 |
| `password` | [!DNL Veeva CRM] アカウントのパスワード値。 |
| `securityToken` | [!DNL Veeva CRM] インスタンスのセキュリティトークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Veeva CRM] の接続仕様 ID は次のとおりです。`fcad62f3-09b0-41d3-be11-449d5a621b69`. |

これらの値の詳細については、[[!DNL Veeva CRM]  ドキュメント ](https://developer.veevacrm.com/api/#order-management-rest-api) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Veeva CRM] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
| `name` | [!DNL Veeva CRM] ベース接続の名前。 この名前を使用して [!DNL Veeva CRM] ベース接続を検索できます。 |
| `description` | [!DNL Veeva CRM] ベース接続に関するオプションの説明。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.environmentUrl` | [!DNL Veeva CRM] インスタンスの URL。 |
| `auth.params.username` | [!DNL Veeva CRM] アカウントのユーザー名の値。 |
| `auth.params.password` | [!DNL Veeva CRM] アカウントのパスワード値。 |
| `auth.params.securityToken` | [!DNL Veeva CRM] インスタンスのセキュリティトークン。 |
| `connectionSpec.id` | [!DNL Veeva CRM] の接続仕様 ID:`fcad62f3-09b0-41d3-be11-449d5a621b69`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Veeva CRM] ベース接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルでフローサービス API](../../explore/crm.md) を使用して CRM システムを調べる方法を学ぶ際に使用できます。[
