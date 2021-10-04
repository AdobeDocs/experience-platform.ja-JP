---
keywords: Experience Platform；ホーム；人気のあるトピック；SFTP;SFTP；セキュアファイル転送プロトコル；セキュアファイル転送プロトコル
solution: Experience Platform
title: フローサービス API を使用した SFTP ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを SFTP(Secure File Transfer Protocol) サーバーに接続する方法を説明します。
exl-id: b965b4bf-0b55-43df-bb79-c89609a9a488
source-git-commit: 9ad09fba3119b631576f22574a2151c74f91e07b
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 7%

---

# [!DNL Flow Service] API を使用した SFTP ベース接続の作成

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL SFTP]（セキュアファイル転送プロトコル）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

>[!IMPORTANT]
>
>[!DNL SFTP] ソース接続を持つ JSON オブジェクトを取り込む場合は、改行やキャリッジリターンを避けることをお勧めします。 この制限を回避するには、1 行に 1 つの JSON オブジェクトを使用し、その後のファイルに複数行を使用します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL SFTP] サーバーに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL SFTP] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL SFTP] サーバーに関連付けられている名前または IP アドレス。 |
| `port` | 接続先の SFTP サーバーポート。 指定しない場合、値のデフォルトは `22` です。 |
| `username` | [!DNL SFTP] サーバーへのアクセス権を持つユーザー名。 |
| `password` | [!DNL SFTP] サーバーのパスワード。 |
| `privateKeyContent` | Base64 でエンコードされた SSH 秘密鍵のコンテンツ。 OpenSSH キーのタイプは、RSA または DSA に分類する必要があります。 |
| `passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワード。 `privateKeyContent` がパスワードで保護されている場合は、秘密鍵コンテンツのパスフレーズを値として使用する必要があります。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL SFTP] の接続仕様 ID は次のとおりです。`b7bf2577-4520-42c9-bae9-cad01560f7bc`. |

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL SFTP] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

### 基本認証を使用した [!DNL SFTP] ベース接続の作成

基本的なPOSTを使用して [!DNL SFTP] ベース接続を作成するには、接続の `host`、`userName` および `password` に値を指定しながら、[!DNL Flow Service] API に対して認証リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、基本認証を使用して [!DNL SFTP] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
        "name": "SFTP connector with password",
        "description": "SFTP connector password",
        "auth": {
            "specName": "Basic Authentication for sftp",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | SFTP サーバーのホスト名。 |
| `auth.params.username` | SFTP サーバーに関連付けられているユーザー名。 |
| `auth.params.password` | SFTP サーバーに関連付けられたパスワード。 |
| `connectionSpec.id` | SFTP サーバー接続仕様 ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新しく作成された接続の一意の識別子 (`id`) を返します。 この ID は、次のチュートリアルで SFTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

### SSH 公開鍵認証を使用した [!DNL SFTP] ベース接続の作成

SSH 公開鍵POSTを使用して [!DNL SFTP] ベース接続を作成するには、接続の `host`、`userName`、`privateKeyContent` および `passPhrase` に値を指定しながら、[!DNL Flow Service] API に認証リクエストを送信します。

>[!IMPORTANT]
>
>[!DNL SFTP] コネクタは、RSA または DSA タイプの OpenSSH 鍵をサポートします。 キーファイルの内容が `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` で始まり、`"-----END [RSA/DSA] PRIVATE KEY-----"` で終わることを確認します。 秘密鍵ファイルが PPK 形式のファイルの場合は、PuTTY ツールを使用して PPK 形式から OpenSSH 形式に変換します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、SSH 公開鍵認証を使用して [!DNL SFTP] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "SFTP connector with SSH authentication",
        "description": "SFTP connector with SSH authentication",
        "auth": {
            "specName": "SSH PublicKey Authentication for sftp",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "privateKeyContent": "{PRIVATE_KEY_CONTENT}",
                "passPhrase": "{PASSPHRASE}"
            }
        },
        "connectionSpec": {
            "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | [!DNL SFTP] サーバーのホスト名。 |
| `auth.params.username` | [!DNL SFTP] サーバーに関連付けられているユーザー名。 |
| `auth.params.privateKeyContent` | Base64 でエンコードされた SSH 秘密鍵のコンテンツ。 OpenSSH キーのタイプは、RSA または DSA に分類する必要があります。 |
| `auth.params.passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合、このパラメーターは PrivateKeyContent のパスフレーズと共に値として使用する必要があります。 |
| `connectionSpec.id` | [!DNL SFTP] サーバ接続仕様 ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新しく作成された接続の一意の識別子 (`id`) を返します。 この ID は、次のチュートリアルで [!DNL SFTP] サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL SFTP] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID を使用して [ フローサービス API](../../explore/cloud-storage.md) または [ フローサービス API](../../cloud-storage-parquet.md) を使用して Parquet データを取り込み、クラウドストレージを調べることができます。
