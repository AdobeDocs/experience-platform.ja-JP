---
title: Flow Service APIを使用してSnowflakeをExperience Platformに接続する
description: Flow Service APIを使用してAdobe Experience PlatformをSnowflakeに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 0ef34d30-7b4c-43f5-8e2e-cde05da05aa5
source-git-commit: 7ccb8f7c6cfe6e3d030e79bc03ea0136003e5dfa
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 26%

---

# [!DNL Flow Service] APIを使用して[!DNL Snowflake]をExperience Platformに接続します

>[!IMPORTANT]
>
>[!DNL Snowflake] ソースは、Real-Time Customer Data Platform Ultimateを購入したユーザーがソースカタログで利用できます。

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/)を使用して[!DNL Snowflake] ソースアカウントをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform APIの使用

Experience Platform APIの呼び出しを正常に行う方法について詳しくは、[Experience Platform APIの概要](../../../../../landing/api-guide.md)に関するガイドを参照してください。

次の節では、[!DNL Flow Service] APIを使用して[!DNL Snowflake]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Snowflake] 概要](../../../../connectors/databases/snowflake.md#prerequisites)を参照してください。

## [!DNL Snowflake]をAzure上のExperience Platformに接続 {#azure}

Azureで[!DNL Snowflake] ソースをExperience Platformに接続する方法について詳しくは、以下の手順を参照してください。

>[!NOTE]
>
>[!DNL Snowflake] データベースからExperience Platformへのデータのアンロードを許可するには、`PREVENT_UNLOAD_TO_INLINE_URL` フラグを`FALSE`に設定する必要があります。

### Azure上のExperience Platformで[!DNL Snowflake]のベース接続を作成する {#azure-base}

ベース接続は、ソースの認証情報、接続の現在の状態、一意のベース接続IDなど、ソースとExperience Platform間の情報を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続IDを作成するには、[!DNL Snowflake]認証情報をリクエスト本文の一部として提供しながら、`/connections` エンドポイントにPOST リクエストを行います。

**API 形式**

```https
POST /connections
```

>[!BEGINTABS]

>[!TAB 暗号化された秘密鍵を使用したキーペア認証]

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
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられているユーザー名。 |
| `auth.params.database` | データの取得元となる[!DNL Snowflake] データベース。 |
| `auth.params.privateKey` | お客様の[!DNL Snowflake] アカウントの[!DNL Base64-] エンコード済み暗号化された秘密鍵。 |
| `auth.params.privateKeyPassphrase` | 秘密鍵に対応するパスフレーズ。 |
| `auth.params.warehouse` | 使用している[!DNL Snowflake] ウェアハウス。 |
| `connectionSpec.id` | [!DNL Snowflake]接続仕様ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++

>[!TAB 暗号化されていない秘密鍵を使用したキーペア認証]

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
      "name": "Snowflake base connection with unencrypted private key",
      "description": "Snowflake base connection with unencrypted private key",
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
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられているユーザー名。 |
| `auth.params.database` | データの取得元となる[!DNL Snowflake] データベース。 |
| `auth.params.privateKey` | お客様の[!DNL Snowflake] アカウントの[!DNL Base64-] エンコードされた暗号化されていない秘密鍵。 |
| `auth.params.warehouse` | 使用している[!DNL Snowflake] ウェアハウス。 |
| `connectionSpec.id` | [!DNL Snowflake]接続仕様ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++

>[!ENDTABS]

## [!DNL Snowflake]をAmazon Web Services （AWS）上のExperience Platformに接続 {#aws}

>[!AVAILABILITY]
>
>この節は、Amazon Web Services（AWS）で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、一部のお客様にご利用いただけます。 サポートされているExperience Platform インフラストラクチャについて詳しくは、[Experience Platform マルチクラウドの概要](../../../../../landing/multi-cloud.md)を参照してください。

AWSで[!DNL Snowflake] ソースをExperience Platformに接続する方法について詳しくは、以下の手順を参照してください。

### AWSのExperience Platformで[!DNL Snowflake]のベース接続を作成する {#aws-base}

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB 基本認証]

次のリクエストは、[!DNL Snowflake]がAWS上のExperience Platformにデータを取り込むためのベース接続を作成します。

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
| `auth.params.port` | インターネット経由でサーバーに接続する際に[!DNL Snowflake]が使用するポート番号。 |
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられているユーザー名。 |
| `auth.params.database` | データの取得元となる[!DNL Snowflake] データベース。 |
| `auth.params.password` | [!DNL Snowflake] アカウントに関連付けられているパスワード。 |
| `auth.params.warehouse` | 使用している[!DNL Snowflake] ウェアハウス。 |
| `auth.params.schema` | [!DNL Snowflake] データベースに関連付けられているスキーマの名前。 データベースにアクセス権を付与するユーザーがこのスキーマにもアクセス権を持っていることを確認する必要があります。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700d77b-0000-0200-0000-5e3b41a10000\""
}
```

+++

>[!TAB 暗号化されていない秘密鍵を使用したキーペア認証]

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
      "name": "Snowflake base connection with unencrypted private key",
      "description": "Snowflake base connection with unencrypted private key",
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
| `auth.params.username` | [!DNL Snowflake] アカウントに関連付けられているユーザー名。 |
| `auth.params.database` | データの取得元となる[!DNL Snowflake] データベース。 |
| `auth.params.privateKey` | お客様の[!DNL Snowflake] アカウントの[!DNL Base64-] エンコードされた暗号化されていない秘密鍵。 |
| `auth.params.warehouse` | 使用している[!DNL Snowflake] ウェアハウス。 |
| `connectionSpec.id` | [!DNL Snowflake]接続仕様ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`。 |

+++


+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700d77b-0000-0200-0000-5e3b41a10000\""
}
```

+++

>[!ENDTABS]

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Snowflake] ベース接続を作成しました。 このベース接続 ID は、次のチュートリアルで使用できます。

* [&#x200B; [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [&#x200B; [!DNL Flow Service] APIを使用してデータベースデータをExperience Platformに取り込むデータフローを作成します](../../collect/database-nosql.md)
