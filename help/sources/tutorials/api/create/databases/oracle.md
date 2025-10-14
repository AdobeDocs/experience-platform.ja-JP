---
title: Flow Service API を使用したOracle DB のExperience Platformへの接続
description: API を使用してOracle DB をExperience Platformに接続する方法について説明します。
exl-id: b1cea714-93ff-425f-8e12-6061da97d094
source-git-commit: aa5496be968ee6f117649a6fff2c9e83a4ed7681
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 5%

---

# [!DNL Oracle DB] API を使用した [!DNL Flow Service] のExperience Platformへの接続

このガイドでは、[!DNL Oracle DB]API[[!DNL Flow Service]  を使用して &#x200B;](https://developer.adobe.com/experience-platform-apis/references/flow-service/) アカウントをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Oracle] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Flow Service] ています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Oracle DB]  概要 &#x200B;](../../../../connectors/databases/oracle.md#prerequisites) を参照してください。

## [!DNL Oracle DB] を Azure 上のExperience Platformに接続 {#azure}

[!DNL Oracle DB] アカウントを Azure 上のExperience Platformに接続する方法については、以下の手順を参照してください。

### Azure 上のExperience Platformに [!DNL Oracle DB] のベース接続を作成する {#azure-base}

ベース接続は、ソースをExperience Platformにリンクし、認証の詳細、接続ステータス、一意の ID を保存します。 この ID を使用して、ソースファイルを参照し、データのタイプや形式など、取り込む特定の項目を特定します。

**API 形式**

```https
POST /connections
```

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、リクエストパラメーターの一部として [!DNL Oracle DB] 認証資格情報を指定します。

**リクエスト**

次のリクエストは、接続文字列認証を使用して、[!DNL Oracle DB] のベース接続を作成します。

+++リクエストを表示


```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Oracle DB base connection",
    "description": "A base connection to connect Oracle DB to Experience Platform on Azure",
    "auth": {
      "specName": "ConnectionString",
      "params": {
        "connectionString": "Host={HOST};Port={PORT};Sid={SID};UserId={USERNAME};Password={PASSWORD}"
      }
    },
    "connectionSpec": {
      "id": "d6b52d86-f0f8-475f-89d4-ce54c8527328",
      "version": "1.0"
    }
  }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL Oracle DB] への接続に使用する接続文字列。 [!DNL Oracle DB] の接続文字列パターンは `Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}` です。 |
| `connectionSpec.id` | [!DNL Oracle] 接続仕様 ID: `d6b52d86-f0f8-475f-89d4-ce54c8527328`。 |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続の詳細が返されます。

+++応答を表示

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

+++

## Amazon Web Services上のExperience Platformへの [!DNL Oracle DB] の接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL Oracle DB] アカウントをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### AWS上のExperience Platformに [!DNL Oracle DB] のベース接続を作成する {#aws-base}

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle DB] がAWS上のExperience Platformに接続するためのベース接続を作成します。

+++リクエストを表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Oracle DB on Experience Platform AWS",
      "description": "Oracle DB on Experience Platform AWS",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "diy.us-dawkins-1.oraclecloud.com",
              "port": "1521",
              "database": "mcmg_profits_diy.oraclecloud.com",
              "username": "Admin",
              "password": "xxxx",
              "schema": "ADMIN",
              "sslMode": "true"
          }
      },
      "connectionSpec": {
          "id": "26d738e0-8963-47ea-aadf-c60de735468a",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL Oracle DB] サーバーの IP アドレスまたはホスト名。 |
| `auth.params.port` | [!DNL Oracle DB] サーバーのポート番号。 |
| `auth.params.database` | 接続先の [!DNL Oracle DB] インスタンスの名前。 |
| `auth.params.username` | [!DNL Oracle DB] インスタンスに関連付けられたユーザーアカウント。 |
| `auth.prams.password` | [!DNL Oracle DB] ユーザーアカウントに対応するパスワード。 |
| `auth.params.schema` | データベースオブジェクトを含むスキーマ。 |
| `auth.params.sslMode` | SSL 測定を適用するかどうかを示すブール値。 |
| `connectionSpec.id` | [!DNL Oracle DB] ソースに対応する接続仕様 ID。 この ID 値は次のように固定されます。`d6b52d86-f0f8-475f-89d4-ce54c8527328.` |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）と対応する接続を含め、新しく作成されたベース接続の詳細が返されます。 ID を使用して [&#x200B; ソース接続を作成 &#x200B;](../../collect/database-nosql.md#create-a-source-connection) し、`etag` を使用して [&#x200B; アカウントを更新 &#x200B;](../../update.md) できます。

+++応答を表示

```json
{
    "id": "f847950c-1c12-4568-a550-d5312b16fdb8",
    "etag": "\"0c0099f4-0000-0200-0000-67da91710000\""
}
```

+++


## データのデータフロー [!DNL Oracle DB] 作成

[!DNL Oracle DB] アカウントに正常に接続したので、次は [&#x200B; データフローを作成し、データベースからExperience Platformにデータを取り込む &#x200B;](../../collect/database-nosql.md) ことができます。