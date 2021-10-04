---
keywords: Experience Platform；ホーム；人気のあるトピック；MariaDB;mariadb
solution: Experience Platform
title: フローサービス API を使用した MariaDB ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを MariaDB に接続する方法を説明します。
exl-id: 9b7ff394-ca55-4ab4-99ef-85c80b04a6df
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 12%

---

# [!DNL Flow Service] API を使用して [!DNL MariaDB] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL MariaDB] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL MariaDB] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL MariaDB] と接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL MariaDB] 認証に関連付けられた接続文字列。 [!DNL MariaDB] 接続文字列パターンは次のとおりです。`Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL MariaDB] の接続仕様 ID は `3000eb99-cd47-43f3-827c-43caf170f015` です。 |

接続文字列の取得について詳しくは、[[!DNL MariaDB]  ドキュメント ](https://mariadb.com/kb/en/about-mariadb-connector-odbc/) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL MariaDB] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL MariaDB] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Test connection for maria-db",
        "description": "Test connection for maria-db",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "3000eb99-cd47-43f3-827c-43caf170f015",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | [!DNL MariaDB] 認証に関連付けられた接続文字列。 [!DNL MariaDB] 接続文字列パターンは次のとおりです。`Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | [!DNL MariaDB] 接続仕様 ID は次のとおりです。`3000eb99-cd47-43f3-827c-43caf170f015`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でデータベースを調べるために必要です。

```json
{
    "id": "be3a2d71-1fb6-4fea-ba2d-711fb61fea50",
    "etag": "\"02002624-0000-0200-0000-5e41f7040000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL MariaDB] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID は、次のチュートリアルで [ フローサービス API](../../explore/database-nosql.md) を使用してデータベースや NoSQL システムを調べる方法を学ぶ際に使用できます。
