---
keywords: Experience Platform；ホーム；人気のトピック；couchbase;Couchbase
solution: Experience Platform
title: Flow Service API を使用した Couchbase ベース接続の作成
type: Tutorial
description: Flow Service API を使用して Couchbase をAdobe Experience Platformに接続する方法について説明します。
exl-id: 625e3acf-fc27-44cf-b4e6-becf1d107ff2
source-git-commit: 0e3fee4d78646b1d1d6730495358b3ced4127f4e
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 63%

---

# [!DNL Flow Service] API を使用した [!DNL Couchbase] ベース接続の作成

>[!IMPORTANT]
>
>[!DNL Couchbase] ソースは 2025 年 5 月末に非推奨（廃止予定）になります。 別の方法として、[[!DNL Data Landing Zone]](../cloud-storage/data-landing-zone.md) ソースを使用することもできます。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Couchbase] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Couchbase] ています。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Couchbase] インスタンスへの接続に使用する接続文字列。 [!DNL Couchbase] の接続文字列のパターンは `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];` です。 接続文字列の取得について詳しくは、[ この Couchbase ドキュメント ](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview) を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Couchbase] の接続仕様 ID は `1fe283f6-9bec-11ea-bb37-0242ac130002` です。 |

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Couchbase] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Couchbase] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Couchbase test connection",
        "description": "A test connection for a Couchbase source",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                    "connectionString": "Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];"
                }
        },
        "connectionSpec": {
            "id": "1fe283f6-9bec-11ea-bb37-0242ac130002",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL Couchbase] アカウントへの接続に使用する接続文字列。 接続文字列のパターンは `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];` です。 |
| `connectionSpec.id` | [!DNL Couchbase] 接続仕様 ID: `1fe283f6-9bec-11ea-bb37-0242ac130002`。 |

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成された接続の詳細が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "54997109-07b5-40b7-9971-0907b5a0b75a",
    "etag": "\"280168f5-0000-0200-0000-5ed72b230000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Couchbase] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
