---
title: フローサービス API を使用したSnowflakeベース接続の作成
description: フローサービス API を使用してAdobe Experience PlatformをSnowflakeに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 0ef34d30-7b4c-43f5-8e2e-cde05da05aa5
source-git-commit: 4de2193a45fc2925af310b5e2475eabe26d13adc
workflow-type: tm+mt
source-wordcount: '932'
ht-degree: 25%

---

# [!DNL Flow Service] API を使用した [!DNL Snowflake] ベース接続の作成

>[!IMPORTANT]
>
>The [!DNL Snowflake] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログで利用できます。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

次のチュートリアルを使用して、のベース接続を作成する方法を学びます。 [!DNL Snowflake] の使用 [[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

次の節では、に正常に接続するために必要な追加情報を示します。 [!DNL Snowflake] の使用 [!DNL Flow Service] API.

### 必要な資格情報の収集

次の資格情報プロパティの値を指定して、 [!DNL Snowflake] ソース。

>[!BEGINTABS]

>[!TAB アカウントキー認証]

| 資格情報 | 説明 |
| ---------- | ----------- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 その場合は、異なる [!DNL Snowflake] 組織。 これをおこなうには、アカウント名の前に組織名を追加する必要があります。 例えば、`orgname-account_name` のようになります。アカウント名の詳細については、 [!DNL Snowflake] に関するドキュメント [アカウント識別子](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization). |
| `warehouse` | The [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に引き渡す際には、個別にアクセスする必要があります。 |
| `database` | The [!DNL Snowflake] データベースには、Platform を取り込むデータが含まれます。 |
| `username` | のユーザー名 [!DNL Snowflake] アカウント。 |
| `password` | のパスワード [!DNL Snowflake] ユーザーアカウント。 |
| `role` | デフォルトのアクセス制御の役割で、 [!DNL Snowflake] セッション。 この役割は、指定したユーザーに既に割り当てられている既存の役割である必要があります。 デフォルトの役割は `PUBLIC`. |
| `connectionString` | に接続するために使用される接続文字列 [!DNL Snowflake] インスタンス。 次の接続文字列パターン： [!DNL Snowflake] 次に該当 `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` |

>[!TAB キーペア認証]

キーペア認証を使用するには、2048 ビットの RSA キーペアを生成し、次の値を指定して、 [!DNL Snowflake] ソース。

| 資格情報 | 説明 |
| --- | --- |
| `account` | アカウント名は、組織内のアカウントを一意に識別します。 その場合は、異なる [!DNL Snowflake] 組織。 これをおこなうには、アカウント名の前に組織名を追加する必要があります。 例えば、`orgname-account_name` のようになります。アカウント名の詳細については、 [!DNL Snowflake] に関するドキュメント [アカウント識別子](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization). |
| `username` | ユーザー名 [!DNL Snowflake] アカウント。 |
| `privateKey` | The [!DNL Base64-]エンコードされた秘密鍵 [!DNL Snowflake] アカウント。 暗号化または暗号化されていない秘密鍵を生成できます。 暗号化された秘密鍵を使用する場合は、Experience Platformに対する認証時に秘密鍵のパスフレーズも指定する必要があります。 |
| `privateKeyPassphrase` | 秘密鍵のパスフレーズは、暗号化された秘密鍵で認証する際に使用する必要があるセキュリティの追加レイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| `database` | The [!DNL Snowflake] データベースに取り込むデータを含むExperience Platform。 |
| `warehouse` | The [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データをExperience Platformに引き渡す際には、個別にアクセスする必要があります。 |

これらの値について詳しくは、 [[!DNL Snowflake] キーペア認証ガイド](https://docs.snowflake.com/en/user-guide/key-pair-auth.html).

>[!ENDTABS]

>[!NOTE]
>
>次の項目を設定する必要があります。 `PREVENT_UNLOAD_TO_INLINE_URL` フラグを設定 `FALSE` データのアンロードを許可する [!DNL Snowflake] データベースからExperience Platformへ。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Snowflake] 認証資格情報をリクエスト本文の一部として使用します。

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
| `auth.params.connectionString` | に接続するために使用される接続文字列 [!DNL Snowflake] インスタンス。 次の接続文字列パターン： [!DNL Snowflake] 次に該当 `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}`. |
| `connectionSpec.id` | The [!DNL Snowflake] 接続仕様 ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`. |

+++

+++応答

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++


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
| `auth.params.account` | お客様の [!DNL Snowflake] アカウント。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL Snowflake] アカウント。 |
| `auth.params.database` | The [!DNL Snowflake] データの取り込み元となるデータベース。 |
| `auth.params.privateKey` | The [!DNL Base64-]エンコードされた暗号化された秘密鍵 [!DNL Snowflake] アカウント。 |
| `auth.params.privateKeyPassphrase` | 秘密鍵に対応するパスフレーズ。 |
| `auth.params.warehouse` | The [!DNL Snowflake] 使用しているウェアハウス。 |
| `connectionSpec.id` | The [!DNL Snowflake] 接続仕様 ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`. |

+++

+++応答

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

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
| `auth.params.account` | お客様の [!DNL Snowflake] アカウント。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL Snowflake] アカウント。 |
| `auth.params.database` | The [!DNL Snowflake] データの取り込み元となるデータベース。 |
| `auth.params.privateKey` | The [!DNL Base64-]暗号化されていない秘密鍵 [!DNL Snowflake] アカウント。 |
| `auth.params.warehouse` | The [!DNL Snowflake] 使用しているウェアハウス。 |
| `connectionSpec.id` | The [!DNL Snowflake] 接続仕様 ID: `b2e08744-4f1a-40ce-af30-7abac3e23cf3`. |

+++

+++応答

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

+++

>[!ENDTABS]

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Snowflake] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)
