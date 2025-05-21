---
title: Flow Service API を使用した MariaDB のExperience Platformへの接続
description: API を使用して MariaDB アカウントをExperience Platformに接続する方法を説明します。
exl-id: 9b7ff394-ca55-4ab4-99ef-85c80b04a6df
source-git-commit: bca4f40d452f0a5e70a388872a65640d1fd58533
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 22%

---

# [!DNL Flow Service] API を使用した [!DNL MariaDB] のExperience Platformへの接続

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL MariaDB] アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL MariaDB] ています。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL MariaDB]  概要 ](../../../../connectors/databases/mariadb.md#prerequisites) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法については、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) に関するガイドを参照してください。

## [!DNL MariaDB] をExperience Platformに接続

[!DNL MariaDB] アカウントをExperience Platformに接続する方法については、以下の手順を参照してください。

### [!DNL MariaDB] のベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

**API 形式**

```https
POST /connections
```

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、[!DNL MariaDB] アカウントに適切な認証資格情報を指定します。

>[!BEGINTABS]

>[!TAB  接続文字列ベースの認証 ]

**リクエスト**

次のリクエストは、接続文字列ベースの認証を使用して、[!DNL MariaDB] ソースのベース接続を作成します。

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
    "name": "MariaDB connection",
    "description": "MariaDB connection",
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
| `auth.params.connectionString` | [!DNL MariaDB] 認証に関連付けられた接続文字列。 [!DNL MariaDB] の接続文字列パターンは `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です。 |
| `connectionSpec.id` | [!DNL MariaDB] 接続仕様 ID は `3000eb99-cd47-43f3-827c-43caf170f015` です。 |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "be3a2d71-1fb6-4fea-ba2d-711fb61fea50",
    "etag": "\"02002624-0000-0200-0000-5e41f7040000\""
}
```

+++

>[!TAB  基本認証 ]

**リクエスト**

次のリクエストは、基本認証を使用して [!DNL MariaDB] ソースのベース接続を作成します。

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
      "name": "MariaDB on Experience Platform using basic auth",
      "description": "MariaDB on Experience Platform using basic auth",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "{SERVER}",
              "database": "{DATABASE}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}",
              "sslMode": "{SSLMODE}"
          }
      },
      "connectionSpec": {
          "id": "3000eb99-cd47-43f3-827c-43caf170f015",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL MariaDB] データベースの名前または IP。 |
| `auth.params.database` | データベースの名前。 |
| `auth.params.username` | データベースに対応するユーザー名。 |
| `auth.params.password` | データベースに対応するパスワード。 |
| `auth.params.sslMode` | データ転送中にデータを暗号化する方法。 |
| `connectionSpec.id` | [!DNL MariaDB] 接続仕様 ID は `3000eb99-cd47-43f3-827c-43caf170f015` です。 |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "f847950c-1c12-4568-a550-d5312b16fdb8",
    "etag": "\"0c0099f4-0000-0200-0000-67da91710000\""
}
```

+++

>[!ENDTABS]


## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL MariaDB] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)
