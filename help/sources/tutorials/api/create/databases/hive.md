---
keywords: Experience Platform；ホーム；人気の高いトピック；Apache hive;hive;Hive
solution: Experience Platform
title: フローサービス API を使用して Azure HDInsights Base Connection 上に Apache Hive を作成する
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Azure HDInsights 上の Apache Hive をAdobe Experience Platformに接続する方法を説明します。
exl-id: e1469a29-6f61-47ba-995e-39f06ee4a4a4
source-git-commit: 93061c84639ca1fdd3f7abb1bbd050eb6eebbdd6
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 53%

---

# の作成 [!DNL Apache Hive] オン [!DNL Azure HDInsights] を使用したベース接続 [!DNL Flow Service] API

>[!NOTE]
>
>この [!DNL Apache Hive] オン [!DNL Azure HDInsights] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Apache Hive] オン [!DNL Azure HDInsights] （以下「」という。）[!DNL Hive]」) [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Hive] の使用 [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Hive] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Hive] サーバー。 |
| `username` | アクセスに使用するユーザー名 [!DNL Hive] サーバー。 |
| `password` | ユーザーに対応するパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Hive] 次に該当： `aac9bbd4-6c01-46ce-b47e-51c6f0f6db3f` |

の導入について詳しくは、 [この Hive ドキュメント](https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-GettingStarted).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Hive] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Hive] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Apache Hive test connection",
        "description": "A test connection for Apache Hive",
        "auth": {
            "specName": "HDInsights Basic Authentication",
            "params": {
                "connectionString": "{CONNECTION_STRING}"
            }
        },
        "connectionSpec": {
            "id": "aac9bbd4-6c01-46ce-b47e-51c6f0f6db3f",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | 次に示すように、 [!DNL Hive] アカウント |
| `connectionSpec.id` | この [!DNL Hive] 接続仕様 ID: `aac9bbd4-6c01-46ce-b47e-51c6f0f6db3f`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "9f6e4311-e032-4c00-ae43-11e032bc00c7",
    "etag": "\"f4004fb7-0000-0200-0000-5e865c1e0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Apache Hive] オン [!DNL Azure HDInsights] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)
