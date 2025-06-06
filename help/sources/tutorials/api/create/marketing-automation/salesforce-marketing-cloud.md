---
title: Flow Service API を使用したSalesforce Marketing Cloud ベース接続の作成
description: Flow Service API を使用して、Experience Platformに対してSalesforce Marketing Cloud アカウントを認証する方法を説明します。
exl-id: fbf68d3a-f8b1-4618-bd56-160cc6e3346d
source-git-commit: 7ff0709b62590bb80c1ed664368f28cdc4a950ea
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 31%

---

# [!DNL Flow Service] API を使用した [!DNL Salesforce Marketing Cloud] ベース接続の作成

>[!WARNING]
>
>[!DNL Salesforce Marketing Cloud] ソースは 2026 年 1 月に非推奨（廃止予定）になります。 新しいソースは、代替手段として今年後半にリリースされる予定です。 新しいソースがリリースされたら、2026 年 1 月末までに、新しいアカウント接続とデータフローを作成して、新しいソースに移行する計画を立てる必要があります。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>) を使用して、[!DNL Salesforce Marketing Cloud] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示し [!DNL Salesforce Marketing Cloud] す。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Salesforce Marketing Cloud] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **メモ：**`host` 値を入力する場合は、`{subdomain}.rest.marketingcloudapis.com` を指定する必要があります。 例えば、ホスト URL が `https://acme-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/` の場合、ホスト値として `acme-ab12c3d4e5fg6hijk7lmnop8qrst.rest.marketingcloudapis.com/` を入力する必要があります。 |
| `clientId` | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント ID。 |
| `clientSecret` | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Salesforce Marketing Cloud] の接続仕様 ID は `ea1c2a08-b722-11eb-8529-0242ac130003` です。 |

基本について詳しくは、この [[!DNL Salesforce Marketing Cloud]  ドキュメント ](<https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/authentication.htm>) を参照してください。

## ベース接続の作成

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、[!DNL Salesforce Marketing Cloud] ソース統合ではサポートされていません。

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Salesforce Marketing Cloud] 認証資格情報をリクエスト本文の一部として指定します。

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
| `auth.params.clientId` | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL Salesforce Marketing Cloud] 接続仕様 ID: `ea1c2a08-b722-11eb-8529-0242ac130003`。 |

**応答**

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce Marketing Cloud] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、マーケティング自動化データをExperience Platformに取り込むデータフローの作成](../../collect/marketing-automation.md)
