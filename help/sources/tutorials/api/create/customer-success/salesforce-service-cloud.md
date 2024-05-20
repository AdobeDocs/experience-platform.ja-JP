---
keywords: Experience Platform；ホーム；人気のトピック；Salesforce Service Cloud;Salesforce service cloud
solution: Experience Platform
title: Flow Service API を使用した Salesforce サービスクラウドソース接続の作成
type: Tutorial
description: Flow Service API を使用してAdobe Experience Platformを Salesforce Service Cloud に接続する方法について説明します。
exl-id: ed133bca-8e88-4c85-ae52-c3269b6bf3c9
source-git-commit: 1f13b5fcad683b4c0ede96654e35d6f0c64d9eb7
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 68%

---

# を作成 [!DNL Salesforce Service Cloud] を使用したソース接続 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Salesforce Service Cloud] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために必要な追加情報を示します [!DNL Salesforce Service Cloud] の使用 [!DNL Flow Service] API です。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Salesforce Service Cloud] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | ---|
| `environmentUrl` | の URL [!DNL Salesforce] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Salesforce Service Cloud] ユーザーアカウント。 |
| `password` | パスワード [!DNL Salesforce Service Cloud] アカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Salesforce Service Cloud] アカウント。 |
| `apiVersion` | （オプション）の REST API バージョン [!DNL Salesforce Service Cloud] 使用しているインスタンス。 このフィールドを空白のままにすると、Experience Platformでは使用可能な最新のバージョンが自動的に使用されます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Service Cloud] の接続仕様 ID は `cb66ab34-8619-49cb-96d1-39b37ede86ea` です。 |

基本について詳しくは、を参照してください。 [この Salesforce Service Cloud ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Salesforce Service Cloud] 認証資格情報をリクエストパラメーターの一部として使用します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Base connection for salesforce service cloud",
      "description": "Base connection for salesforce service cloud",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "environmentUrl": "https://acme-enterprise-3126.my.salesforce.com",
              "username": "acme-salesforce-service-cloud",
              "password": "{PASSWORD}",
              "securityToken": "{SECURITY_TOKEN}"
          }
      },
      "connectionSpec": {
          "id": "cb66ab34-8619-49cb-96d1-39b37ede86ea",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| ---| --- |
| `auth.params.environmentUrl` | の URL [!DNL Salesforce Service Cloud] インスタンス。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL Salesforce Service Cloud] アカウント。 |
| `auth.params.password` | に関連付けられたパスワード [!DNL Salesforce Service Cloud] アカウント。 |
| `auth.params.securityToken` | に関連付けられたセキュリティトークン [!DNL Salesforce Service Cloud] アカウント。 |
| `connectionSpec.id` | [!DNL Salesforce Service Cloud] 接続仕様 ID：`cb66ab34-8619-49cb-96d1-39b37ede86ea` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成された接続が応答として返されます。この ID は、次の手順で CRM システムを探索するために必要になります。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce Service Cloud] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service]  API を使用した、カスタマーサクセスデータを Platform に取り込むデータフローの作成](../../collect/customer-success.md)
