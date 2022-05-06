---
keywords: Experience Platform；ホーム；人気の高いトピック；IBM [!DNL IBM DB2];IBM;ibm [!DNL IBM DB2];[!DNL IBM DB2];[!DNL IBM DB2]
solution: Experience Platform
title: IBMの作成 [!DNL IBM DB2] フローサービス API を使用したベース接続
topic-legacy: overview
type: Tutorial
description: IBMの接続方法 [!DNL IBM DB2] フローサービス API を使用してAdobe Experience Platformに送信する。
exl-id: 83c1dbe6-975f-4e3b-a7bf-166eb5106dd2
source-git-commit: 93061c84639ca1fdd3f7abb1bbd050eb6eebbdd6
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 41%

---

# IBMの作成 [!DNL IBM DB2] を使用したベース接続 [!DNL Flow Service] API

>[!NOTE]
>
>ザIBM [!DNL IBM DB2] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL IBM DB2] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL IBM DB2] の使用 [!DNL Flow Service] API

| 認証情報 | 説明 |
| ---------- | ----------- |
| `server` | の名前 [!DNL IBM DB2] サーバー。 サーバー名の後にコロンで区切ったポート番号を指定できます。 例：server:port. |
| `database` | の名前 [!DNL IBM DB2] データベース。 |
| `username` | に接続するために使用するユーザー名 [!DNL IBM DB2] データベース。 |
| `password` | ユーザー名に指定したユーザーアカウントのパスワード。 |
| `connectionSpec.id` | 接続の作成に必要な一意の識別子。 の接続仕様 ID [!DNL IBM DB2] が `09182899-b429-40c9-a15a-bf3ddbc8ced7`. |

の導入について詳しくは、 [この [!DNL IBM DB2] 文書](https://www.ibm.com/support/knowledgecenter/SSFMBX/com.ibm.swg.im.dashdb.doc/connecting/connect_credentials.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL IBM DB2] 認証資格情報をリクエストパラメーターの一部として使用します。

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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `auth.params.connectionString` | 次に示すように、 [!DNL IBM DB2] アカウント |
| `connectionSpec.id` | この [!DNL IBM DB2] 接続仕様 ID: `09182899-b429-40c9-a15a-bf3ddbc8ced7`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "575abae5-c99a-452c-9aba-e5c99ac52c4d",
    "etag": "\"e5012c89-0000-0200-0000-5eaa036b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL IBM DB2] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)

