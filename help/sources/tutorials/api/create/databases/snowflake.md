---
keywords: 'エクスペリエンス Platform、home、人気のある話題。「スノーフレーク」: 「スノーフレーク」'
solution: Experience Platform
title: フローサービス API を使用したスノーフレーク基本的な接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Adobe エクスペリエンスプラットフォームをスノーフレークに接続する方法について説明します。
exl-id: 0ef34d30-7b4c-43f5-8e2e-cde05da05aa5
source-git-commit: 76b3e3e9bcb27eb2bd6981ae6eb109410ae16336
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 10%

---

# [!DNL Snowflake]API を使用したベース接続の作成 [!DNL Flow Service]

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、API を使用するための基本的な接続を作成する手順について説明し [!DNL Snowflake] [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース ](../../../../home.md) : [!DNL Experience Platform] 多種多様なソースからのデータの ingested を可能にするとともに、サービスを使用した受信データを構造化、ラベル付け、拡張するための機能を提供し [!DNL Platform] ます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

次の節では、 [!DNL Snowflake] API を使用して正常に接続するために必要な追加情報について説明し [!DNL Flow Service] ます。

### 必要な資格情報の収集

に [!DNL Flow Service] 接続するには [!DNL Snowflake] 、次の接続プロパティを指定する必要があります。

| Chap | 説明 |
| --- | --- |
| `account` | アカウントに関連付けられた完全なアカウント名を指定 [!DNL Snowflake] します。 完全修飾アカウント名には、 [!DNL Snowflake] お客様のアカウント名、地域、およびクラウドプラットフォームが含まれています。 たとえば、`cj12345.east-us-2.azure` のように設定します。アカウント名について詳しくは、次を参照してください [[!DNL Snowflake document on account identifiers] ](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html) 。 |
| `warehouse` | ウェアハウスによって、 [!DNL Snowflake] アプリケーションのクエリ実行プロセスが管理されます。 各 [!DNL Snowflake] ウェアハウスは互いに独立したもので、プラットフォームにデータを取り込むときに個別にアクセスする必要があります。 |
| `database` | データベースには、 [!DNL Snowflake] プラットフォームに取り込むデータが格納されています。 |
| `username` | [!DNL Snowflake]取引先企業のユーザー名です。 |
| `password` | ユーザーアカウントのパスワードを入力し [!DNL Snowflake] ます。 |
| `connectionString` | インスタンスへの接続に使用する接続ストリング [!DNL Snowflake] です。 の接続ストリングパターンは、に [!DNL Snowflake] `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` あります。 |
| `connectionSpec.id` | コネクション仕様は、ベースおよびソース接続の作成に関連付けられた認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Snowflake] `b2e08744-4f1a-40ce-af30-7abac3e23cf3` 。 |

概要について詳しくは、このドキュメントを参照してください [[!DNL Snowflake]  ](https://docs.snowflake.com/en/user-guide/oauth-custom.html) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、エンドポイントへの POST 要求を行いますが、 `/connections` [!DNL Snowflake] 要求本文の一部として認証資格情報を指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次の要求によって、次のような基本的な接続が作成され [!DNL Snowflake] ます。

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
| `auth.params.connectionString` | インスタンスへの接続に使用する接続ストリング [!DNL Snowflake] です。 の接続ストリングパターンは、に [!DNL Snowflake] `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` あります。 |
| `connectionSpec.id` | [!DNL Snowflake]コネクション仕様 ID `b2e08744-4f1a-40ce-af30-7abac3e23cf3` 。 |

**応答**

応答が成功した場合は、新しく作成された接続が返されます。これには、一意の接続識別子 () が含ま `id` れます。 この ID は、次のチュートリアルでデータを検索するために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルでは、 [!DNL Snowflake] API を使用して接続を作成 [!DNL Flow Service] し、接続の一意の ID 値を取得しました。 この接続 ID は、 [ フローサービス API を使用してデータベースを検証する方法について学習するために、次のチュートリアルで使用でき ](../../explore/database-nosql.md) ます。
