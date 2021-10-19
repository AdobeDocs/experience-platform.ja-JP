---
keywords: エクスペリエンス Platform、home、人気のある話題。SFTP、sftp、およびセキュリティで保護されたファイル転送プロトコル。セキュリティで保護されたファイル転送プロトコル
solution: Experience Platform
title: フローサービス API を使用した SFTP ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Adobe エクスペリエンスプラットフォームを SFTP (セキュアファイル転送プロトコル) サーバーに接続する方法について説明します。
exl-id: b965b4bf-0b55-43df-bb79-c89609a9a488
source-git-commit: 13bd1254dfe89004465174a7532b4f6aaef54c09
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 7%

---

# API を使用した SFTP ベース接続の作成 [!DNL Flow Service]

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、 [!DNL SFTP] API を使用した (セキュアなファイル転送プロトコル) の基本的な接続を作成する手順について説明し [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース: エクスペリエンスプラットフォームを使用すると、 ](../../../../home.md) 多種多様なソースからデータを ingested することができます。また、プラットフォームサービスを使用して、着信データを構造化、ラベル付け、拡張する機能が提供されます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

>[!IMPORTANT]
>
>JSON オブジェクトを ingesting 接続で使用する場合は、改行文字または改行文字を使用しないようにすることをお勧め [!DNL SFTP] します。 この制限を回避するには、1行に1つの JSON オブジェクトを使用し、後続ファイルには複数行を使用します。

以下の各セクションでは、 [!DNL SFTP] API を使用してサーバーに接続するために必要な追加情報を記載して [!DNL Flow Service] います。

### 必要な資格情報の収集

に接続するには [!DNL Flow Service] [!DNL SFTP] 、次の接続プロパティの値を指定する必要があります。

| Chap | 説明 |
| ---------- | ----------- |
| `host` | サーバーに関連付けられた名前または IP アドレスを指定し [!DNL SFTP] ます。 |
| `port` | 接続する SFTP サーバーポートを指定します。 指定されていない場合は、デフォルト値の「」が表示され `22` ます。 |
| `username` | サーバーへのアクセスを許可するユーザー名 [!DNL SFTP] です。 |
| `password` | サーバーのパスワードを指定 [!DNL SFTP] します。 |
| `privateKeyContent` | Base64 でエンコードされた SSH 秘密キーの内容。 OpenSSH キーの種類は、RSA または DSA に分類されていなければなりません。 |
| `passPhrase` | キーファイルまたはキーの内容がパスフレーズによって保護されている場合に、秘密キーを復号化するためのパスフレーズまたはパスワード。 がパスワードで保護されている場合は、 `privateKeyContent` 値として秘密キーのコンテンツのパスフレーズを使用する必要があります。 |
| `connectionSpec.id` | コネクション仕様は、ベースおよびソース接続の作成に関連付けられた認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL SFTP] は、次のとおりです `b7bf2577-4520-42c9-bae9-cad01560f7bc` 。 |

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、そのエンドポイントに POST 要求を行います。この場合は、 `/connections` [!DNL SFTP] 要求パラメーターの一部として認証資格情報を指定します。

### [!DNL SFTP]基本認証を使用したベース接続の作成

[!DNL SFTP]基本認証を使用して基本的な接続を作成するには、API に対して POST 要求を行い、 [!DNL Flow Service] 接続の、、およびに値を指定 `host` `userName` `password` します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求は、基本認証を使用するための基本的な接続を作成し [!DNL SFTP] ます。

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
| `auth.params.username` | SFTP サーバーに関連付けられたユーザー名です。 |
| `auth.params.password` | SFTP サーバーに関連付けられたパスワードを指定します。 |
| `connectionSpec.id` | SFTP サーバーの接続条件 ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

応答が成功した場合は、新しく作成された接続の一意の識別子 () が返され `id` ます。 この ID は、次のチュートリアルで SFTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

### [!DNL SFTP]SSH 公開鍵認証を使用したベース接続の作成

[!DNL SFTP]SSH 公開鍵認証を使用したベース接続を作成するには、API への POST 要求を行い、接続の、、、 [!DNL Flow Service] およびに値を指定 `host` `userName` `privateKeyContent` `passPhrase` します。

>[!IMPORTANT]
>
>コネクタは、 [!DNL SFTP] RSA または DSA タイプ OpenSSH キーをサポートします。 キーファイルの内容が `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"` で始まり、で終わることを確認してください `"-----END [RSA/DSA] PRIVATE KEY-----"` 。 秘密キーファイルが PPK フォーマットのファイルである場合は、PuTTY ツールを使用して PPK フォーマットから OpenSSH フォーマットに変換する必要があります。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求によっ [!DNL SFTP] て、SSH 公開鍵認証を使用するための基本的な接続が作成されます。

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
| `auth.params.host` | サーバーのホスト名を指定 [!DNL SFTP] します。 |
| `auth.params.username` | サーバーに関連付けられたユーザー名 [!DNL SFTP] 。 |
| `auth.params.privateKeyContent` | Base64 でエンコードされた SSH 秘密キーの内容。 OpenSSH キーの種類は、RSA または DSA に分類されていなければなりません。 |
| `auth.params.passPhrase` | キーファイルまたはキーの内容がパスフレーズによって保護されている場合に、秘密キーを復号化するためのパスフレーズまたはパスワード。 PrivateKeyContent がパスワードで保護されている場合は、値として PrivateKeyContent のパスフレーズを使用する必要があります。 |
| `connectionSpec.id` | [!DNL SFTP]サーバーの接続条件 ID。`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

応答が成功した場合は、新しく作成された接続の一意の識別子 () が返され `id` ます。 この ID は [!DNL SFTP] 、次のチュートリアルでサーバーを表示するために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルでは、 [!DNL SFTP] API を使用して接続を作成 [!DNL Flow Service] し、接続の一意の ID 値を取得しました。 この接続 ID を使用し [ て、フローサービス API を使用してクラウドストレージを探すことができ ](../../explore/cloud-storage.md) ます。
