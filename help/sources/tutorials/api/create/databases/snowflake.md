---
title: Flow Service API を使用したSnowflakeとExperience Platformの接続
description: Flow Service API を使用してAdobe Experience PlatformをSnowflakeに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 0ef34d30-7b4c-43f5-8e2e-cde05da05aa5
source-git-commit: cde31b692e9a11b15cf91a505133f75f69604cba
workflow-type: tm+mt
source-wordcount: '1187'
ht-degree: 20%

---

# [!DNL Flow Service] API を使用した [!DNL Snowflake] のExperience Platformへの接続

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Snowflake] ソースを利用できます。

[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL Snowflake] ソースアカウントをAdobe Experience Platformに接続する方法については、このガイドを参照してください。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示し [!DNL Snowflake] す。

## [!DNL Snowflake] を Azure 上のExperience Platformに接続 {#azure}

[!DNL Snowflake] ソースを Azure 上のExperience Platformに接続する方法については、以下の手順を参照してください。

### 必要な資格情報の収集

[!DNL Snowflake] ソースを認証するには、次の資格情報プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

| 資格情報 | 説明 |
| ---------- | ----------- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`orgname-account_name`。 詳しくは、[ アカウント識別子の取得 ](../../../../connectors/databases/snowflake.md#retrieve-your-account-identifier) に関するガイドを参  [!DNL Snowflake]  してください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に取り込む際は個別にアクセスする必要があります。 |
| `database` | [!DNL Snowflake] データベースには、Platform に取り込むデータが含まれています。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `password` | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| `role` | [!DNL Snowflake] セッションで使用する既定のアクセス制御ロールです。 役割は、指定したユーザーに既に割り当てられている既存の役割である必要があります。 デフォルトの役割は `PUBLIC` です。 |
| `connectionString` | [!DNL Snowflake] インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake] の接続文字列パターンは `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` です |

>[!TAB  キーペア認証 ]

キーペア認証を使用するには、[!DNL Snowflake] ソースのアカウントを作成する際に、2048 ビットの RSA キーペアを生成してから、次の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`orgname-account_name`。 詳しくは、[ アカウント識別子の取得 ](../../../../connectors/databases/snowflake.md#retrieve-your-account-identifier) に関するガイドを参  [!DNL Snowflake]  してください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| `username` | [!DNL Snowflake] アカウントのユーザー名。 |
| `privateKey` | [!DNL Snowflake] アカウントの [!DNL Base64-] エンコードされた秘密鍵。 暗号化された秘密鍵または暗号化されていない秘密鍵のいずれかを生成できます。 暗号化された秘密鍵を使用している場合は、Experience Platformに対して認証を行う際に、秘密鍵のパスフレーズも指定する必要があります。 詳しくは、[ 秘密鍵の取得 ](../../../../connectors/databases/snowflake.md) に関す  [!DNL Snowflake]  ガイドを参照してください。 |
| `privateKeyPassphrase` | 秘密鍵のパスフレーズは、暗号化された秘密鍵を使用して認証を行う場合に使用する必要がある、追加のセキュリティレイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| `database` | Experience Platformに取り込むデータを含んだ [!DNL Snowflake] データベース。 |
| `warehouse` | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに取り込む際は個別にアクセスする必要があります。 |

これらの値について詳しくは、[[!DNL Snowflake]  キーペア認証ガイド ](https://docs.snowflake.com/en/user-guide/key-pair-auth.html) を参照してください。

>[!ENDTABS]

>[!NOTE]
>
>[!DNL Snowflake] データベースからExperience Platformにデータをアンロードできるようにするには、`PREVENT_UNLOAD_TO_INLINE_URL` フラグを `FALSE` に設定する必要があります。

### Azure 上のExperience Platformに [!DNL Snowflake] のベース接続を作成する {#azure-base}

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Snowflake] 認証資格情報をリクエスト本文の一部として指定します。

**API 形式**

```https
POST /connections
```

>[!BEGINTABS]

>[!TAB ConnectionString]

+++リクエスト

次のリクエストは、[!DNL Snowflake] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Snowflake base connection",
      "description": "Snowflake base connection",
      "auth": {
          "specName": "ConnectionString",
          "params": {
              "connectionString": "jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}"
          }
      },
      "connectionSpec": {
          "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | [!DNL Snowflake] インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake] の接続文字列のパターンは `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` です。 |
| `connectionSpec.id` | [!DNL Snowflake] 接続仕様 ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`。 |

+++

+++応答

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++


>[!TAB  暗号化された秘密鍵を使用したキーペア認証 ]

+++リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Snowflake base connection with encrypted private key",
      "description": "Snowflake base connection with encrypted private key",
      "auth": {
        "specName": "KeyPair Authentication",
        "params": {
            "account": "acme-snowflake123",
            "username": "acme-cj123",
            "database": "ACME_DB",
            "privateKey": "{BASE_64_ENCODED_PRIVATE_KEY}",
            "privateKeyPassphrase": "abcd1234",
            "warehouse": "COMPUTE_WH"
        }
    },
    "connectionSpec": {
        "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
        "version": "1.0"
    }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.account` | [!DNL Snowflake] アカウントの名前。 |
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられたユーザー名。 |
| `auth.params.database` | データの取得元となる [!DNL Snowflake] データベース。 |
| `auth.params.privateKey` | [!DNL Snowflake] アカウントの [!DNL Base64-] エンコードされた暗号化された秘密鍵。 |
| `auth.params.privateKeyPassphrase` | 秘密鍵に対応するパスフレーズ。 |
| `auth.params.warehouse` | 使用している [!DNL Snowflake] ウェアハウス。 |
| `connectionSpec.id` | [!DNL Snowflake] 接続仕様 ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`。 |

+++

+++応答

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++

>[!TAB  暗号化されていない秘密鍵を使用したキーペア認証 ]

+++リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Snowflake base connection with encrypted private key",
      "description": "Snowflake base connection with encrypted private key",
      "auth": {
        "specName": "KeyPair Authentication",
        "params": {
            "account": "acme-snowflake123",
            "username": "acme-cj123",
            "database": "ACME_DB",
            "privateKey": "{BASE_64_ENCODED_PRIVATE_KEY}",
            "warehouse": "COMPUTE_WH"
        }
    },
    "connectionSpec": {
        "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
        "version": "1.0"
    }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.account` | [!DNL Snowflake] アカウントの名前。 |
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられたユーザー名。 |
| `auth.params.database` | データの取得元となる [!DNL Snowflake] データベース。 |
| `auth.params.privateKey` | [!DNL Snowflake] アカウントの [!DNL Base64-] エンコードされた暗号化されていない秘密鍵。 |
| `auth.params.warehouse` | 使用している [!DNL Snowflake] ウェアハウス。 |
| `connectionSpec.id` | [!DNL Snowflake] 接続仕様 ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`。 |

+++

+++応答

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++

>[!ENDTABS]

## Experience Platform on Amazon Web Services（AWS）への [!DNL Snowflake] の接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL Snowflake] ソースをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### AWSでExperience Platform上に [!DNL Snowflake] のベース接続を作成する {#aws-base}

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストでは、AWS上のExperience Platformに日付を取り込む [!DNL Snowflake] のベース接続を作成しています。

+++選択すると例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Snowflake base connection for Experience Platform on AWS",
      "description": "Snowflake base connection for Experience Platform on AWS",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "host": "acme.snowflakecomputing.com",
              "port": "443",
              "username": "acme-cj123",
              "password": "{PASSWORD}",
              "database": "ACME_DB",
              "warehouse": "COMPUTE_WH",
              "schema": "{SCHEMA}"
          }
      },
      "connectionSpec": {
          "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.host` | [!DNL Snowflake] アカウントが接続するホスト URL。 |
| `auth.params.port` | インターネット経由でサーバーに接続するときに [!DNL Snowflake] が使用するポート番号です。 |
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられたユーザー名。 |
| `auth.params.database` | データの取得元となる [!DNL Snowflake] データベース。 |
| `auth.params.password` | [!DNL Snowflake] アカウントに関連付けられたパスワード。 |
| `auth.params.warehouse` | 使用している [!DNL Snowflake] ウェアハウス。 |
| `auth.params.schema` | [!DNL Snowflake] データベースに関連付けられたスキーマの名前。 データベースアクセス権を付与するユーザーが、このスキーマにもアクセスできることを確認する必要があります。 |

+++

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでストレージを調査するために必要になります。

+++選択すると例が表示されます

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700d77b-0000-0200-0000-5e3b41a10000\""
}
```

+++

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Snowflake] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
