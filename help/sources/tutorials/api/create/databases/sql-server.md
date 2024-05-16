---
title: Flow Service API を使用したMicrosoft SQL Server ベース接続の作成
type: Tutorial
description: Flow Service API を使用してAdobe Experience PlatformをMicrosoft SQL Server に接続する方法について説明します。
exl-id: 00455a61-c8c1-42f4-a962-fc16f7370cbd
source-git-commit: 1828dd76e9ff317f97e9651331df3e49e44efff5
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 61%

---

# を作成 [!DNL Microsoft] を使用した SQL Server ベース接続 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

のベース接続を作成する方法については、このチュートリアルをお読みください [!DNL Microsoft SQL Server] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために必要な追加情報を示します [!DNL Microsoft SQL Server] の使用 [!DNL Flow Service] API です。

### 必要な資格情報の収集 {#gather-required-credentials}

に接続するには [!DNL Microsoft SQL Server]には、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `connectionString` | に関連付けられた接続文字列 [!DNL Microsoft SQL Server] アカウント。 接続文字列のパターンは、データソースにサーバー名とインスタンス名のどちらを使用しているかによって異なります。<ul><li>サーバー名を使用した接続文字列： `Data Source={SERVER_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};`</li><li>インスタンス名を使用した接続文字列：`Data Source={INSTANCE_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` | `Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword` |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Microsoft SQL Server] 等しい `1f372ff9-38a4-4492-96f5-b9a4e4bd00ec`. |

接続文字列の取得について詳しくは、こちらを参照してください [[!DNL Microsoft SQL Server] 文書](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Microsoft SQL Server] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Microsoft SQL Server] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Base connection for sql-server",
      "description": "Base connection for sql-server",
      "auth": {
          "specName": "Connection String Based Authentication",
          "params": {
              "connectionString": "Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword"
          }
      },
      "connectionSpec": {
          "id": "1f372ff9-38a4-4492-96f5-b9a4e4bd00ec",
          "version": "1.0"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.connectionString` | に関連付けられた接続文字列 [!DNL Microsoft SQL Server] アカウント。 の節を読む [必要な資格情報の収集](#gather-required-credentials) を参照してください。 |
| `connectionSpec.id` | [!DNL Microsoft SQL Server] 接続仕様 ID は `1f372ff9-38a4-4492-96f5-b9a4e4bd00ec` です。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータベースを調べるために必要です。

```json
{
    "id": "0b8224e4-0de8-4293-8224-e40de80293c6",
    "etag": "\"5802c519-0000-0200-0000-5e4d89520000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Microsoft SQL Server] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [を使用した、データベースデータを Platform に取り込むデータフローの作成 [!DNL Flow Service] API](../../collect/database-nosql.md)