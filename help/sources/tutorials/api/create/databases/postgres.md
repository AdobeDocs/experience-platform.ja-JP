---
keywords: Experience Platform；ホーム；人気の高いトピック；PostgreSQL;postgresql;PSQL;psql
solution: Experience Platform
title: フローサービス API を使用した PostgreSQL ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを PostgreSQL に接続する方法を説明します。
exl-id: 5225368a-08c1-421d-aec2-d50ad09ae454
source-git-commit: eea815f72c1e807f4ad6ca6273ba18a9da09ac6e
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 12%

---

# の作成 [!DNL PostgreSQL] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL PostgreSQL] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).


## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL PostgreSQL] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL PostgreSQL]を使用する場合は、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | 次に示すように、 [!DNL PostgreSQL] アカウント この [!DNL PostgreSQL] 接続文字列のパターン： `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL PostgreSQL] が `74a1c565-4e59-48d7-9d67-7c03b8a13137`. |

接続文字列の取得について詳しくは、 [[!DNL PostgreSQL] 文書](https://www.postgresql.org/docs/9.2/app-psql.html).

#### 接続文字列の SSL 暗号化の有効化

の SSL 暗号化を有効にすることができます [!DNL PostgreSQL] 接続文字列を追加します。

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `EncryptionMethod` | で SSL 暗号化を有効にできます [!DNL PostgreSQL] データ。 | <uL><li>`EncryptionMethod=0`(無効)</li><li>`EncryptionMethod=1`(有効)</li><li>`EncryptionMethod=6`(RequestSSL)</li></ul> |
| `ValidateServerCertificate` | が送信した証明書を検証します [!DNL PostgreSQL] データベース `EncryptionMethod` が適用されます。 | <uL><li>`ValidationServerCertificate=0`(無効)</li><li>`ValidationServerCertificate=1`(有効)</li></ul> |

次に、 [!DNL PostgreSQL] SSL 暗号化が追加された接続文字列： `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD};EncryptionMethod=1;ValidateServerCertificate=1`.

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL PostgreSQL] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL PostgreSQL]:

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Test connection for PostgreSQL",
        "description": "Test connection for PostgreSQL",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "74a1c565-4e59-48d7-9d67-7c03b8a13137",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.connectionString` | 次に示すように、 [!DNL PostgreSQL] アカウント この [!DNL PostgreSQL] 接続文字列のパターン： `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | この [!DNL PostgreSQL] 接続仕様 ID: `74a1c565-4e59-48d7-9d67-7c03b8a13137`. |

**応答**

正常な応答は、一意の識別子 (`id`) に設定されます。 この ID は、 [!DNL PostgreSQL] 次のチュートリアルのデータベース。

```json
{
    "id": "056dd1b4-da33-42f9-add1-b4da3392f94e",
    "etag": "\"1700e582-0000-0200-0000-5e3c85180000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL PostgreSQL] を使用した接続 [!DNL Flow Service] API で、接続の一意の ID 値を取得している。 次のチュートリアルでは、この接続 ID を使用して、次の方法を学習できます [フローサービス API を使用したデータベースまたは NoSQL システムの調査](../../explore/database-nosql.md).
