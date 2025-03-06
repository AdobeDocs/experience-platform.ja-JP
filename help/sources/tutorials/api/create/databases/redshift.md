---
title: Flow Service API を使用したAWS Redshift のExperience Platformへの接続
description: Flow Service API を使用してAdobe Experience PlatformをAWS Redshift に接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 2728ce08-05c9-4dca-af1d-d2d1b266c5d9
source-git-commit: dd9aee1ac887637d4761188d6dbcf55ad5bde407
workflow-type: tm+mt
source-wordcount: '721'
ht-degree: 37%

---

# [!DNL Flow Service] API を使用した [!DNL AWS Redshift] のExperience Platformへの接続

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL AWS Redshift] ソースを利用できます。

[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL AWS Redshift] ソースアカウントをAdobe Experience Platformに接続する方法については、このガイドを参照してください。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

## [!DNL AWS Redshift] を Azure 上のExperience Platformに接続 {#azure}

[!DNL AWS Redshift] ソースを Azure 上のExperience Platformに接続する方法については、以下の手順を参照してください。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL AWS Redshift] に接続するには、次の接続プロパティを指定する必要があります。

|資格情報 |説明 |
| `server` | [!DNL AWS Redshift] インスタンスのサーバー名。 |
| `port` | [!DNL AWS Redshift] サーバーがクライアント接続をリッスンするために使用する TCP ポート。 |
| `username` | [!DNL AWS Redshift] アカウントに関連付けられたユーザー名。 |
| `password` | ユーザーアカウントに対応するパスワード。 |
| `database` | データの取得元となる [!DNL AWS Redshift] データベース。 |
| `connectionSpec.id` |接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL AWS Redshift] の接続仕様 ID は `3416976c-a9ca-4bba-901a-1f08f66978ff` です。 |

基本について詳しくは、この [[!DNL AWS Redshift]  ドキュメント ](https://docs.aws.amazon.com/redshift/latest/gsg/new-user-serverless.html) を参照してください。

### Azure [#azure-base] 上のExperience Platformに [!DNL AWS Redshift] のベース接続を作成する

>[!NOTE]
>
>[!DNL Redshift] のデフォルトのエンコーディング規格は Unicode です。 これは変更できません。

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL AWS Redshift] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

+++選択すると例が表示されます

次のリクエストは、[!DNL AWS Redshift] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "AWS-redshift base connection",
      "description": "base connection for AWS-redshift,
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "{SERVER}",
              "port": "{PORT},
              "username": "{USERNAME}",
              "password": "{PASSWORD}",
              "database": "{DATABASE}"
          }
      },
      "connectionSpec": {
          "id": "3416976c-a9ca-4bba-901a-1f08f66978ff",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL AWS Redshift] インスタンスのサーバー名。 |
| `auth.params.port` | クライアント接続をリッスンするために [!DNL AWS Redshift] サーバーが使用する TCP ポート。 |
| `auth.params.username` | [!DNL AWS Redshift] アカウントに関連付けられたユーザー名。 |
| `auth.params.password` | ユーザーアカウントに対応するパスワード。 |
| `auth.params.database` | データの取得元となる [!DNL AWS Redshift] データベース。 |
| `connectionSpec.id` | [!DNL AWS Redshift] 接続仕様 ID：`3416976c-a9ca-4bba-901a-1f08f66978ff` |

+++

**応答**

+++選択すると例が表示されます

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成された接続が応答として返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "373e88fc-43da-4e3c-be88-fc43da3e3c0f",
    "etag": "\"1700ce7b-0000-0200-0000-5e3b405e0000\""
}
```

+++

## AWS Web Services （AWS）上のExperience Platformへの [!DNL AWS Redshift] の接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、AWS web サービス（AWS）上で動作するExperience Platformの実装に当てはまります。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

[!DNL AWS Redshift] ソースをAWS上のExperience Platformに接続する方法については、以下の手順を参照してください。

### AWS上のExperience Platformに [!DNL AWS Redshift] のベース接続を作成する {#aws-base}

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL AWS Redshift] のベース接続を作成します。

+++選択すると例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "AWS Redshift base connection for Experience Platform on AWS",
      "description": "AWS Redshift base connection for Experience Platform on AWS",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "server": "{SERVER}",
              "port": "5439",
              "username": "{USERNAME}",
              "password": "{PASSWORD}",
              "database": "{DATABASE}",
              "schema": "{SCHEMA}"
          }
      },
      "connectionSpec": {
          "id": "3416976c-a9ca-4bba-901a-1f08f66978ff",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `auth.params.server` | [!DNL AWS Redshift] インスタンスのサーバー名。 |
| `auth.params.port` | クライアント接続をリッスンするために [!DNL AWS Redshift] サーバーが使用する TCP ポート。 |
| `auth.params.username` | [!DNL AWS Redshift] アカウントに関連付けられたユーザー名。 |
| `auth.params.password` | ユーザーアカウントに対応するパスワード。 |
| `auth.params.database` | データの取得元となる [!DNL AWS Redshift] データベース。 |
| `auth.params.schema` | [!DNL AWS Redshift] データベースに関連付けられたスキーマの名前。 データベースアクセス権を付与するユーザーが、このスキーマにもアクセスできることを確認する必要があります。 |
| `connectionSpec.id` | [!DNL AWS Redshift] 接続仕様 ID：`3416976c-a9ca-4bba-901a-1f08f66978ff` |

+++

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでストレージを調査するために必要になります。

+++選択すると例が表示されます

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700d77b-0000-0200-0000-5e3b41a10000\""
}
```

+++


## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL AWS Redshift] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
