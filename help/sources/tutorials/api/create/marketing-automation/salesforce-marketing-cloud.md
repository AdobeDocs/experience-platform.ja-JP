---
title: フローサービス API を使用した SalesforceMarketing Cloudベース接続の作成
description: フローサービス API を使用して、SalesforceMarketing CloudアカウントをExperience Platformに対して認証する方法を説明します。
exl-id: fbf68d3a-f8b1-4618-bd56-160cc6e3346d
source-git-commit: 997a9dc70145a8cfd5d6da20ba788a4610e5c257
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 57%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce Marketing Cloud] ベース接続の作成

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、 [!DNL Salesforce Marketing Cloud] ソースの統合。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>) を使用して、[!DNL Salesforce Marketing Cloud] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

次の節では、に正常に接続するために必要な追加情報を示します。 [!DNL Salesforce Marketing Cloud] の使用 [!DNL Flow Service] API.

### 必要な資格情報の収集

次の条件を満たすため [!DNL Flow Service] ～と繋がる [!DNL Salesforce Marketing Cloud]に値を入力する場合は、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **注意：** 次の項目に `host` の値を指定する場合、URL 全体ではなくサブドメインのみを指定する必要があります。 例えば、ホスト URL が `https://acme-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/`を指定した場合は、 `acme-ab12c3d4e5fg6hijk7lmnop8qrst` をホスト値として使用します。 |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Marketing Cloud] の接続仕様 ID は `ea1c2a08-b722-11eb-8529-0242ac130003` です。 |

導入の詳細については、以下を参照してください。 [[!DNL Salesforce Marketing Cloud] 文書](<https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm>).

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Salesforce Marketing Cloud] 認証資格情報をリクエスト本文の一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce Marketing Cloud] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Salesforce Marketing Cloud base connection",
      "description": "Salesforce Marketing Cloud base connection",
      "auth": {
          "specName": "Client-Id-Secret Based Authentication",
          "params": {
              "host": "acme-ab12c3d4e5fg6hijk7lmnop8qrst"
              "clientId": "acme-salesforce-marketing-cloud",
              "clientSecret": "xxxx"
          }
      },
      "connectionSpec": {
          "id": "ea1c2a08-b722-11eb-8529-0242ac130003",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | 次に関連付けられたクライアント ID: [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Salesforce Marketing Cloud] アプリケーション。 |
| `connectionSpec.id` | The [!DNL Salesforce Marketing Cloud] 接続仕様 ID: `ea1c2a08-b722-11eb-8529-0242ac130003`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce Marketing Cloud] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service]  API を使用した、マーケティング自動化データを Platform に取り込むデータフローの作成](../../collect/marketing-automation.md)
