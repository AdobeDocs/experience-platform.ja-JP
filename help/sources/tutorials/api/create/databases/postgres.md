---
title: Flow Service API を使用した PostgreSQL のExperience Platformへの接続
description: API を使用してデータベースをExperience Platform [!DNL PostgreSQL]  接続する方法について説明します。
exl-id: 5225368a-08c1-421d-aec2-d50ad09ae454
source-git-commit: 5348158f6de9fea1a9fe186a14409afb7e7a376e
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 19%

---

# [!DNL Flow Service] API を使用した [!DNL PostgreSQL] ベース接続の作成

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL PostgreSQL] データベースをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL PostgreSQL] ています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法については、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) に関するガイドを参照してください。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL PostgreSQL]  概要 ](../../../../connectors/databases/postgres.md) を参照してください。

### 接続文字列の SSL 暗号化を有効にする

次のプロパティを使用して接続文字列を追加することで、[!DNL PostgreSQL] 接続文字列の SSL 暗号化を有効にできます。

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `EncryptionMethod` | [!DNL PostgreSQL] データに対して SSL 暗号化を有効にできます。 | <uL><li>`EncryptionMethod=0` （無効）</li><li>`EncryptionMethod=1` （有効）</li><li>`EncryptionMethod=6` （RequestSSL）</li></ul> |
| `ValidateServerCertificate` | `EncryptionMethod` ータの適用時に [!DNL PostgreSQL] データベースから送信された証明書を検証します。 | <uL><li>`ValidationServerCertificate=0` （無効）</li><li>`ValidationServerCertificate=1` （有効）</li></ul> |

次に、SSL 暗号化が追加された [!DNL PostgreSQL] 接続文字列の例を示します。`Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD};EncryptionMethod=1;ValidateServerCertificate=1`

## [!DNL PostgreSQL] を Azure 上のExperience Platformに接続 {#azure}

[!DNL PostgreSQL] アカウントを Azure 上のExperience Platformに接続する方法については、以下の手順を参照してください。

### ベース接続の作成 {#azure-base}

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL PostgreSQL] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

>[!BEGINTABS]

>[!TAB  アカウントキーベースの認証 ]

**リクエスト**

次のリクエストは、アカウントキーベースの認証を使用して、[!DNL PostgreSQL] のベース接続を作成します。

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
      "name": "PostgreSQL base connection",
      "description": "PostgreSQL base connection via connection string",
      "auth": {
          "specName": "Connection String Based Authentication",
          "params": {
              "connectionString": "Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "74a1c565-4e59-48d7-9d67-7c03b8a13137",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.connectionString` | [!DNL PostgreSQL] アカウントに関連付けられた接続文字列。 [!DNL PostgreSQL] の接続文字列パターンは `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}` です。 |
| `connectionSpec.id` | [!DNL PostgreSQL] 接続仕様 ID:`74a1c565-4e59-48d7-9d67-7c03b8a13137`。 |

+++

**応答**

リクエストが成功した場合は、新しく作成したベース接続の一意の ID （`id`）が返されます。

+++応答の例を表示

```json
{
    "id": "056dd1b4-da33-42f9-add1-b4da3392f94e",
    "etag": "\"1700e582-0000-0200-0000-5e3c85180000\""
}
```

+++

>[!TAB  基本認証 ]

**リクエスト**

次のリクエストは、基本認証を使用して [!DNL PostgreSQL] のベース接続を作成します。

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
      "name": "PostgreSQL base connection",
      "description": "PostgreSQL base connection via basic authentication",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "{SERVER}",
              "port": "{PORT}",
              "database": "{DATABASE}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}",
              "sslMode": "{SSL_MODE}"
          }
      },
      "connectionSpec": {
          "id": "74a1c565-4e59-48d7-9d67-7c03b8a13137",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| ---| --- |
| `auth.params.server` | [!DNL PostgreSQL] データベースの名前または IP アドレス。 |
| `auth.params.port` | データベースサーバーのポート番号。 |
| `auth.params.database` | [!DNL PostgreSQL] データベースの名前。 |
| `auth.params.username` | [!DNL PostgreSQL] データベース認証に関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL PostgreSQL] データベース認証に関連付けられたパスワード。 |
| `auth.params.sslMode` | データ転送中にデータを暗号化する方法。 使用可能な値は `Disable`、`Allow`、`Prefer`、`Verify Ca`、`Verify Full` などです。 |
| `connectionSpec.id` | [!DNL PostgreSQL] 接続仕様 ID:`74a1c565-4e59-48d7-9d67-7c03b8a13137`。 |

+++

**応答**

リクエストが成功した場合は、新しく作成したベース接続の一意の ID （`id`）が返されます。

+++応答の例を表示

```json
{
    "id": "2c15b1c5-73bf-47ab-9098-0467fcd854d9",
    "etag": "\"2600fc39-0000-0200-0000-67dd48f80000\""
}
```

+++

>[!ENDTABS]

## Amazon Web Services上のExperience Platformへの [!DNL PostgreSQL] の接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL PostgreSQL] データベースをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### ベース接続の作成 {#aws-base}

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL PostgreSQL] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL PostgreSQL] がAWS上のExperience Platformに接続するためのベース接続を作成します。

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
      "name": "PostgreSQL base connection",
      "description": "PostgreSQL base connection via basic authentication",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "{SERVER}",
              "port": "{PORT}",
              "database": "{DATABASE}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}",
              "sslMode": "{SSL_MODE}"
          }
      },
      "connectionSpec": {
          "id": "74a1c565-4e59-48d7-9d67-7c03b8a13137",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| ---| --- |
| `auth.params.server` | [!DNL PostgreSQL] データベースの名前または IP アドレス。 |
| `auth.params.port` | データベースサーバーのポート番号。 |
| `auth.params.database` | [!DNL PostgreSQL] データベースの名前。 |
| `auth.params.username` | [!DNL PostgreSQL] データベース認証に関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL PostgreSQL] データベース認証に関連付けられたパスワード。 |
| `auth.params.sslMode` | データ転送中にデータを暗号化する方法。 使用可能な値は `Disable`、`Allow`、`Prefer`、`Verify Ca`、`Verify Full` などです。 |
| `connectionSpec.id` | [!DNL PostgreSQL] 接続仕様 ID:`74a1c565-4e59-48d7-9d67-7c03b8a13137`。 |

+++

**応答**

リクエストが成功した場合は、新しく作成したベース接続の一意の ID （`id`）が返されます。

+++応答の例を表示

```json
{
    "id": "2c15b1c5-73bf-47ab-9098-0467fcd854d9",
    "etag": "\"2600fc39-0000-0200-0000-67dd48f80000\""
}
```

+++

## 次の手順

[!DNL PostgreSQL] データベースとExperience Platformの間に接続を作成したので、次の手順に進んで、[!DNL PostgreSQL] データをExperience Platformに取り込むことができます。 詳しくは、次のドキュメントを参照してください。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)
