---
keywords: Experience Platform；ホーム；人気のあるトピック；greenplum;Greenplum
solution: Experience Platform
title: フローサービスAPIを使用したGreenPlumベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してGreenPlumをAdobe Experience Platformに接続する方法を説明します。
exl-id: c4ce452a-b4c5-46ab-83ab-61b296c271d0
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL GreenPlum]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL GreenPlum]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL GreenPlum]に正常に接続するために知っておく必要がある追加情報を示します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL GreenPlum]インスタンスへの接続に使用する接続文字列。 [!DNL GreenPlum]の接続文字列パターンは`HOST={SERVER};PORT={PORT};DB={DATABASE};UID={USERNAME};PWD={PASSWORD}`です |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL GreenPlum]の接続仕様IDは`37b6bf40-d318-4655-90be-5cd6f65d334b`です。 |

接続文字列の取得について詳しくは、[このGreenPlumドキュメント](https://gpdb.docs.pivotal.io/580/security-guide/topics/Authenticate.html#topic_fzv_wb2_jr__config_ssl_client_conn)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL GreenPlum]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL GreenPlum]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "GreenPlum test connection",
        "description": "A test connection for a GreenPlum source",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                    "connectionString": "HOST={SERVER};PORT={PORT};DB={DATABASE};UID={USERNAME};PWD={PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "37b6bf40-d318-4655-90be-5cd6f65d334b",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL GreenPlum]アカウントへの接続に使用する接続文字列。 接続文字列のパターンは次のとおりです。`HOST={SERVER};PORT={PORT};DB={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | [!DNL GreenPlum]接続仕様ID:`37b6bf40-d318-4655-90be-5cd6f65d334b`. |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "575abae5-c99a-452c-9aba-e5c99ac52c4d",
    "etag": "\"e5012c89-0000-0200-0000-5eaa036b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL GreenPlum]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/database-nosql.md)を使用してデータベースを調べる方法を学ぶ際に使用できます。[
