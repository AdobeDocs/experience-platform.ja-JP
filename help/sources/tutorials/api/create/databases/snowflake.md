---
keywords: Experience Platform；ホーム；人気のあるトピック；Snowflake;snowflake
solution: Experience Platform
title: フローサービスAPIを使用したSnowflakeベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをSnowflakeに接続する方法を説明します。
source-git-commit: 5e2301a409f04bd4e23888648f244918b871f0ad
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 9%

---

# [!DNL Flow Service] APIを使用して[!DNL Snowflake]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Snowflake]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

次の節では、[!DNL Flow Service] APIを使用して[!DNL Snowflake]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Snowflake]と接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `account` | Platformに接続する[!DNL Snowflake]アカウント。 |
| `warehouse` | [!DNL Snowflake]ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各[!DNL Snowflake]ウェアハウスは互いに独立しており、データをPlatformに引き渡す際には、個別にアクセスする必要があります。 |
| `database` | [!DNL Snowflake]には、プラットフォームに取り込むデータが含まれます。 |
| `username` | [!DNL Snowflake]アカウントのユーザー名。 |
| `password` | [!DNL Snowflake]ユーザーアカウントのパスワード。 |
| `connectionString` | [!DNL Snowflake]インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake]の接続文字列パターンは`jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}`です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Snowflake]の接続仕様IDは`b2e08744-4f1a-40ce-af30-7abac3e23cf3`です。 |

使い始める方法については、この[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/oauth-custom.html)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエスト本文の一部として[!DNL Snowflake]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Snowflake]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Snowflake base connection",
        "description": "Snowflake base connection",
        "auth": {
            "specName": "Basic Authentication for Snowflake,
            "params": {
                "connectionString": "{CONNECTION_STRING}"
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
| `auth.params.connectionString` | [!DNL Snowflake]インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake]の接続文字列パターンは`jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}`です。 |
| `connectionSpec.id` | [!DNL Snowflake]接続仕様ID:`b2e08744-4f1a-40ce-af30-7abac3e23cf3`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Snowflake]接続を作成し、接続の一意のID値を取得しました。 次のチュートリアルでは、フローサービスAPI](../../explore/database-nosql.md)を使用してデータベースを調べる方法を学ぶ際に、この接続IDを使用できます。[