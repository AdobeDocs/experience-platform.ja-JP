---
keywords: Experience Platform；ホーム；人気のあるトピック；MariaDB;mariadb
solution: Experience Platform
title: フローサービスAPIを使用したMariaDBベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをMariaDBに接続する方法を説明します。
exl-id: 9b7ff394-ca55-4ab4-99ef-85c80b04a6df
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 11%

---

# [!DNL Flow Service] APIを使用して[!DNL MariaDB]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL MariaDB]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL MariaDB]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL MariaDB]と接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL MariaDB]認証に関連付けられた接続文字列。 [!DNL MariaDB]接続文字列パターンは次のとおりです。`Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL MariaDB]の接続仕様IDは`3000eb99-cd47-43f3-827c-43caf170f015`です。 |

接続文字列の取得について詳しくは、この[[!DNL MariaDB] ドキュメント](https://mariadb.com/kb/en/about-mariadb-connector-odbc/)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL MariaDB]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL MariaDB]のベース接続を作成します。

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
| `auth.params.connectionString` | [!DNL MariaDB]認証に関連付けられた接続文字列。 [!DNL MariaDB]接続文字列パターンは次のとおりです。`Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | [!DNL MariaDB]接続仕様IDは次のとおりです。`3000eb99-cd47-43f3-827c-43caf170f015`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次の手順でデータベースを調べるために必要です。

```json
{
    "id": "be3a2d71-1fb6-4fea-ba2d-711fb61fea50",
    "etag": "\"02002624-0000-0200-0000-5e41f7040000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL MariaDB]接続を作成し、接続の一意のID値を取得しました。 次のチュートリアルでは、フローサービスAPI](../../explore/database-nosql.md)を使用してデータベースやNoSQLシステムを調べる方法を学ぶ際に、この接続IDを使用できます。[
