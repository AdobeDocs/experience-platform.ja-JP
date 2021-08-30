---
keywords: Experience Platform；ホーム；人気のあるトピック；SFTP;sftp；セキュアファイル転送プロトコル；セキュアファイル転送プロトコル
solution: Experience Platform
title: フローサービスAPIを使用したSFTPベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをSFTP（セキュアファイル転送プロトコル）サーバーに接続する方法を説明します。
exl-id: b965b4bf-0b55-43df-bb79-c89609a9a488
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '794'
ht-degree: 7%

---

# [!DNL Flow Service] APIを使用したSFTPベース接続の作成

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL SFTP]（セキュアファイル転送プロトコル）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

>[!IMPORTANT]
>
>ソース接続[!DNL SFTP]でJSONオブジェクトを取り込む際には、改行やキャリッジリターンを避けることをお勧めします。 この制限を回避するには、1行に1つのJSONオブジェクトを使用し、その後のファイルに複数行を使用します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL SFTP]サーバーに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL SFTP]に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL SFTP]サーバーに関連付けられている名前またはIPアドレス。 |
| `username` | [!DNL SFTP]サーバーへのアクセス権を持つユーザー名。 |
| `password` | [!DNL SFTP]サーバーのパスワード。 |
| `privateKeyContent` | Base64でエンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| `passPhrase` | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワード。 `privateKeyContent`がパスワードで保護されている場合は、秘密鍵コンテンツのパスフレーズを値として使用する必要があります。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL SFTP]の接続仕様IDは次のとおりです。`b7bf2577-4520-42c9-bae9-cad01560f7bc`. |

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL SFTP]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

### 基本認証を使用した[!DNL SFTP]接続の作成

基本的なPOSTを使用して[!DNL SFTP]ベース接続を作成するには、接続の`host`、`userName`および`password`に値を指定しながら、[!DNL Flow Service] APIに対して認証リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、基本認証を使用して[!DNL SFTP]のベース接続を作成します。

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
| `auth.params.host` | SFTPサーバーのホスト名。 |
| `auth.params.username` | SFTPサーバーに関連付けられているユーザー名。 |
| `auth.params.password` | SFTPサーバーに関連付けられたパスワード。 |
| `connectionSpec.id` | SFTPサーバー接続仕様ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新しく作成された接続の一意の識別子(`id`)を返します。 このIDは、次のチュートリアルでSFTPサーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

### SSH公開鍵認証を使用した[!DNL SFTP]接続の作成

SSH公開鍵POSTを使用して[!DNL SFTP]ベース接続を作成するには、接続の`host`、`userName`、`privateKeyContent`および`passPhrase`に値を指定しながら、[!DNL Flow Service] APIに認証リクエストを送信します。

>[!IMPORTANT]
>
>[!DNL SFTP]コネクタは、RSAまたはDSAタイプのOpenSSHキーをサポートします。 キーファイルの内容が`"-----BEGIN [RSA/DSA] PRIVATE KEY-----"`で始まり、`"-----END [RSA/DSA] PRIVATE KEY-----"`で終わることを確認します。 秘密鍵ファイルがPPK形式のファイルである場合は、PuTTYツールを使用して、PPKからOpenSSH形式に変換します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、SSH公開鍵認証を使用して[!DNL SFTP]のベース接続を作成します。

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
| `auth.params.host` | [!DNL SFTP]サーバーのホスト名。 |
| `auth.params.username` | [!DNL SFTP]サーバーに関連付けられているユーザー名。 |
| `auth.params.privateKeyContent` | Base64でエンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| `auth.params.passPhrase` | キーファイルまたはキーコンテンツがパスフレーズで保護されている場合に秘密鍵を復号化するパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合、このパラメーターをPrivateKeyContentのパスフレーズと共に値として使用する必要があります。 |
| `connectionSpec.id` | [!DNL SFTP]サーバー接続仕様ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新しく作成された接続の一意の識別子(`id`)を返します。 このIDは、次のチュートリアルで[!DNL SFTP]サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL SFTP]接続を作成し、接続の一意のID値を取得しました。 この接続IDを使用して[フローサービスAPI](../../explore/cloud-storage.md)または[フローサービスAPI](../../cloud-storage-parquet.md)を使用してParquetデータを取り込み、クラウドストレージを調べることができます。
