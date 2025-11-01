---
title: Flow Service API を使用したSalesforce Marketing CloudとExperience Platformの接続
description: API を使用してSalesforce Marketing Cloud アカウントをExperience Platformに接続する方法について説明します。
exl-id: fbf68d3a-f8b1-4618-bd56-160cc6e3346d
source-git-commit: 16cc811a545414021b8686ae303d6112bcf6cebb
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 6%

---

# [!DNL Salesforce Marketing Cloud] API を使用した [!DNL Flow Service] のExperience Platformへの接続

>[!WARNING]
>
>[!DNL Salesforce Marketing Cloud] ソースは 2026 年 1 月に非推奨（廃止予定）になります。 新しいソースは、代替手段として今年後半にリリースされる予定です。 新しいソースがリリースされたら、2026 年 1 月末までに、新しいアカウント接続とデータフローを作成して、新しいソースに移行する計画を立てる必要があります。

このガイドでは、[!DNL Salesforce Marketing Cloud]API[[!DNL Flow Service]  を使用して &#x200B;](https://developer.adobe.com/experience-platform-apis/references/flow-service/) アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Azure Synapse Analytics] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Flow Service] ています。


### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

次の節では、[!DNL Salesforce Marketing Cloud] API を使用してに正常に接続するために必要な追加情報を示し [!DNL Flow Service] す。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Salesforce Marketing Cloud]  認証の概要 &#x200B;](../../../../connectors/marketing-automation/salesforce-marketing-cloud.md) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法については、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) に関するガイドを参照してください。

## [!DNL Salesforce Marketing Cloud] を [!DNL Azure] のExperience Platformに接続

ベース接続を作成し [!DNL Salesforce Marketing Cloud] アカウントを [!DNL Azure] 上のExperience Platformに接続する方法については、以下をお読みください。

### ベース接続の作成 {#azure-base}

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、[!DNL Salesforce Marketing Cloud] ソース統合ではサポートされていません。

**ベース接続** には、ソースシステムをAdobe Experience Platformにリンクする主要な情報が格納されます。 これには以下が含まれます。

* ソースの認証資格情報
* 接続の現在のステータス
* 一意の **ベース接続 ID**

**ベース接続 ID** を使用すると、ソースからファイルを参照して探索し、取り込む項目とそのデータタイプおよび形式を特定するのに役立ちます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを送信し、リクエストパラメーターに [!DNL Salesforce Marketing Cloud] 認証資格情報を含めます。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce Marketing Cloud] のベース接続を作成します。

+++リクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Salesforce Marketing Cloud base connection for Azure",
    "description": "Salesforce Marketing Cloud base connection for Azure",
    "auth": {
      "specName": "Client-Id-Secret Based Authentication",
      "params": {
        "host": "acme-ab12c3d4e5fg6hijk7lmnop8qrst",
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
| --- | --- |
| `auth.params.host` |  |
| `auth.params.clientId` | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL Salesforce Marketing Cloud] 接続仕様 ID: `ea1c2a08-b722-11eb-8529-0242ac130003`。 |

+++

+++応答の例を表示

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++

## Amazon Web Services上のExperience Platformへの [!DNL Salesforce Marketing Cloud] の接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL Salesforce Marketing Cloud] アカウントをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### ベース接続の作成 {#aws-base}

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Salesforce Service Cloud] がAWS上のExperience Platformに接続するためのベース接続を作成します。

+++リクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Salesforce Marketing Cloud base connection for AWS",
    "description": "Salesforce Marketing Cloud base connection for AWS",
    "auth": {
      "specName": "OAuth2 Client Credential",
      "params": {
        "subdomain": "mc563885gzs27c5t9-63k636ttgm",
        "clientId": "3a1b2c3d4e5f67890123456789abcdef",
        "clientSecret": "xxxx"
      }
    },
    "connectionSpec": {
      "id": "ea1c2a08-b722-11eb-8529-0242ac130003",
      "version": "1.0"
    }
  }'
```

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++


## データのデータフロー [!DNL Salesforce Marketing Cloud] 作成

[!DNL Salesforce Marketing Cloud] アカウントに正常に接続したので、次は [&#x200B; データフローを作成し、マーケティング自動化プロバイダーからExperience Platformにデータを取り込む &#x200B;](../../collect/marketing-automation.md) ことができます。