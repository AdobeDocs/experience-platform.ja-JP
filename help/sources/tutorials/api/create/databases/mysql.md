---
title: Flow Service API を使用した MySQL のExperience Platformへの接続
description: API を使用して MySQL データベースをExperience Platformに接続する方法について説明します。
exl-id: 273da568-84ed-4a3d-bfea-0f5b33f1551a
source-git-commit: b73ced639100c95f6c62be92d4796a206a688958
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 7%

---

# [!DNL Flow Service] API を使用した [!DNL MySQL] のExperience Platformへの接続

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL MySQL] アカウントをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL MySQL] ています。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL MySQL]  概要 ](../../../../connectors/databases/mysql.md#prerequisites) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法については、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) に関するガイドを参照してください。

## [!DNL MySQL] を Azure 上のExperience Platformに接続 {#azure}

[!DNL MySQL] アカウントを Azure 上のExperience Platformに接続する方法については、以下の手順を参照してください。

### Azure 上のExperience Platformに [!DNL MySQL] のベース接続を作成する {#azure-base}

ベース接続は、ソースをExperience Platformにリンクし、認証の詳細、接続ステータス、一意の ID を保存します。 この ID を使用して、ソースファイルを参照し、データのタイプや形式など、取り込む特定の項目を特定します。

**API 形式**

```https
POST /connections
```

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、リクエストパラメーターの一部として [!DNL MySQL] 認証資格情報を指定します。

>[!BEGINTABS]

>[!TAB  接続文字列ベースの認証 ]

**リクエスト**

次のリクエストは、接続文字列ベースの認証を使用して、[!DNL MySQL] のベース接続を作成します。

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
      "name": "MySQL Base Connection to Experience Platform",
      "description": "Via Connection String,
      "auth": {
          "specName": "Connection String Based Authentication",
          "params": {
              "connectionString": "Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "26d738e0-8963-47ea-aadf-c60de735468a",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.connectionString` | アカウントに関連付けられた [!DNL MySQL] の接続文字列。 [!DNL MySQL] の接続文字列パターンは `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です。 |
| `connectionSpec.id` | [!DNL MySQL] 接続仕様 ID: `26d738e0-8963-47ea-aadf-c60de735468a`。 |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "1a444165-3439-4c16-8441-653439dc166a",
    "etag": "\"5b04c219-0000-0200-0000-5e179c8f0000\""
}
```

+++

>[!TAB  基本認証 ]

**リクエスト**

次のリクエストは、基本認証を使用して [!DNL MySQL] ソースのベース接続を作成します。

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
      "name": "MySQL Base Connection to Experience Platform",
      "description": "Via Basic Authentication",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "localhost",
              "port": "443",
              "database": "mysql-acme",
              "username": "acme",
              "password": "xxxx",
              "sslMode": "DISABLED"
          }
      },
      "connectionSpec": {
          "id": "26d738e0-8963-47ea-aadf-c60de735468a",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL MySQL] データベースの名前または IP。 |
| `auth.params.database` | データベースの名前。 |
| `auth.params.username` | データベースに対応するユーザー名。 |
| `auth.params.password` | データベースに対応するパスワード。 |
| `auth.params.sslMode` | データ転送中にデータを暗号化する方法。 |
| `connectionSpec.id` | [!DNL MySQL] 接続仕様 ID は `26d738e0-8963-47ea-aadf-c60de735468a` です。 |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答の例を表示

```json
{
    "id": "025d4158-4113-403b-b551-e81724d3880c",
    "etag": "\"ae004437-0000-0200-0000-67ee107e0000\""
}
```

+++

>[!ENDTABS]

## Amazon Web Services上のExperience Platformへの [!DNL MySQL] の接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL MySQL] アカウントをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### AWS上のExperience Platformに [!DNL MySQL] のベース接続を作成する {#aws-base}

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL MySQL] がAWS上のExperience Platformに接続するためのベース接続を作成します。

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
      "name": "MySQL on Experience Platform AWS",
      "description": "MySQL on Experience Platform AWS",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "localhost",
              "port": "443",
              "database": "mysql-acme",
              "username": "acme",
              "password": "xxxx",
              "sslMode": "false"
          }
      },
      "connectionSpec": {
          "id": "26d738e0-8963-47ea-aadf-c60de735468a",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL MySQL] データベースの名前または IP。 |
| `auth.params.database` | データベースの名前。 |
| `auth.params.username` | データベースに対応するユーザー名。 |
| `auth.params.password` | データベースに対応するパスワード。 |
| `auth.params.sslMode` | サーバーサポートに応じて、SSL を適用するかどうかを制御するブール値。 この設定のデフォルトは `false` です。 |
| `connectionSpec.id` | [!DNL MySQL] 接続仕様 ID は `26d738e0-8963-47ea-aadf-c60de735468a` です。 |

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

## データのデータフロー [!DNL MySQL] 作成

[!DNL MySQL] データベースに正常に接続できたので、[ データフローを作成し、データベースからExperience Platformにデータを取り込む ](../../collect/database-nosql.md) ことができます。