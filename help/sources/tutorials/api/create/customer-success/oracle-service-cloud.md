---
keywords: Experience Platform；ホーム；人気の高いトピック；Oracleサービスクラウド；oracleサービスクラウド
title: フローサービス API を使用したOracleサービスクラウドソース接続の作成
description: フローサービス API を使用してAdobe Experience PlatformをOracleサービスクラウドに接続する方法を説明します。
source-git-commit: 078a266967cd7b0818f958283a58a8af4c886a21
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 33%

---

# （ベータ版） [!DNL Flow Service] API

>[!NOTE]
>
>oracleサービスクラウドソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Flow Service] API

### 必要な認証情報の収集

次のために [!DNL Flow Service] oracleサービスクラウドに接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `host` | oracleサービスクラウドインスタンスのホスト URL。 |
| `username` | Service Cloud ユーザーアカウントのOracle。 |
| `password` | Service Cloud アカウントのOracle。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 oracleサービスクラウドの接続仕様 ID は次のとおりです。 `ba5126ec-c9ac-11eb-b8bc-0242ac130003`. |

oracleサービスクラウドアカウントの認証について詳しくは、 [[!DNL Oracle] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/b2c-service/20c/cxska/OKCS_Authenticate_and_Authorize.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを使用して、Oracleサービスクラウド認証資格情報をリクエストパラメーターの一部として指定する際に使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、Oracleサービスクラウドのベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Base connection for Oracle Service Cloud",
      "description": "Base connection for Oracle Service Cloud",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "host": "{HOST}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "ba5126ec-c9ac-11eb-b8bc-0242ac130003",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.host` | oracleサービスクラウドインスタンスのホスト URL。 |
| `auth.params.username` | Service Cloud アカウントに関連付けられたOracle。 |
| `auth.params.password` | Service Cloud アカウントに関連付けられたOracle。 |
| `connectionSpec.id` | oracleサービスクラウド接続仕様 ID: `ba5126ec-c9ac-11eb-b8bc-0242ac130003` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) をクリックします。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルでは、 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/customer-success.md)
