---
keywords: Experience Platform；ホーム；人気のトピック；PostgreSQL;postgresql;PSQL;psql
solution: Experience Platform
title: Flow Service API を使用した PostgreSQL ベース接続の作成
type: Tutorial
description: Flow Service API を使用してAdobe Experience Platformを PostgreSQL に接続する方法を説明します。
exl-id: 5225368a-08c1-421d-aec2-d50ad09ae454
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 44%

---

# [!DNL Flow Service] API を使用した [!DNL PostgreSQL] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL PostgreSQL] のベース接続を作成する手順を説明します。


## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL PostgreSQL] ています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL PostgreSQL] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL PostgreSQL] アカウントに関連付けられた接続文字列。 [!DNL PostgreSQL] の接続文字列パターンは `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL PostgreSQL] の接続仕様 ID は `74a1c565-4e59-48d7-9d67-7c03b8a13137` です。 |

接続文字列の取得について詳しくは、この [[!DNL PostgreSQL]  ドキュメント ](https://www.postgresql.org/docs/9.2/app-psql.html) を参照してください。

#### 接続文字列の SSL 暗号化を有効にする

次のプロパティを使用して接続文字列を追加することで、[!DNL PostgreSQL] 接続文字列の SSL 暗号化を有効にできます。

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `EncryptionMethod` | [!DNL PostgreSQL] データに対して SSL 暗号化を有効にできます。 | <uL><li>`EncryptionMethod=0` （無効）</li><li>`EncryptionMethod=1` （有効）</li><li>`EncryptionMethod=6` （RequestSSL）</li></ul> |
| `ValidateServerCertificate` | `EncryptionMethod` ータの適用時に [!DNL PostgreSQL] データベースから送信された証明書を検証します。 | <uL><li>`ValidationServerCertificate=0` （無効）</li><li>`ValidationServerCertificate=1` （有効）</li></ul> |

次に、SSL 暗号化が追加された [!DNL PostgreSQL] 接続文字列の例を示します。`Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD};EncryptionMethod=1;ValidateServerCertificate=1`

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL PostgreSQL] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL PostgreSQL] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `auth.params.connectionString` | [!DNL PostgreSQL] アカウントに関連付けられた接続文字列。 [!DNL PostgreSQL] の接続文字列パターンは `Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}` です。 |
| `connectionSpec.id` | [!DNL PostgreSQL] 接続仕様 ID:`74a1c565-4e59-48d7-9d67-7c03b8a13137`。 |

**応答**

リクエストが成功した場合は、新しく作成したベース接続の一意の ID （`id`）が返されます。 この ID は、次のチュートリアルで [!DNL PostgreSQL] データベースを探索するために必要です。

```json
{
    "id": "056dd1b4-da33-42f9-add1-b4da3392f94e",
    "etag": "\"1700e582-0000-0200-0000-5e3c85180000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PostgreSQL] 接続ベース接続を作成しました。 このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)
