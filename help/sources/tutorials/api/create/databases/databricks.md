---
title: Flow Service API を使用した Databricks のExperience Platformへの接続
description: API を使用して Databricks をExperience Platformに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
badgeBeta: label="ベータ版" type="Informative"
exl-id: c3974bab-8e67-49a1-b1a5-d453cf7bfd1d
source-git-commit: 96e395e3b3d977d7eb04c400f6fd290977bf1101
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 17%

---

# [!DNL Databricks] API を使用した [!DNL Flow Service] のExperience Platformへの接続

>[!AVAILABILITY]
>
>* Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Databricks] ソースを利用できます。
>
>* [!DNL Databricks] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、ソースの概要の [ 利用条件 ](../../../../home.md#terms-and-conditions) を参照してください。

このガイドでは、[!DNL Databricks]API[[!DNL Flow Service]  を使用して ](https://developer.adobe.com/experience-platform-apis/references/flow-service/) アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API の呼び出しを正常に実行する方法について詳しくは、[Experience Platform API の基本を学ぶ方法 ](../../../../../landing/api-guide.md) に関するガイドを参照してください。

### 前提条件の設定

アカウントをExperience Platformに接続する前に完了する必要がある前提条件の設定については、[[!DNL Databricks]  概要 ](../../../../connectors/databases/databricks.md) を参照してください。

### 必要な資格情報の収集

[!DNL Databricks] をExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `domain` | [!DNL Databricks] ワークスペースの URL。 例：`https://adb-1234567890123456.7.azuredatabricks.net`。 |
| `clusterId` | [!DNL Databricks] 内のクラスターの ID。 このクラスターは、既に既存のクラスターであり、インタラクティブなクラスターである必要があります。 |
| `accessToken` | [!DNL Databricks] アカウントを認証するアクセストークン。 [!DNL Databricks] ワークスペースを使用してアクセストークンを生成できます。 |
| `database` | delta lake 内のデータベースの名前。 |
| `connectionSpec.Id` | 接続仕様 ID は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。 [!DNL Databricks] の接続仕様 ID は `e9d7ec6b-0873-4e57-ad21-b3a7c65e310b` です。 |

詳しくは、[[!DNL Databricks] 概要](../../../../connectors/databases/databricks.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、[!DNL Databricks] アカウントに適切な認証資格情報を指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストでは、アクセストークン認証を使用して、[!DNL Databricks] ソースのベース接続を作成しています。

+++リクエストの例を表示

```shell
curl -X POST \
'https://platform.adobe.io/data/foundation/flowservice/connections' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'x-api-key: {API_KEY}' \
-H 'x-gw-ims-org-id: {ORG_ID}' \
-H 'x-sandbox-name: {SANDBOX_NAME}' \
-H 'Content-Type: application/json' \
-d '{
    "name": "Databricks connection to Experience Platform",
    "description": "A Databricks base connection to Experience Platform",
    "auth": {
        "specName": "Access Token Authentication",
        "params": {
          "domain": "https://adb-1234567890123456.7.azuredatabricks.net",
          "clusterId": "xxxx",
          "accessToken": "xxxx",
          "database": "acme-db"
        }
    },
    "connectionSpec": {
        "id": "e9d7ec6b-0873-4e57-ad21-b3a7c65e310b",
        "version": "1.0"
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.domain` | [!DNL Databricks] ワークスペースの URL。 |
| `auth.params.clusterId` | [!DNL Databricks] 内のクラスターの ID。 このクラスターは、既に既存のクラスターであり、対話型クラスターである必要があります |
| `auth.params.accessToken` | [!DNL Databricks] アカウントを認証するアクセストークン。 |
| `auth.params.database` | delta lake 内のデータベースの名前。 |
| `connectionSpec.id` | [!DNL Databricks] 接続仕様 ID。 |

+++

**応答**

正常な応答では、ベース接続 ID を含む、新しく作成された接続が返されます。

+++応答の例を表示

```json
{
    "id": "f847950c-1c12-4568-a550-d5312b16fdb8",
    "etag": "\"0c0099f4-0000-0200-0000-67da91710000\""
}
```

+++

## 次の手順

このチュートリアルでは、[!DNL Databricks] アカウントとExperience Platformの間の接続を正常に作成しました。 次のチュートリアルでは、新しく生成されたベース接続 ID を使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)
