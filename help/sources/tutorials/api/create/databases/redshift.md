---
title: Flow Service API を使用したAmazon Redshift ベース接続の作成
description: Flow Service API を使用してAdobe Experience PlatformをAmazon Redshift に接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 2728ce08-05c9-4dca-af1d-d2d1b266c5d9
source-git-commit: a7c2c5e4add5c80e0622d5aeb766cec950d79dbb
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 59%

---

# [!DNL Flow Service] API を使用した [!DNL Amazon Redshift] ベース接続の作成

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Amazon Redshift] ソースを利用できます。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Amazon Redshift] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Amazon Redshift] ています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Amazon Redshift] に接続するには、次の接続プロパティを指定する必要があります。

| **認証情報** | **説明** |
| -------------- | --------------- |
| `server` | [!DNL Amazon Redshift] アカウントに関連付けられたサーバー。 |
| `port` | クライアント接続をリッスンするために [!DNL Amazon Redshift] サーバーが使用する TCP ポート。 |
| `username` | [!DNL Amazon Redshift] アカウントに関連付けられたユーザー名。 |
| `password` | [!DNL Amazon Redshift] アカウントに関連付けられたパスワード。 |
| `database` | アクセスする [!DNL Amazon Redshift] データベース。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Amazon Redshift] の接続仕様 ID は `3416976c-a9ca-4bba-901a-1f08f66978ff` です。 |

基本について詳しくは、この [[!DNL Amazon Redshift]  ドキュメント ](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html) を参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

>[!NOTE]
>
>[!DNL Redshift] のデフォルトのエンコーディング規格は Unicode です。 これは変更できません。

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Amazon Redshift] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Amazon Redshift] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "amazon-redshift base connection",
      "description": "base connection for amazon-redshift,
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "{SERVER}",
              "port": "{PORT},
              "username": "{USERNAME}",
              "password": "{PASSWORD}",
              "database": "{DATABASE}"
          }
      },
      "connectionSpec": {
          "id": "3416976c-a9ca-4bba-901a-1f08f66978ff",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.server` | [!DNL Amazon Redshift] サーバー。 |
| `auth.params.port` | クライアント接続をリッスンするために [!DNL Amazon Redshift] サーバーが使用する TCP ポート。 |
| `auth.params.database` | [!DNL Amazon Redshift] アカウントに関連付けられたデータベース。 |
| `auth.params.password` | [!DNL Amazon Redshift] アカウントに関連付けられたパスワード。 |
| `auth.params.username` | [!DNL Amazon Redshift] アカウントに関連付けられたユーザー名。 |
| `connectionSpec.id` | [!DNL Amazon Redshift] 接続仕様 ID：`3416976c-a9ca-4bba-901a-1f08f66978ff` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成された接続が応答として返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "373e88fc-43da-4e3c-be88-fc43da3e3c0f",
    "etag": "\"1700ce7b-0000-0200-0000-5e3b405e0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Amazon Redshift] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
