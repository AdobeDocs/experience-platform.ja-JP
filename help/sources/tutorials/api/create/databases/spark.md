---
keywords: Experience Platform；ホーム；人気のトピック；Apache Spark;Apache spark;Azure HDInsights
solution: Experience Platform
title: フローサービス API を使用した Azure HDInsights Base Connection での Apache Spark の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Azure HDInsights 上の Apache Spark をAdobe Experience Platformに接続する方法を説明します。
exl-id: 1f7ca86e-32f4-45f7-92c2-f87c5c0c4ea4
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 10%

---

# の作成 [!DNL Apache Spark] オン [!DNL Azure] HDInsights ベース接続 ( [!DNL Flow Service] API

>[!NOTE]
>
>この [!DNL Apache Spark] オン [!DNL Azure HDInsights] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Apache Spark] オン [!DNL Azure HDInsights] （以下「」という。）[!DNL Spark]」) [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Spark] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL Spark]を使用する場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Spark] サーバー。 |
| `username` | アクセスに使用するユーザー名 [!DNL Spark] サーバー。 |
| `password` | ユーザーに対応するパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Spark] 次に該当： `6a8d82bc-1caf-45d1-908d-cadabc9d63a6` |

の導入について詳しくは、 [この Spark ドキュメント](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-overview).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Spark] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Spark]:


```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Spark test connection",
        "description": "A Spark test connection",
        "auth": {
            "specName": "HDInsights Basic Authentication",
        "params": {
            "host":  "{HOST}",
            "username": "{USERNAME}",
            "password":"{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "6a8d82bc-1caf-45d1-908d-cadabc9d63a6",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.host` | のホスト [!DNL Spark] サーバー。 |
| `auth.params.username` | ユーザー名 [!DNL Spark] 接続。 |
| `auth.params.password` | ユーザーに関連付けられたパスワード [!DNL Spark] 接続。 |
| `connectionSpec.id` | この [!DNL Spark] 接続仕様 ID: `6a8d82bc-1caf-45d1-908d-cadabc9d63a6`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "a45f2f58-e3a2-46ba-9f2f-58e3a2b6baf2",
    "etag": "\"900009d6-0000-0200-0000-5e8500010000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Spark] を使用した接続 [!DNL Flow Service] API を介して取得され、接続の一意の ID 値を取得している。 この ID は、次のチュートリアルで、 [フローサービス API を使用したデータベースの調査](../../explore/database-nosql.md).
