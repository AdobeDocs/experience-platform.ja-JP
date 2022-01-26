---
keywords: Experience Platform；ホーム；人気のトピック；redshift;Redshift;Amazon Redshift;Amazon Redshift
solution: Experience Platform
title: フローサービス API を使用したAmazon Redshift ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをAmazon Redshift に接続する方法を説明します。
exl-id: 2728ce08-05c9-4dca-af1d-d2d1b266c5d9
source-git-commit: 2fb972b0ec8d1f679c6ce104a439265b5cc4d535
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 11%

---

# の作成 [!DNL Amazon Redshift] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Amazon Redshift] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Amazon Redshift] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL Amazon Redshift]に値を入力する場合は、次の接続プロパティを指定する必要があります。

| **資格情報** | **説明** |
| -------------- | --------------- |
| `server` | サーバーが [!DNL Amazon Redshift] アカウント |
| `username` | ユーザー名 [!DNL Amazon Redshift] アカウント |
| `password` | ユーザーに関連付けられたパスワード [!DNL Amazon Redshift] アカウント |
| `database` | この [!DNL Amazon Redshift] アクセスするデータベース。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Amazon Redshift] が `3416976c-a9ca-4bba-901a-1f08f66978ff`. |

導入の詳細については、 [[!DNL Amazon Redshift] 文書](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

>[!NOTE]
>
>のデフォルトのエンコーディング規格 [!DNL Redshift] は Unicode です。 これは変更できません。

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Amazon Redshift] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Amazon Redshift]:

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
| `auth.params.server` | お使いの [!DNL Amazon Redshift] サーバー。 |
| `auth.params.database` | データベースが [!DNL Amazon Redshift] アカウント |
| `auth.params.password` | ユーザーに関連付けられたパスワード [!DNL Amazon Redshift] アカウント |
| `auth.params.username` | ユーザー名 [!DNL Amazon Redshift] アカウント |
| `connectionSpec.id` | この [!DNL Amazon Redshift] 接続仕様 ID: `3416976c-a9ca-4bba-901a-1f08f66978ff` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "373e88fc-43da-4e3c-be88-fc43da3e3c0f",
    "etag": "\"1700ce7b-0000-0200-0000-5e3b405e0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Amazon Redshift] を使用した接続 [!DNL Flow Service] API で、接続の一意の ID 値を取得している。 次のチュートリアルでは、この接続 ID を使用して、次の方法を学習できます [フローサービス API を使用したデータベースまたは NoSQL システムの調査](../../explore/database-nosql.md).
