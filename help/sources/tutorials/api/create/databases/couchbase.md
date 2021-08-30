---
keywords: Experience Platform；ホーム；人気のあるトピック；couchbase;Couchbase
solution: Experience Platform
title: フローサービスAPIを使用したCouchbaseベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してCouchbaseをAdobe Experience Platformに接続する方法を説明します。
exl-id: 625e3acf-fc27-44cf-b4e6-becf1d107ff2
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL Couchbase]ベース接続を作成する

>[!NOTE]
>
>[!DNL Couchbase]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Couchbase]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Couchbase]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Couchbase]インスタンスへの接続に使用する接続文字列。 [!DNL Couchbase]の接続文字列パターンは`Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`です。 接続文字列の取得について詳しくは、[このCouchbaseドキュメント](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview)を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Couchbase]の接続仕様IDは`1fe283f6-9bec-11ea-bb37-0242ac130002`です。 |

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Couchbase]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Couchbase]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `auth.params.connectionString` | [!DNL Couchbase]アカウントへの接続に使用する接続文字列。 接続文字列のパターンは次のとおりです。`Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`. |
| `connectionSpec.id` | [!DNL Couchbase]接続仕様ID:`1fe283f6-9bec-11ea-bb37-0242ac130002`. |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "54997109-07b5-40b7-9971-0907b5a0b75a",
    "etag": "\"280168f5-0000-0200-0000-5ed72b230000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Couchbase]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/database-nosql.md)を使用してデータベースを調べる方法を学ぶ際に使用できます。[
