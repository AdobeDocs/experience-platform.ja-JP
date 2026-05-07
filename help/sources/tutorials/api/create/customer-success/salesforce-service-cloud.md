---
title: Flow Service APIを使用したSalesforce Service Cloud Source Connectionの作成
description: Flow Service APIを使用してAdobe Experience PlatformをSalesforce Service Cloudに接続する方法について説明します。
exl-id: ed133bca-8e88-4c85-ae52-c3269b6bf3c9
source-git-commit: b9a9b00114b3c1159a14b7e39484d250fa7563ba
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 35%

---

# [!DNL Flow Service] APIを使用して[!DNL Salesforce Service Cloud] ソース接続を作成します

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Salesforce Service Cloud]のベース接続を作成する方法について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むことができますが、[!DNL Experience Platform] サービスを使用して、着信データを構造化、ラベル付け、強化することができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformは、1つの[!DNL Experience Platform] インスタンスを個別のバーチャル環境に分割し、デジタルエクスペリエンスアプリケーションの開発と進化に役立つバーチャルサンドボックスを提供します。

次の節では、[!DNL Flow Service] APIを使用して[!DNL Salesforce Service Cloud]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

資格情報の取得について詳しくは、[認証ガイド ](../../../../connectors/customer-success/salesforce-service-cloud.md#credentials)を参照してください。

### Experience Platform APIの使用

Experience Platform APIの呼び出しを正常に行う方法について詳しくは、[Experience Platform APIの概要](../../../../../landing/api-guide.md)に関するガイドを参照してください。

## ベース接続の作成

ベース接続は、ソースの認証情報、接続の現在の状態、一意のベース接続IDなど、ソースとExperience Platform間の情報を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Salesforce Service Cloud] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、OAuth 2 クライアント資格情報を使用して[!DNL Salesforce Service Cloud]のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Salesforce Service Cloud account for ACME data (OAuth2)",
      "description": "Salesforce Service Cloud account for ACME data (OAuth2)",
      "auth": {
          "specName": "OAuth2 Client Credential",
          "params":
            "environmentUrl": "https://acme-enterprise-3126.my.salesforce.com",
            "clientId": "xxxx",
            "clientSecret": "xxxx",
            "apiVersion": "60.0"
        }
      },
      "connectionSpec": {
          "id": "cb66ab34-8619-49cb-96d1-39b37ede86ea",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.environmentUrl` | [!DNL Salesforce Service Cloud] インスタンスのURL。 |
| `auth.params.clientId` | [!DNL Salesforce Service Cloud] アカウントに関連付けられているクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Salesforce Service Cloud] アカウントに関連付けられているクライアントの秘密鍵。 |
| `auth.params.apiVersion` | 使用している[!DNL Salesforce Service Cloud] インスタンスのREST API バージョン。 |
| `connectionSpec.id` | [!DNL Salesforce Service Cloud]接続仕様ID: `cb66ab34-8619-49cb-96d1-39b37ede86ea`。 |

**応答**

応答が成功すると、新しく作成したベース接続とその一意のIDが返されます。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Salesforce Service Cloud] ベース接続を作成しました。 このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] APIを使用してExperience Platformにカスタマーサクセスデータを取り込むデータフローを作成します](../../collect/customer-success.md)
