---
title: Flow Service APIを使用してDatabricksをExperience Platformに接続します
description: APIを使用してDatabricksをExperience Platformに接続する方法について説明します。
exl-id: c3974bab-8e67-49a1-b1a5-d453cf7bfd1d
source-git-commit: 23b8d5d49e217d587dfe3d68631e6056c61b2cb8
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 16%

---

# [!DNL Databricks] APIを使用して[!DNL Flow Service]をExperience Platformに接続します

>[!AVAILABILITY]
>
>[!DNL Databricks] ソースは、Real-Time CDP Ultimateを購入したユーザーがソースカタログで利用できます。

このガイドでは、[!DNL Databricks]API[[!DNL Flow Service] を使用して](https://developer.adobe.com/experience-platform-apis/references/flow-service/) アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むことができますが、Experience Platform サービスを使用して着信データを構造化、ラベル付け、強化することができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformは、1つのExperience Platform インスタンスを個別のバーチャル環境に分割して、デジタルエクスペリエンスアプリケーションの開発と進化に役立つバーチャルサンドボックスを提供します。

### Experience Platform APIの使用

Experience Platform APIを正常に呼び出す方法について詳しくは、[Experience Platform APIの使い方](../../../../../landing/api-guide.md)に関するガイドを参照してください。

### 前提条件の設定

アカウントをExperience Platformに接続する前に完了する必要がある前提条件の設定については、[[!DNL Databricks] 概要](../../../../connectors/databases/databricks.md)を参照してください。

### 必要な資格情報の収集

[!DNL Databricks]をExperience Platformに接続するために、次の資格情報の値を指定してください。

| 資格情報 | 説明 |
| --- | --- |
| `domain` | [!DNL Databricks] ワークスペースのURL。 例：`https://adb-1234567890123456.7.azuredatabricks.net` |
| `clusterId` | [!DNL Databricks]のクラスターのID。 このクラスターは既に既存のクラスターである必要があり、インタラクティブクラスターである必要があります。 |
| `accessToken` | [!DNL Databricks] アカウントを認証するアクセストークン。 [!DNL Databricks] ワークスペースを使用してアクセストークンを生成できます。 |
| `database` | 差分レイク内のデータベースの名前。 |
| `catalog` | 差分レイク内のカタログの名前。 デフォルトカタログの値を指定する必要はありません。 |
| `connectionSpec.Id` | 接続仕様IDは、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Databricks]の接続仕様IDは`e9d7ec6b-0873-4e57-ad21-b3a7c65e310b`です。 |

詳しくは、[[!DNL Databricks] 概要](../../../../connectors/databases/databricks.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースの認証情報、接続の現在の状態、一意のベース接続IDなど、ソースとExperience Platform間の情報を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続IDを作成するには、`/connections` エンドポイントにPOST リクエストを行い、[!DNL Databricks] アカウントに適切な認証情報を提供します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、アクセストークン認証を使用して[!DNL Databricks] ソースのベース接続を作成します。

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
| `auth.params.domain` | [!DNL Databricks] ワークスペースのURL。 |
| `auth.params.clusterId` | [!DNL Databricks]のクラスターのID。 このクラスターは既に既存のクラスターである必要があり、インタラクティブクラスターである必要があります |
| `auth.params.accessToken` | [!DNL Databricks] アカウントを認証するアクセストークン。 |
| `auth.params.database` | 差分レイク内のデータベースの名前。 |
| `connectionSpec.id` | [!DNL Databricks]接続仕様ID。 |

+++

**応答**

応答が成功すると、新しく作成した接続（ベース接続IDを含む）が返されます。

+++応答の例を表示

```json
{
    "id": "f847950c-1c12-4568-a550-d5312b16fdb8",
    "etag": "\"0c0099f4-0000-0200-0000-67da91710000\""
}
```

+++

## 次の手順

このチュートリアルに従うことで、[!DNL Databricks] アカウントとExperience Platformの間の接続が正常に作成されました。 新しく生成したベース接続IDは、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] APIを使用してデータベースデータをExperience Platformに取り込むデータフローを作成します](../../collect/database-nosql.md)
