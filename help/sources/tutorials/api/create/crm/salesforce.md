---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce;Salesforce
solution: Experience Platform
title: フローサービスAPIを使用したSalesforceベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをSalesforceアカウントに接続する方法を説明します。
exl-id: 43dd9ee5-4b87-4c8a-ac76-01b83c1226f6
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL Salesforce]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Salesforce]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Platform]を[!DNL Salesforce]アカウントに正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Salesforce]に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | [!DNL Salesforce]ソースインスタンスのURL。 |
| `username` | [!DNL Salesforce]ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Salesforce]ユーザーアカウントのパスワード。 |
| `securityToken` | [!DNL Salesforce]ユーザーアカウントのセキュリティトークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL AdWords]の接続仕様IDは次のとおりです。`cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`. |

開始方法の詳細については、[このSalesforceドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Salesforce]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。


**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `auth.params.username` | [!DNL Salesforce]アカウントに関連付けられているユーザー名。 |
| `auth.params.password` | [!DNL Salesforce]アカウントに関連付けられているパスワード。 |
| `auth.params.securityToken` | [!DNL Salesforce]アカウントに関連付けられたセキュリティトークン。 |
| `connectionSpec.id` | [!DNL Salesforce]接続仕様ID:`cfc0fee1-7dc0-40ef-b73e-d8b134c436f5`. |

**応答**

正常な応答は、新しく作成された接続を、一意の識別子(`id`)を含めて返します。 このIDは、次の手順でCRMシステムを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Salesforce]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/crm.md)を使用してCRMシステムを調べる方法を学ぶ際に使用できます。[
