---
keywords: Experience Platform；ホーム；人気の高いトピック；ファイル転送プロトコル；ファイル転送プロトコル
solution: Experience Platform
title: フローサービス API を使用した FTP ベース接続の作成
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを FTP（ファイル転送プロトコル）サーバーに接続する方法について説明します。
exl-id: a7bef346-b357-49bc-ac54-ac8b42adac50
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 54%

---

# を使用した FTP ベース接続の作成 [!DNL Flow Service] API

>[!NOTE]
>
>FTP コネクタはベータ版です。 機能とドキュメントは変更される場合があります。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL FTP] （ファイル転送プロトコル） [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL FTP] サーバー [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL FTP] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | に関連付けられている名前または IP アドレス [!DNL FTP] サーバー。 |
| `username` | ユーザー名 ( [!DNL FTP] サーバー。 |
| `password` | ユーザーのパスワード [!DNL FTP] サーバー。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL FTP] の接続仕様 ID は `fb2e94c9-c031-467d-8103-6bd6e0a432f2` です。 |

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL FTP] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL FTP] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
        "name": "FTP connector with password",
        "description": "FTP connector password",
        "auth": {
            "specName": "Basic Authentication for FTP",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "fb2e94c9-c031-467d-8103-6bd6e0a432f2",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | FTP サーバーのホスト名。 |
| `auth.params.username` | FTP サーバーに関連付けられたユーザー名。 |
| `auth.params.password` | FTP サーバーに関連付けられたパスワード。 |
| `connectionSpec.id` | FTP サーバー接続の仕様 ID: `fb2e94c9-c031-467d-8103-6bd6e0a432f2` |

**応答**

正常な応答は、一意の識別子 (`id`) をクリックします。 この ID は、次のチュートリアルで FTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルでは、 [!DNL Flow Service] API で、接続の一意の ID 値を取得している。 この接続 ID を [フローサービス API を使用したクラウドストレージの調査](../../explore/cloud-storage.md).
