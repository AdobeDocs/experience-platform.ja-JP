---
keywords: Experience Platform；ホーム；人気の高いトピック；SFTP;SFTP；セキュアファイル転送プロトコル；セキュアファイル転送プロトコル
solution: Experience Platform
title: Flow Service APIを使用したSFTPソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience PlatformをSFTP（セキュアファイル転送プロトコル）サーバーに接続する方法を説明します。
exl-id: b965b4bf-0b55-43df-bb79-c89609a9a488
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '850'
ht-degree: 21%

---

# [!DNL Flow Service] APIを使用したSFTPソース接続の作成

このチュートリアルでは、[!DNL Flow Service] APIを使用して、Experience PlatformをSFTP(Secure File Transfer Protocol)サーバに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

>[!IMPORTANT]
>
>SFTPソース接続でJSONオブジェクトを取り込む場合は、改行やキャリッジリターンを避けることをお勧めします。 この制限を回避するには、1行に1つのJSONオブジェクトを使用し、続くファイルには複数行を使用します。

[!DNL Flow Service] APIを使用してSFTPサーバーに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]がSFTPに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | SFTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | SFTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | SFTPサーバーのパスワードです。 |
| `privateKeyContent` | Base64エンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| `passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のデータフローを作成する場合に使用できる接続は1つだけです。

### 基本認証を使用したSFTP接続の作成

基本的な認証を使用してSFTP接続を作成するには、[!DNL Flow Service] APIにPOSTリクエストを行い、接続の`host`、`userName`および`password`に値を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

SFTP接続を作成するには、一意の接続指定IDをPOSTリクエストの一部として指定する必要があります。 SFTPの接続指定IDは`b7bf2577-4520-42c9-bae9-cad01560f7bc`です。

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
| `auth.params.host` | SFTPサーバーのホスト名です。 |
| `auth.params.username` | SFTPサーバーに関連付けられているユーザー名です。 |
| `auth.params.password` | SFTPサーバーに関連付けられているパスワードです。 |
| `connectionSpec.id` | SFTPサーバー接続仕様ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新たに作成された接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでSFTPサーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

### SSH公開鍵認証を使用したSFTP接続の作成

SSH公開鍵認証を使用してSFTPPOSTを作成するには、[!DNL Flow Service]、`userName`、`privateKeyContent`および`passPhrase`の値を指定しながら、`host` APIに接続要求を行います。

>[!IMPORTANT]
>
>SFTPコネクタは、RSAまたはDSAタイプのOpenSSHキーをサポートします。 `"-----BEGIN [RSA/DSA] PRIVATE KEY-----"`で終わり`"-----END [RSA/DSA] PRIVATE KEY-----"`で終わる主要なファイルコンテンツ開始を確認してください。 秘密鍵ファイルがPPK形式のファイルの場合は、PuTTYツールを使用してPPK形式からOpenSSH形式に変換します。

**API 形式**

```http
POST /connections
```

**リクエスト**

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
| `auth.params.host` | SFTPサーバーのホスト名です。 |
| `auth.params.username` | SFTPサーバーに関連付けられているユーザー名です。 |
| `auth.params.privateKeyContent` | Base64エンコードされたSSH秘密鍵のコンテンツ。 OpenSSHキーのタイプは、RSAまたはDSAに分類する必要があります。 |
| `auth.params.passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |
| `connectionSpec.id` | SFTPサーバー接続仕様ID:`b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新たに作成された接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでSFTPサーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用してSFTP接続を作成し、接続の固有のID値を取得したことになります。 この接続IDを使用して、Flow Service API](../../explore/cloud-storage.md)または[Flow Service API](../../cloud-storage-parquet.md)を使用してParketストレージを[調査できます。
