---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft SQL;microsoft sql;sql server;SQL server
solution: Experience Platform
title: フローサービス API を使用した SQL Server ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをMicrosoft SQL Server に接続する方法を説明します。
exl-id: 00455a61-c8c1-42f4-a962-fc16f7370cbd
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 11%

---

# [!DNL Flow Service] API を使用して [!DNL Microsoft] SQL Server ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Microsoft SQL Server] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Microsoft SQL Server] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Microsoft SQL Server] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Microsoft SQL Server] アカウントに関連付けられた接続文字列。 [!DNL Microsoft SQL Server] 接続文字列パターンは次のとおりです。`Data Source={SERVER_NAME}\\<{INSTANCE_NAME} if using named instance>;Initial Catalog={DATABASE};Integrated Security=False;User ID={USERNAME};Password={PASSWORD};`. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Microsoft SQL Server] の接続仕様 ID は `1f372ff9-38a4-4492-96f5-b9a4e4bd00ec` です。 |

接続文字列の取得について詳しくは、[[!DNL Microsoft SQL Server]  ドキュメント ](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Microsoft SQL Server] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Base connection for sql-server",
        "description": "Base connection for sql-server",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Data Source={SERVER_NAME}\\<{INSTANCE_NAME} if using named instance>;Initial Catalog={DATABASE};Integrated Security=False;User ID={USERNAME};Password={PASSWORD};"
            }
        },
        "connectionSpec": {
            "id": "1f372ff9-38a4-4492-96f5-b9a4e4bd00ec",
            "version": "1.0"
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL Microsoft SQL Server] アカウントに関連付けられた接続文字列。 [!DNL Microsoft SQL Server] 接続文字列パターンは次のとおりです。`Data Source={SERVER_NAME}\\<{INSTANCE_NAME} if using named instance>;Initial Catalog={DATABASE};Integrated Security=False;User ID={USERNAME};Password={PASSWORD};`. |
| `connectionSpec.id` | [!DNL Microsoft SQL Server] 接続仕様 ID は次のとおりです。`1f372ff9-38a4-4492-96f5-b9a4e4bd00ec`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次のチュートリアルでデータベースを調べるために必要です。

```json
{
    "id": "0b8224e4-0de8-4293-8224-e40de80293c6",
    "etag": "\"5802c519-0000-0200-0000-5e4d89520000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Microsoft SQL Server] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID は、次のチュートリアルで [ フローサービス API](../../explore/database-nosql.md) を使用してデータベースや NoSQL システムを調べる方法を学ぶ際に使用できます。
