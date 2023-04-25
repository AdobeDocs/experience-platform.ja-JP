---
keywords: Experience Platform;ホーム;人気の高いトピック;Oracle Service Cloud;oracle service cloud
title: Flow Service API を使用した Oracle Service Cloud ソース接続の作成
description: Flow Service API を使用して Adobe Experience Platform を Oracle Service Cloud に接続する方法について説明します。
exl-id: 00c0bc9c-a740-4bab-a882-2cfed8abe758
source-git-commit: 1695b7d638feb648d5cd7af07879f3ed13f938eb
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 98%

---

# を使用してOracleサービスクラウドのソース接続を作成する [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して Oracle Service Cloud のベース接続を作成する手順について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して Oracle Service Cloud に正しく接続するために必要な追加情報を示します。

### 必要な認証情報の収集

[!DNL Flow Service] を Oracle Service Cloud に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | Oracle Service Cloud インスタンスのホスト URL。 |
| `username` | Oracle Service Cloud ユーザーアカウントのユーザー名。 |
| `password` | Oracle Service Cloud アカウントのパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。Oracle Service Cloud の接続仕様 ID は `ba5126ec-c9ac-11eb-b8bc-0242ac130003` です。 |

Oracle Service Cloud アカウントの認証について詳しくは、[[!DNL Oracle] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/b2c-service/20c/cxska/OKCS_Authenticate_and_Authorize.html)を参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に Oracle Service Cloud 認証資格情報をリクエストパラメーターの一部として指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストでは、Oracle Service Cloud のベース接続を作成しています。

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
| `auth.params.host` | Oracle Service Cloud インスタンスのホスト URL。 |
| `auth.params.username` | Oracle Service Cloud アカウントに関連付けられたユーザー名。 |
| `auth.params.password` | Oracle Service Cloud アカウントに関連付けられたパスワード。 |
| `connectionSpec.id` | Oracle Service Cloud 接続仕様 ID：`ba5126ec-c9ac-11eb-b8bc-0242ac130003` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成された接続が応答として返されます。この ID は、次の手順で CRM システムを探索するために必要になります。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して Oracle Service Cloud ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service]  API を使用した、カスタマーサクセスデータを Platform に取り込むデータフローの作成](../../collect/customer-success.md)
