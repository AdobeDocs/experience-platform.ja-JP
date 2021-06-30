---
keywords: Experience Platform；ホーム；人気のあるトピック；redshift;Redshift;Amazon Redshift;amazon redshift
solution: Experience Platform
title: フローサービスAPIを使用したAmazon Redshiftベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをAmazon Redshiftに接続する方法を説明します。
exl-id: 2728ce08-05c9-4dca-af1d-d2d1b266c5d9
source-git-commit: 5fb5f0ce8bd03ba037c6901305ba17f8939eb9ce
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 12%

---

# [!DNL Flow Service] APIを使用して[!DNL Amazon Redshift]ベース接続を作成する

>[!NOTE]
>
>[!DNL Amazon Redshift]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して[!DNL Amazon Redshift]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Amazon Redshift]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Amazon Redshift]と接続するには、次の接続プロパティを指定する必要があります。

| **資格情報** | **説明** |
| -------------- | --------------- |
| `server` | [!DNL Amazon Redshift]アカウントに関連付けられたサーバー。 |
| `username` | [!DNL Amazon Redshift]アカウントに関連付けられているユーザー名。 |
| `password` | [!DNL Amazon Redshift]アカウントに関連付けられているパスワード。 |
| `database` | アクセスする[!DNL Amazon Redshift]データベース。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Amazon Redshift]の接続仕様IDは`3416976c-a9ca-4bba-901a-1f08f66978ff`です。 |

使い始める方法については、この[[!DNL Amazon Redshift] ドキュメント](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Amazon Redshift]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Amazon Redshift]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "amazon-redshift base connection",
        "description": "base connection for amazon-redshift,
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "server": "{SERVER}",
                "database": "{DATABASE}",
                "password": "{PASSWORD}",
                "username": "{USERNAME}"
            }
        },
        "connectionSpec": {
            "id": "3416976c-a9ca-4bba-901a-1f08f66978ff",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.server` | [!DNL Amazon Redshift]サーバー。 |
| `auth.params.database` | [!DNL Amazon Redshift]アカウントに関連付けられたデータベース。 |
| `auth.params.password` | [!DNL Amazon Redshift]アカウントに関連付けられているパスワード。 |
| `auth.params.username` | [!DNL Amazon Redshift]アカウントに関連付けられているユーザー名。 |
| `connectionSpec.id` | [!DNL Amazon Redshift]接続仕様ID:`3416976c-a9ca-4bba-901a-1f08f66978ff` |

**応答**

正常な応答は、新しく作成された接続を、一意の識別子(`id`)を含めて返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "373e88fc-43da-4e3c-be88-fc43da3e3c0f",
    "etag": "\"1700ce7b-0000-0200-0000-5e3b405e0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Amazon Redshift]接続を作成し、接続の一意のID値を取得しました。 次のチュートリアルでは、フローサービスAPI](../../explore/database-nosql.md)を使用してデータベースやNoSQLシステムを調べる方法を学ぶ際に、この接続IDを使用できます。[
