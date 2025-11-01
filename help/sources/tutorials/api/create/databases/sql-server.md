---
title: Flow Service API を使用したMicrosoft SQL Server ベース接続の作成
type: Tutorial
description: Flow Service API を使用してAdobe Experience PlatformをMicrosoft SQL Server に接続する方法について説明します。
exl-id: 00455a61-c8c1-42f4-a962-fc16f7370cbd
source-git-commit: 16cc811a545414021b8686ae303d6112bcf6cebb
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 39%

---

# [!DNL Microsoft] API を使用した [!DNL Flow Service] SQL Server ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

[!DNL Microsoft SQL Server]API[[!DNL Flow Service]  を使用して ](https://www.adobe.io/experience-platform-apis/references/flow-service/) のベース接続を作成する方法については、このチュートリアルをお読みください。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Microsoft SQL Server] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Flow Service] ています。

### 必要な資格情報の収集 {#gather-required-credentials}

を [!DNL Microsoft SQL Server] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `connectionString` | [!DNL Microsoft SQL Server] アカウントに関連付けられた接続文字列。 接続文字列のパターンは、データソースにサーバー名とインスタンス名のどちらを使用しているかによって異なります。<ul><li>サーバー名 `Data Source={SERVER_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` を使用した接続文字列</li><li>インスタンス名を使用した接続文字列：`Data Source={INSTANCE_NAME};Initial Catalog={DATABASE};Integrated Security=False;User ID={USER_ID};Password={PASSWORD};` | `Data Source=mssqlserver.database.windows.net;Initial Catalog=mssqlserver_e2e_db;Integrated Security=False;User ID=mssqluser;Password=mssqlpassword` |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Microsoft SQL Server] の接続仕様 ID は `1f372ff9-38a4-4492-96f5-b9a4e4bd00ec` です。 |  |

接続文字列の取得について詳しくは、この [[!DNL Microsoft SQL Server]  ドキュメント ](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

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
| `auth.params.connectionString` | [!DNL Microsoft SQL Server] アカウントに関連付けられた接続文字列。 詳しくは、[ 必要な資格情報の収集 ](#gather-required-credentials) の節を参照してください。 |
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
* [ [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)