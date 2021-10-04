---
keywords: Experience Platform；ホーム；人気のあるトピック；IBM [!DNL IBM DB2];IBM;ibm [!DNL IBM DB2];[!DNL IBM DB2];[!DNL IBM DB2]
solution: Experience Platform
title: フローサービス API を使用した IBM [!DNL IBM DB2]  ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して IBM [!DNL IBM DB2]  をAdobe Experience Platformに接続する方法を説明します。
exl-id: 83c1dbe6-975f-4e3b-a7bf-166eb5106dd2
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 6%

---

# [!DNL Flow Service] API を使用して IBM [!DNL IBM DB2] ベース接続を作成する

>[!NOTE]
>
>IBM [!DNL IBM DB2] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL IBM DB2] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL IBM DB2] に正常に接続するために知っておく必要がある追加情報を示します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `server` | [!DNL IBM DB2] サーバの名前。 サーバー名の後にコロンで区切ったポート番号を指定できます。 例：server:port. |
| `database` | [!DNL IBM DB2] データベースの名前。 |
| `username` | [!DNL IBM DB2] データベースへの接続に使用するユーザー名。 |
| `password` | ユーザー名に指定したユーザーアカウントのパスワード。 |
| `connectionSpec.id` | 接続の作成に必要な一意の識別子。 [!DNL IBM DB2] の接続仕様 ID は `09182899-b429-40c9-a15a-bf3ddbc8ced7` です。 |

使い始める方法については、[ [!DNL IBM DB2]  ドキュメント ](https://www.ibm.com/support/knowledgecenter/SSFMBX/com.ibm.swg.im.dashdb.doc/connecting/connect_credentials.html) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL IBM DB2] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL IBM DB2] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "[!DNL IBM DB2] connection",
        "description": "[!DNL IBM DB2] test connection",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                    "server": "{SERVER}",
                    "database": "{DATABASE}",
                    "authenticationType": "{AUTHENTICATION_TYPE}",
                    "username": "{USERNAME}",
                    "password": "{PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "09182899-b429-40c9-a15a-bf3ddbc8ced7",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL IBM DB2] アカウントに関連付けられた接続文字列。 |
| `connectionSpec.id` | [!DNL IBM DB2] 接続仕様 ID:`09182899-b429-40c9-a15a-bf3ddbc8ced7`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "575abae5-c99a-452c-9aba-e5c99ac52c4d",
    "etag": "\"e5012c89-0000-0200-0000-5eaa036b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して IBM [!DNL IBM DB2] 接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルでフローサービス API](../../explore/database-nosql.md) を使用してデータベースを調べる方法を学ぶ際に使用できます。[
